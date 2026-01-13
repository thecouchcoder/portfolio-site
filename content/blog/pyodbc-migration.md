+++
title = 'Understanding Python’s DB API 2.0 Through a Real Migration'
date = 2026-01-13T15:30:59-06:00
+++

## Intro

We recently migrated away from a homegrown SQL Server library that implemented Python’s DB API 2.0 and moved to PyODBC, another DB API 2.0–compliant module built on top of ODBC. Even though both libraries adhered to the same standard, I realized during the migration that I didn’t fully understand what guarantees the spec actually provides or how PyODBC interprets and extends it in practice.

To make the migration successful, I dove into Python’s DB API 2.0 (PEP 249), cross-referencing the specification with PyODBC’s behavior to understand how connections, cursors, transactions, parameter binding, and exceptions are _supposed_ to work. This post shares what I learned—and how that understanding shaped our migration.

---

## Motivation

Our homegrown library only supported up to Python 3.9. Since Python 3.9 is now EOL, we wanted to upgrade—but couldn’t until that library was updated. None of the original authors still work at the company, and continuing to maintain a bespoke database layer didn’t feel like a good long-term investment.

Instead, I decided it was worth migrating to something well-supported and open source. I chose PyODBC because it is mature, widely used, and keeps the door open for a future migration to SQLAlchemy’s ORM if we decide to go that route.

---

## What is DB API 2.0?

The key thing to understand about DB API 2.0 is that it doesn’t contain any code. It doesn’t connect to a database, parse SQL, or manage transactions. Instead, **it’s a specification** - a set of rules that database libraries agree to follow so Python applications can interact with different databases in a consistent way.

A simple example makes this clear.

Imagine you’re building an application using MariaDB and you pick a Python library that is _not_ DB API 2.0 compliant. In that library, creating a connection looks like this:

`create_connection(connection_string)`

Later, you switch to Postgres and find another library—also not DB API 2.0 compliant—that expects:

`build_connection(connection_string)`

Even though the concept is the same (“connect to the database”), your application code now has to change. And that’s just connection creation. Imagine rewriting every `execute`, `fetch`, and `cursor` call too.

DB API 2.0 solves this by defining a common interface that compliant libraries must implement. For example, the spec requires a top-level function named `connect()` that accepts the parameters needed for a database connection. So with a DB API 2.0–compliant library for MariaDB, you write:

`connect(connection_string)`

And when you switch to a Postgres library that also implements DB API 2.0, you still write:

`connect(connection_string)`

Even though the underlying database and implementation have changed, **your application code stays the same**.

That’s the power of DB API 2.0: a shared contract that makes Python database libraries more predictable and portable—so you can focus on your application instead of rewriting boilerplate.

---

## A Mental Model for Database Calls

When application code talks to a database, it isn’t connecting directly. There are a few layers in between, and each one has a clear responsibility.

At the bottom is **the database** itself (SQL Server). Databases don’t understand Python—they speak their own low-level network protocols.

Above that is the **database driver**. The driver is native code that knows how to speak the database’s protocol. In our case, this is the **SQL Server ODBC driver**. Without a driver, Python would have no way to communicate with the database at all.

Next is the **driver manager**. On Unix systems, this is **unixODBC**. The driver manager doesn’t know anything about SQL—it simply:

- Loads the correct driver
- Forwards calls to it

**PyODBC** sits on top of unixODBC. It provides a Python-friendly interface and implements Python’s DB API 2.0, translating Python calls into ODBC calls—but it does not talk to SQL Server directly.

Finally, **application code** sits at the top and uses the functions from PyODBC without needing to care which driver or database is underneath.

In our setup, the full stack looks like this:

> **Application → PyODBC → unixODBC → SQL Server ODBC driver → SQL Server**

---

## Key Points for the Migration

### 1. Parameter Style (`paramstyle`)

DB API 2.0 allows libraries to choose how SQL parameters are represented. This seemingly small detail can have big migration implications.

- **Our old library used numeric parameters (`:1`, `:2`, etc.)**
- **PyODBC uses qmark parameters (`?`)**

Example:

`-- Old library SELECT * FROM users WHERE id = :1  -- PyODBC SELECT * FROM users WHERE id = ?`

In our case, this was significant in theory but fairly trivial in practice. We mostly execute stored procedures and had a single function responsible for converting a dictionary of parameters into a stored procedure call. Updating that one function solved the problem everywhere:

`def _callproc(cursor: pyodbc.Cursor, stored_procedure: str, params: Dict[str, Any]):     param_names = ", ".join([f"@{key}=?" for key in params.keys()])     param_values = list(params.values())     query = f"EXEC {stored_procedure} {param_names}"     cursor.execute(query, param_values)`

If your codebase has lots of inline SQL, though, this can be one of the most painful migration points.

---

### 2. Transaction Behavior (autocommit)

The DB API spec requires `commit()` and `rollback()`, and assumes transactions are **off by default**. Our old library was configured to enable autocommit, so we needed to ensure PyODBC matched that behavior.

PyODBC allows this to be configured via the connection string, which let us preserve existing semantics without rewriting business logic.

---

### 3. Connection Pooling

DB API 2.0 doesn’t define pooling at all—so every library handles it differently.

- **Our old library implemented application-level pooling**
- **PyODBC does not provide pooling itself**, but can rely on:
  - ODBC driver–level pooling (`pyodbc.pooling = True`)
  - A third-party Python pooling solution (e.g., DBUtils or SQLAlchemy)

Our plan is to use SQLAlchemy solely for connection pooling, without adopting its ORM.

---

### 4. Async Support

Our old library was natively async; PyODBC is synchronous. Calling it directly from an async API would block the event loop.

To avoid this, we execute database calls in a thread:

`async def callproc(self, stored_procedure: str, **kwargs) -> None:     await asyncio.to_thread(self.sync_db.callproc, stored_procedure, **kwargs)`

Python’s default thread pool allows up to 32 concurrent threads. If that becomes a concern, we can layer in concurrency controls like a semaphore.

---

### 5. Column Access

DB API 2.0 only requires column access by index:

`row[0]`

Most libraries add conveniences on top. Our old library allowed access by index, name, _or attribute_. PyODBC supports index and name—but not attribute access:

`print(row.Version) print(row[0]) print(row['Version'])  # Raises an exception in PyODBC`

This turned out to be trivial to fix: we had only a few call sites using attribute access, and switching them to name-based access was straightforward.

---

### 6. UUIDs

Our old library returned `SQL_GUID` values as `uuid.UUID` objects. PyODBC returns them as strings by default, which caused unit test failures and could have introduced subtle bugs.

The fix was simple:

`pyodbc.native_uuid = True`

This restores typed UUID behavior and keeps application semantics unchanged.

---

### 7. Timeouts

Our old library supported both:

- Connection (login) timeouts
- Per-query execution timeouts

PyODBC supports both as well, but the configuration is a bit under-documented.

- Connection timeout is passed to `connect(...)`
- Query timeout is set via `conn.timeout` on the connection object

Because SQLAlchemy manages our connections, we pass the login timeout via `connect_args`:

`engine = create_engine(     ...     connect_args={         "timeout": config.login_timeout,     },     ... )`

And we use a connection listener to set the query timeout on every connection:

`@listens_for(engine, "connect") def set_query_timeout(dbapi_conn, connection_record):     dbapi_conn.timeout = config.timeout`

---

### 8. Exceptions

DB API 2.0 defines a consistent exception hierarchy, which is one of its strongest features. The key rule:

> **Every DB API exception inherits from `Error`.**

That means you can safely write:

`try:     cursor.execute(...) except Error:     ...`

Both our old library and PyODBC follow this contract, making this part of the migration refreshingly boring—which is exactly what standards are supposed to do.

## Lessons Learned

1. **DB API 2.0 is a contract, not a guarantee of identical behavior**  
   The specification defines _what_ must exist, not _how_ it must behave. Details like parameter styles, pooling, async support, and type conversions are intentionally left to individual implementations—and those differences matter during migrations.
2. **Standards enable boring migrations—and that’s a good thing**  
   Large parts of this migration didn’t require changes because both libraries honored the same exception hierarchy, cursor semantics, and transaction model. When a standard works, it fades into the background.
3. **Understanding the spec makes debugging faster**  
   Reading PEP 249 clarified what behavior was guaranteed by the standard versus what was library-specific. That distinction made it much easier to reason about unexpected behavior and choose the right fixes.
4. **“Just use the standard” isn’t enough—read it**  
   We thought we understood DB API 2.0 because we were already using it. In reality, we were relying on undocumented behavior. Reading the spec turned implicit assumptions into explicit decisions—and made the migration far smoother.

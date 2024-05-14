+++
title = 'SQLite Without CGO'
date = 2024-05-14T16:57:21-05:00
+++

# Introduction
Have you ever been working within a Go project that uses a SQLite database, and been met with the error `Binary was compiled with 'CGO_ENABLED=0', go-sqlite3 requires cgo to work. This is a stub` OR `cgo: C compiler "gcc" not found: exec: "gcc": executable file not found`?  This issue occurs commonly on Windows machines when using the popular [mattn/go-sqlite3 package](https://pkg.go.dev/github.com/mattn/go-sqlite3).  But what causes it and how do we fix it?  Read on to find out.

# What's Going On?
Cgo allows us to write Gocode that calls C code.  This is great because any library that already exists in C can be seamlessly incorporated in a Go app.  However to compile your code you need a GCC compiler - a compiler that can compile C code.  When we look at the documentation for mattn/go-sqlite3 we can [clearly see](https://github.com/mattn/go-sqlite3?tab=readme-ov-file#installation) this is a CGO package and needs a GCC compiler installed.  This means that the library is not a pure port of SQLite to Go.  Instead it is a wrapper around the C SQLite library. While we could go through the effort of installing GCC on Windows, is that our only option?

# Moving Forward
Enter the [modernc.org/sqlite package](https://pkg.go.dev/modernc.org/sqlite) - a CGO free port of SQLite.  With this package you can install and use SQLite without a GCC compiler.  The best part is that this package is a drop in replacement for mattn/go-sqlite3.  How is that possible?  Well, both mattn/go-sqlite3 and modernc.org/sqlite conform the the Go standard `database/sql` interface, so as long as you are only using `database/sql` in your code then you can simply swap these libraries in and out!

# Conclusion
In this post, we've examined the complexities involved in using the popular SQLite package, mattn/go-sqlite3.  Often when using SQLite we are prototyping small projects and may not necessarily want to go through the effort of installing a GCC compiler. Fortunately, in that case we can utilize the SQLite package created by modernc.  By providing a pure Go port of SQLite, we can prototype our project without ever needing to worry about CGO or GCC.


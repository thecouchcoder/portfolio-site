+++
title = 'Implicit Type Declarations'
date = 2024-10-14T07:59:53-05:00
+++

# Intro

Here's a fun problem: What would you expect **each** of these snippets to output?

```Python
my_var = 42
def inner():
    my_var = 24
    print(my_var)
inner()
print(my_var)
```

```JavaScript
myVar = 42;
function inner() {
  myVar = 24;
  console.log(myVar);
}
inner();
console.log(myVar);
```

# Answer

Python -> 24 42
Javscript -> 24 24

# Why

Python and Javascript are both languages that use implicit variable declaration. That's just a fancy way of saying there's not a separate syntax for declaring a new variable vs assigning to an existing variable (JS does have let, but it's not required).
When a language uses implicit variable declaration the language designers have to decide how the language should behave when a statement is ambiguous because it's assigning to a variable that already exists in the outer scope.
Python and JS treat this differently.
Python chose to make all inner scope variables DECLARE a new variable in the inner function scope, so it does NOT overwrite the outer scope variable. Hence why we get 24 and 42
JS on the other hand chose to make inner scope variables always ASSIGN if the variable exists in the outer scope. That's why with JS we get 24 24

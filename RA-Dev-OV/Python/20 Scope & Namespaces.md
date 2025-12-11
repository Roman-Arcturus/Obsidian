A deep conceptual foundation for how Python _resolves names_, _creates and destroys variables_, and _manages separate logical domains of identifiers_.

This topic is essential for:
- Understanding why some variables “disappear” outside functions.
- Diagnosing bugs where you “shadow” names accidentally.
- Using closures and decorators competently.
- Avoiding unintended side effects.

# 1. What Is a _Namespace_?

A **namespace** is a mapping (dictionary-like structure) that associates **names** → **objects**.

Examples of namespaces:

- The **global namespace** of a module  
    Contains variables defined at the top level of a file.
    
- The **local namespace** of a function  
    Created when the function begins execution.
    
- The **built-in namespace**  
    Contains names like `len`, `str`, `print`, `Exception`, etc.

Conceptually: `name → object` 
Each namespace is separate. A name in one does not automatically exist in another.


# 2. Scope: The Region of Code Where a Name Is Visible

**Scope** = the textual area of the program where a name can be referenced.

Python follows the standard **LEGB rule** to resolve variable names:
1. **Local** (L): inside the current function block
2. **Enclosing** (E): local scopes of outer functions (for closures)
3. **Global** (G): module-level names
4. **Built-in** (B): Python-provided names

Example illustrating LEGB:
```
x = "global"

def outer():
    x = "enclosing"
    def inner():
        x = "local"
        print(x)
    inner()

outer()
# output is --> local
```
Because the inner function had its own `x`.

# 3. Local Scope and Lifetime

A function call creates a new **local namespace**, which:
- is created when the function begins execution 
- is destroyed when the function returns (unless captured by a closure)

Example:
```
def f():
    x = 10  # local
    return x

print(x)  # error: name 'x' is not defined
```
`x` exists only while `f()` runs.

# 4. Global Scope and the `global` Keyword

A variable defined at the top level of a module is **global to that module** (not global to all modules—Python has **module-level** globals).

You can _rebind_ a global variable from inside a function only with `global`:
```
count = 0

def increment():
    global count
    count += 1
```
Use sparingly — it makes code harder to reason about and test.

# 5. Enclosing Scope and the `nonlocal` Keyword

Occurs when you have nested functions.

Example:
```
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 1
        return x
    return inner()
    
print( outer() ) # 11
```
`nonlocal` allows modification of a variable that lives in an _outer function’s_ scope, not global scope.

# 6. Built-In Scope

The final fallback if all others fail.

Example:
```
len  # found in built-in namespace

# Avoid shadowing built-ins:

len = 5  # bad idea: shadows built-in name

```

# 7. Name Shadowing

When a name in a more local scope hides a name in an outer scope.

Example:
```
x = 99

def f():
    x = 1  # shadows global 'x'
    print(x)

f()
print(x)  # 99

```

# 8. Closures and Extended Lifetimes

Variables normally die when a function ends.

**BUT**, if referenced by an inner function, the outer variable is kept alive.

Example:
```
def make_counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc

c = make_counter()
print(c())  # 1
print(c())  # 2

```
Here, `count` survives because the closure references it.

This is how decorators and stateful functions work.

# 9. Practical Example Combining All Concepts

```
x = "global"

def outer():
    y = "enclosing"

    def inner():
        z = "local"
        print(x, y, z)

    inner()

outer()

# global enclosing local
```

# 10. Common Pitfalls

## Pitfall 1: Modifying a variable without `global` or `nonlocal`
```
counter = 0

def bad():
    counter += 1  # error: local variable referenced before assignment

```
Why?  
Python sees `counter += 1` and decides `counter` is a _local_ variable (assignment in function → local). But it was never defined locally.

Fix with either: `global counter`
OR restructure code so mutation happens on a mutable object, not rebinding.

## Pitfall 2: Mutable defaults + scope confusion

You already addressed this earlier, but it's tied to scope because the default argument lives in the **function definition scope**, not call scope.

# Quick Summary Table

|Scope Type|Created When|Destroyed When|Typical Usage|
|---|---|---|---|
|Local|Function call begins|Function returns|Function variables|
|Enclosing|Outer function is defined|When no references remain|Closures|
|Global|Module is imported / executed|Program exit|Module-level state|
|Built-in|Python interpreter starts|Never (until process ends)|Built-ins|

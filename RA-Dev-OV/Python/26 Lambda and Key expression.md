# **Lambda Expressions & Key Functions**

This topic is essential because lambdas appear everywhere in real Python: sorting, filtering, transformations, callbacks, deduplication, grouping, and lightweight functional pipelines.

# 1. What It Is

A **lambda expression** in Python is an inline, anonymous function.  
It behaves like a regular function created with `def`, but:

- it has no name
- it consists of exactly one expression
- it is used where a short, one-off function is needed

Example:
```
lambda x: x * 2
```
This represents a function that takes `x` and returns `x*2`.


# 2. Why It’s Useful

Lambdas eliminate the need to create many small “helper functions” when:
- sorting a list of complex objects
- selecting values using `min()` / `max()`
- mapping a transformation in a tight scope
- defining quick filters
- building pipelines
- manipulating nested structures inline

They reduce boilerplate and increase locality of logic: the transformation is expressed _next to_ the operation that uses it.

Real-world examples:
```
sorted(users, key=lambda u: u["name"])
max(users, key=lambda u: u["meta"]["score"])
filter(lambda u: u["active"], users)
```

# 3. Unified Mental Model

Think of a **lambda** as a micro-tool you craft instantly to describe _how to look inside a structure_.  
They don’t add new capabilities — they only make existing operations (sorting, selecting, mapping) _target the right field_.

A lambda is essentially:
```
A small function whose only purpose is:
“Extract or transform some property of this element at this specific moment.”
```

Whenever you use `key=` in Python, you are describing “how to measure something,” and lambdas are the ideal tool.

---

# Exercises

These follow directly from your progression and build on your existing skills with nested data.

You may assume the following input for all exercises:
```
users = [
    {"name": "Alice", "active": True,  "meta": {"score": 10}},
    {"name": "Bob",   "active": False, "meta": {"score": 5}},
    {"name": "Kevin", "active": True,  "meta": {"score": 7}},
]
```

### **Exercise 1 — Sorting by Score**

Sort users by score (ascending) using `sorted()` and a lambda.

Expected output structure:


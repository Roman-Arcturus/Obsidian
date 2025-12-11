We extend the foundational model:
```
OUTPUT_EXPRESSION  
    ← for ITEM in SEQUENCE  
        ← optional FILTER
```

with two advanced capabilities:
1. **Inline conditional expressions (ternary operations)**
2. **Nested comprehensions as structured transformations**

These are essential for writing _idiomatic_, clean Python data pipelines.

# **1. Inline Conditional Expressions**

You already used:
```
... for x in seq if condition
```
That is a _filter_.

Now we add transformation logic:
```
expression_if_true if condition else expression_if_false
```
Embedded directly in the **output expression**, not the filter.

### Example

Transform numbers:
- if positive → use `n`
- if negative → use `0`
```
normalized = [n if n > 0 else 0 for n in nums]
```





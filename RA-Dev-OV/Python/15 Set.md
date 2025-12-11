# Sets — Unordered Collections of Unique Elements

A **set** is a mutable, unordered collection of **unique** items.
### What they are
- A **hash-based**, **unordered** collection with **no duplicates**.
- Membership testing (`in`) is O(1).
### Why useful
- Removing duplicates.
- Fast membership checking.
- Mathematical set operations.

Example:
```
s = {1, 2, 3}
s = set([1, 2, 3, 3])   # duplicates removed
```

Key properties:
1. **No duplicates** — automatically removed 
2. **Unordered** — no indexing, no positions
3. **Mutable** — you can add/remove items
4. **Fast membership tests** — O(1) average
5. **Elements must be immutable** — like dictionary keys

Sets are used heavily in professional Python code whenever you need to check presence, perform deduplication, or do mathematical set operations.

## 1. When to Use a Set

Use a set when your main questions are:
- “Is this item already here?”
- “Give me only unique items.”
- “I need fast membership checks.”
- “I need mathematical operations: union, intersection, difference.”

Examples:
- Removing duplicates from large datasets
- Checking if something has been seen before
- Fast lookup tables
- Membership validation
- Graph algorithms, search algorithms

## 2. Creating Sets

**Empty set**
```
items: set[int] = set()
```
Why not `{}`?  
Because `{}` creates a dictionary.

**Literal**
```
fruits = {"apple", "banana", "orange"}
```

## 3. Adding and Removing Elements
```
fruits.add("papaya")
```

**Remove (fails if not present)**
```
items.remove(10)
```

**Safe remove**
```
items.discard(10)
```
Use `.discard()` when you don’t want exceptions.

## 4 Membership Tests (sets are the fastest)

```
if 5 in items:     ...
```

This is the main reason sets exist:  
**membership operations are extremely fast** because sets use hash tables.

## 5. Set Operations (very important in real projects)

Python sets support mathematical set operations.

**Union**
```
a | b
```

**Intersection**
```
a & b
```

**Difference**
```
a - b
```

**Symmetric difference (elements only in one set)**
```
a ^ b
```

These are used constantly in data processing, validation logic, and algorithms.
Examples:
```
common = allowed_ids & requested_ids
missing = required_ids - provided_ids
```

## 6. Iterating Over Sets

```
for item in items:
	print(item)
```

Order is arbitrary — do not rely on it.

## 7. Copying Sets
```
copy_set = items.copy()
```

## 8 Why Sets Are Useful for Learning Python

Sets force you to understand:
- hashability (only immutable types allowed)
- the difference between order and non-order
- mutability rules
- membership-based algorithms
- efficient data structures

They also complement lists and dictionaries:

- lists → ordered, allow duplicates
- dictionaries → key-value mapping
- sets → unique membership collections

# Summary Before We Proceed

You now understand the three core Python container types:

1. **Lists** — ordered, mutable sequences 
2. **Dictionaries** — key-value mappings
3. **Sets** — unique, unordered collections

These structures appear everywhere in modern Python code.

Next, we will combine them with functions to implement real logic and learn more advanced concepts such as:

- list comprehensions (already introduced, now deeper)
- dict comprehensions
- set comprehensions
- using these structures safely in functions
- best-practice patterns for data processing

If ready, say **continue**.

# **Unified Mental Model**

| Type     | Ordered | Mutable | Unique Elements | Typical Use                            |
| -------- | ------- | ------- | --------------- | -------------------------------------- |
| **List** | Yes     | Yes     | No              | Sequences, pipelines, storage in order |
| **Dict** | No      | Yes     | Keys unique     | Mapping, structured data               |
| **Set**  | No      | Yes     | Yes             | Membership tests, deduplication        |
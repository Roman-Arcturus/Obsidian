# Lists — The Most Fundamental Mutable Data Type in Python

A **list** is an ordered, mutable collection of objects.  
It is Python’s general-purpose container type used in most real-world code.

### What they are
- A list is a **dynamic array**. 
- It stores **references** to objects (not raw values).
- It preserves **order**.
- It is **mutable** (can be changed after creation).
### Why useful
- Most common data structure for pipelines, loops, collections.
- Efficient `append()` due to over-allocation.
- Flexible and easy to work with.

Example:
numbers = [10, 20, 30]

Key properties:
1. **Ordered** — elements maintain their position. 
2. **Mutable** — you can add, remove, and change elements.
3. **Heterogeneous** (technically) — can contain mixed types, though in real projects **you avoid mixing types** to keep code clear.
4. **Dynamic size** — no fixed length.
5. Backed by a **dynamic array** internally.

## Why Lists Matter

Lists are used for:
- Iteration
- Collecting results
- Processing data from APIs, databases, files
- Passing structured data between functions
- Implementing higher-level operations (filtering, mapping, grouping)

## Creating Lists

**Empty list**
```
values: list[int] = []
```

**Basic literal**
```
values = [1, 2, 3]
```

**From another iterable**
```
values = list(range(5))
# values is [0, 1, 2, 3, 4]
```

**Accessing Elements**
```
values = [10, 20, 30, 40]
values[0]    # 10
values[2]    # 30
```

**Negative indexing:**
```
values[-1]   # 40
values[-2]   # 30
```
## Modifying Lists (mutation)

Mutation is the most important feature.

**Changing values**
```
values[1] = 99
```

**Adding elements**
```
values.append(100)
values.insert(0, 5)
```
**Removing elements**
```
values.pop()     # removes last, returns it
values.remove(99)  # removes first occurrence
```
Modern best practice:
- **append** for adding
- **pop** for removing
- **never** use `del` unless necessary

## Iterating Over Lists
```
for value in values:
    print(value)
```
Modern and clear.
Avoid indexing loops unless needed.

## List Comprehensions (critical modern concept)

A **list comprehension** is the Pythonic way to build lists concisely.
```
squares = [x * x for x in range(5)]
```
Why use them?
- Faster
- Cleaner
- More expressive
- Matches Python idioms
- Widely used in real codebases

Equivalent verbose version:
```
squares = []
for x in range(5):
    squares.append(x * x)
```
## Important Note About Mutability

Lists are **mutable**, and Python passes references to objects.
Example:
```
def modify(values: list[int]) -> None:
    values.append(99)

numbers = [1, 2, 3]
modify(numbers)
print(numbers)
# [1, 2, 3, 99]
```
This happens because:
- `numbers` holds a reference to a list in memory 
- `values` receives the same reference
- mutation affects the same underlying object

This is why you avoid using mutable default parameters — we will reinforce that later.

## Copying Lists Correctly

Because lists are mutable, copying must be explicit.

**Shallow copy (common)**
```
copy1 = numbers.copy()
```
or:
```
copy2 = list(numbers)
```

**Slicing copy**
```
copy3 = numbers[:]
```
All produce a new list object.

## Checking Membership
```
if 5 in values:
    # ...
```
## Summary Before Moving On

You now know:
- what lists are,
- why they exist,
- how to create and modify them,
- how mutation works and why it matters,
- how to iterate,
- how to build lists with comprehensions,
- how to copy them correctly.

This forms the basis for understanding more advanced data structures.

# Next Step: Dictionaries

Dictionaries are the most powerful built-in container in Python.  
They model structured data and are central to how modern Python applications store state.

If ready, say **continue**.


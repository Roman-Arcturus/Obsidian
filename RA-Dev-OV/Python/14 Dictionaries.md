# Dictionaries — Python’s Core Mapping Type

A **dictionary** is an associative container: it maps **keys** to **values**.
```
user = {
    "name": "Roman",
    "age": 40,
    "is_active": True
}
```
Conceptually, a dictionary is like a hash table in other languages.

Key properties:
1. **Key → value mapping**
2. **Keys must be immutable** (strings, ints, tuples, etc.)
3. **Values can be anything**
4. **Mutable** — can add, remove, and change entries
5. **Fast lookups** — dictionaries are O(1) on average
6. **Insertion-ordered** (since Python 3.7)

Dictionaries are used for configuration, JSON-like data, API responses, and all forms of structured, named data.
### What they are
- An **unordered**, **hash-based** mapping of keys to values.
- Keys must be **hashable** (immutable types: str, int, tuple, etc.).
- Values can be anything.
### Why useful
- Fast lookup by key (O(1)).
- Foundation for most Python data structures and objects.
- Used anywhere you need “named fields,” configuration, indexes, counters.

## 1. Creating Dictionaries

settings = dict(debug=True, port=8000)
```
user = {"name": "Roman", "age": 40}
```

**Empty**
```
data: dict[str, int] = {}
```

**Using dict() constructor**
(not commonly needed but acceptable)
```
settings = dict(debug=True, port=8000)
```

## 2. Accessing Values
```
user["name"]      # "Roman"
```
If the key doesn’t exist, this raises `KeyError`.  
To avoid this:
```
user.get("email")                   # returns None
user.get("email", default_value)    # returns default value
```
Modern best practice: use `.get()` when key may be missing.

## 3. Adding and Modifying Entries
```
user["country"] = "Germany"
user["age"] = 41
```
Overwriting uses the same syntax as adding.

## 4. Removing Entries
```
user.pop("age")
```
Best practice: use `.pop()` rather than `del`.

## 5. Iterating Over Dictionaries

**Keys**
```
for key in user:
    print(key)
```

**Values**
```
for value in user.values():
    print(value)
```

**Key & Value pairs**
```
for key, value in user.items():
    print(key, value)
```
Iterating `.items()` is the most useful pattern in real code.

## 6. Checking Membership

Membership checks test **keys**, not values.
```
if "name" in user:
    ...
```

## 7. Why Dictionaries Are So Important

Python uses dictionaries for:
- Object attributes
- Function keyword arguments
- Configuration structures
- JSON data
- Web and API payloads
- Almost all in-memory structured data

Whenever you need named fields → dictionary.
They are the backbone of modern Python applications.

## 8. Mutability and Function Interaction

Because dictionaries are mutable, modifying them inside functions affects the original.
```
def activate(u: dict[str, bool]) -> None:
    u["active"] = True

user = {"active": False}
activate(user)
# user is now {"active": True}
```
Same behavior as lists: Python passes references.

## 9. Copying Dictionaries

**Shallow copy**
```
copy1 = user.copy()
copy2 = dict(user)
```
These create new dictionary objects.

## 10. Summary Before Moving On

You now understand:
- keys and values
- how to access, modify, and remove entries
- how membership and iteration work
- why dictionaries matter
- how mutability affects function behavior

Dictionaries are the most common structured data container in modern Python.

# Next step: Sets

Sets introduce a different kind of container: unordered, unique, and highly optimized for membership tests.

If ready, say **continue**.
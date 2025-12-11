## 1. What it is

A **nested data structure** is any combination of lists, dictionaries, sets, and tuples _inside one another_.  

Examples:
- list of dicts
- dict of lists
- dict with dict inside
- list of dicts with dict inside

Real-world data (JSON, APIs, configs, DB query results) always use nested structures.

## 2. Why it’s useful

Because real applications never store data flat.  
You always have objects with attributes, lists of objects, objects containing collections, etc.

Practical uses:
- parsing JSON from an API
- responding to API requests
- reading/writing configs
- representing rows with metadata
- structuring intermediate computation state
- representing relationships (user → address → city)

Once you master nested structures, the next steps (classes, type hints, architecture) become natural.

## 3. Unified Mental Model

A nested structure is best understood as:

**A tree of containers.**
- Lists → ordered branches
- Dicts → named branches
- Leaves → primitive values (int, str, bool)

You “walk the tree,” inspecting or transforming each node.

This mental model will unify:
- validation
- copying
- normalization
- mutation vs immutability
- converting to/from JSON
- preparing objects for OOP design

## Exercise 1
```
user = {
    "name": "Alice",
    "meta": {
        "score": 10,
        "tags": ["admin", "premium"]
    }
}

def get_score(user: dict) -> int:
	...

"""
It must return:
- `user["meta"]["score"]` if it exists
- otherwise return `0`
- **must not mutate input**
- must be safe even if `"meta"` does not exist
- must be safe even if `"meta"` exists but has no `"score"`

**Your constraints:**
- No try/except
- No external libraries
- Only dictionary operations (`get`, `in`, etc.)

**Goal:** build reliable nested access logic.
"""
```

```
def get_score(user: dict) -> int: 
    if "meta" in user and "score" in user["meta"]:
        return user["meta"]["score"]
    
    return 0
```

## Exercise 2.
```
user = {
    "name": "Alice",
    "meta": {
        "tags": ["admin", "premium"],
        "score": 10
    }
}

def normalize_tags(user: dict) -> dict:
    ...
    
"""
But in real data, tags may be:
missing entirely
present but None
present but empty list
present but not a list (invalid input, e.g., a string)

**Requirements:**
1. Must return a NEW user dict (do not mutate original).
2. `"tags"` must always become a list inside `"meta"`.
3. If `"meta"` is missing, create it.
4. If `"tags"` is missing, set `[]`.
5. If `"tags"` is `None` or not a list, also set `[]`.
6. Preserve other fields as-is (e.g., `"score"`).
"""
```

```
def normalize_tags(user: dict) -> dict:
    result: dict = user.copy()
    result["meta"] = { "tags" : [] }

    if "meta" in user:
        if type(user["meta"]) is dict:
            result["meta"] = user["meta"].copy()

            if "tags" in result["meta"]:
                if type(result["meta"]["tags"]) is not list:
                    result["meta"] = { "tags" : [] }
            else:
                result["meta"]["tags"] = []

    return result
```
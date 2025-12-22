## Intent

Demonstrate how multiple names can reference the same object, how rebinding a name works, and how identity differs from value equality.

### Explanation

- Shows the **fundamental behavior of Python names as bindings**.
    
- Reinforces reasoning about shared identity, which underpins **Ownership and Boundaries**, **Mutation vs Transformation**, and **Safe List Processing**.
    
- Prepares the mental model for understanding **closures and data sharing**.

---
## Concept / Pattern Target

- Names and Binding  
- Identity vs Equality  
- Ownership and Boundaries

### Explanation

- **Names and Binding**: The core idea demonstrated — how names reference objects.
    
- **Identity vs Equality**: Highlights the difference between `is` (identity) and `==` (equality).
    
- **Ownership and Boundaries**: Illustrates potential ownership confusion when multiple names reference the same object.

This section ensures the example is **anchored in the mental model** rather than just being executable code.

---

## Setup / Input

- Objects:
  - A mutable list: `numbers = [1, 2, 3]`
  - An integer: `x = 10`

- Names:
  - `alias` will reference the same list as `numbers`
  - `copy_numbers` will hold a shallow copy of the list

### Explanation

- Explicitly defining **mutable vs immutable objects** and their bindings.
- Prepares to demonstrate:
    - Shared identity through `alias`
    - Rebinding of names
    - Safe copying via `copy_numbers`
- Ensures clarity about **ownership and potential mutation effects**.

---

## Code / Execution

```python
numbers = [1, 2, 3]
alias = numbers            # alias references the same list
copy_numbers = numbers[:]  # shallow copy

# Mutate through alias
alias.append(4)

# Rebind numbers to a new list
numbers = [10, 20, 30]

# Compare identity and equality
print("alias is numbers:", alias is numbers)
print("alias == numbers:", alias == numbers)
print("alias is copy_numbers:", alias is copy_numbers)
print("alias == copy_numbers:", alias == copy_numbers)
```
### Explanation

- `alias` and `numbers` initially **share identity**, showing how multiple names can reference the same object.
- `numbers = [10, 20, 30]` **rebinding** does not affect `alias`, illustrating the difference between rebinding and mutation.
- `copy_numbers` demonstrates **defensive copying** to preserve independence.
- Identity (`is`) vs equality (`==`) distinctions are clearly illustrated.

---
## Output / Behavior

```
- `alias is numbers` → `False` (different objects) ✅
- `alias == numbers` → `False` ([1,2,3,4] != [10,20,30]) ✅
- `alias is copy_numbers` → `False` (different objects) ✅
- `alias == copy_numbers` → `False` ([1,2,3,4] != [1,2,3]) ✅
```
### Explanation
- `alias is numbers` → different objects due to rebinding.
- `alias == numbers` → different contents after rebinding.
- `alias is copy_numbers` → different objects, shallow copy created a new object.
- `alias == copy_numbers` → different contents because `alias` was mutated after copy creation.

**Key takeaway:** Mutation through one name affects all names referencing the same object **until a rebinding or copy changes the reference or content**.


----
## Lessons Learned / Notes

- Names in Python are references (bindings) to objects; rebinding a name does not mutate the object.  

- Mutation through one name affects all other names sharing the same identity.  

- Shallow copies create independent objects, preventing unintentional side effects.

- Always consider ownership and boundaries when passing mutable objects to functions.  

- Identity (`is`) and equality (`==`) are distinct: identity tracks object reference, equality tracks content.  

- Understanding these behaviors is foundational for safe closures, pipelines, and list processing.

### Explanation

- Reinforces **Names and Binding**, **Identity vs Equality**, and **Ownership and Boundaries**.

- Prepares your mental model for **patterns like Safe List Processing and Defensive Closures**.

- Provides a **disposable, reproducible reference** that can be revisited anytime in Obsidian.

----
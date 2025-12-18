## Intent

Demonstrate the difference between object identity (`is`) and equality (`==`), especially for mutable and immutable objects, and how it affects reasoning about shared data.

### Explanation
- Highlights the fundamental distinction:
    - **Identity (`is`)** → do two names point to the same object?
    - **Equality (`==`)** → do two objects have the same value/content?
    
- This distinction underpins **Ownership and Boundaries**, **Mutation vs Transformation**, and **Safe List Processing**.
    
- Critical for understanding **when mutation affects multiple references**.

---

## Concept / Pattern Target

- [[ct04 Identity vs Equality]]  
- [[ct01 Names and Binding]]  
- [[ct02 Mutation vs Transformation]]  
- [[ct05 Ownership and Boundaries]]

### Explanation

- **Identity vs Equality**: Core focus—distinguishing object references from value comparison.
    
- **Names and Binding**: Demonstrates how multiple names may point to the same object.
    
- **Mutation vs Transformation**: Explains why mutation affects all names sharing identity, but not equal-but-different objects.
    
- **Ownership and Boundaries**: Shows how shared identity may cross boundaries, creating implicit ownership conflicts.

This ensures the example reinforces **mental models**, not just syntax.

---

## Setup / Input

- Immutable objects:
  - `a = 100`
  - `b = 100`  # same value as `a`

- Mutable objects:
  - `list1 = [1, 2, 3]`
  - `list2 = [1, 2, 3]`  # same value as `list1`, different object
  - `alias = list1`       # shared reference

### Explanation

- Immutable objects (`int`) may have **value equality and shared identity** due to interning, but this is implementation-dependent.
    
- Mutable objects (`list`) can be equal in content but differ in identity.
    
- `alias` allows demonstration of **shared mutation**.
    
- This setup prepares a clear comparison between `is` and `==` for both mutable and immutable cases.

---

## Code / Execution

```
## Code / Execution

# Immutable integers
a = 100
b = 100

print("a == b:", a == b)
print("a is b:", a is b)

# Mutable lists
list1 = [1, 2, 3]
list2 = [1, 2, 3]  # same value, different object
alias = list1      # shared reference

print("list1 == list2:", list1 == list2)
print("list1 is list2:", list1 is list2)
print("list1 is alias:", list1 is alias)

# Mutate alias
alias.append(4)
print("After mutation:")
print("list1:", list1)
print("alias:", alias)
print("list2:", list2)

```

### Explanation

- Immutable values (`a`, `b`) show that **equality and identity may coincide**, but identity can be implementation-dependent.
    
- Mutable lists (`list1`, `list2`) demonstrate that **equal content does not imply shared identity**.
    
- `alias` demonstrates that mutation **affects all names sharing identity**, but not equal-but-distinct objects.
    
- This clearly illustrates **how shared references propagate changes** and why identity matters in reasoning about ownership.

---

## Output / Behavior

```
a == b: True
a is b: True  # may be True due to interning

list1 == list2: True
list1 is list2: False
list1 is alias: True

After mutation:
list1: [1, 2, 3, 4]
alias: [1, 2, 3, 4]
list2: [1, 2, 3]
```

### Explanation

- **Immutable integers**: equality and identity coincide (`a == b` and `a is b`) because Python may reuse small integers.
    
- **Mutable lists**:
    - `list1 == list2` → True (same content)
    - `list1 is list2` → False (different objects)
    - `list1 is alias` → True (shared reference)
    
- **Mutation via alias** affects `list1` but not `list2`, demonstrating **ownership and boundary implications**.
    

**Key insight**:

- Equality (`==`) checks content, identity (`is`) checks reference.
    
- Understanding both is essential when reasoning about **mutation propagation** and **safe list processing**.

---

## Lessons Learned / Notes

- Identity (`is`) checks whether two names reference the same object; equality (`==`) checks whether their contents are the same.  
- Immutable objects may sometimes share identity due to interning; mutable objects do not unless explicitly aliased.  
- Mutation affects all names sharing identity, but not objects that are merely equal in value.  
- Understanding identity is critical for predicting side effects, ownership, and safe list processing.  
- These insights support reasoning for:
  - Defensive Closures  
  - Pure Data Pipelines  
  - Safe List Processing

### Explanation

- Reinforces the difference between **shared references** and **value equivalence**.
- Connects directly to **Ownership and Boundaries**, **Mutation vs Transformation**, and **Names and Binding**.
- Prepares the learner to reason about **side effects**, **function safety**, and **predictable transformations**.

---


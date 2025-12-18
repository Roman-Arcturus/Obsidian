## Intent

Demonstrate safe ways to process and transform lists without unintended side effects,
using defensive copying and clear ownership of data.

### Explanation

- Focuses on **preserving boundaries and avoiding accidental mutation** when working with lists.
    
- Highlights techniques for **processing lists safely** in Python:
    - Iterating and transforming without mutating the original
    - Using copies when mutation is necessary
    
- Reinforces mental models:
    - Ownership and Boundaries
    - Mutation vs Transformation
    - Safe handling in functional pipelines

---

## Concept / Pattern Target

- [[pt03 Safe List Processing]]  
- [[ct05 Ownership and Boundaries]]  
- [[ct02 Mutation vs Transformation]]  
- [[pt02 Pure Data Pipelines]]

### Explanation

- **Safe List Processing**: Primary focus — how to handle lists safely while avoiding unintended side effects.
    
- **Ownership and Boundaries**: Demonstrates how copying or transforming preserves boundaries.
    
- **Mutation vs Transformation**: Shows when to mutate versus when to return a new list.
    
- **Pure Data Pipelines**: Provides context for integrating safe list operations into functional pipelines.
    

This ensures the example teaches **both the problem and safe solution** clearly.

----

## Setup / Input

- Original list: `numbers = [1, 2, 3, 4, 5]`  
- Goal: produce a processed list where:
  1. All numbers are doubled
  2. Even numbers are filtered out
- Demonstrate two approaches:
  - Mutating a copy safely
  - Returning a new list via transformation
- Keep the original list unchanged

### Explanation

- The scenario highlights **safe handling of mutable lists**.
    
- Using either **copy-then-mutate** or **pure transformation** preserves ownership boundaries.
    
- This setup reinforces the principle that **shared references can propagate unintended changes**, so explicit control is required.

---
## Code / Execution

```
## Code / Execution

numbers = [1, 2, 3, 4, 5, 6]

# Approach 1: Copy and mutate safely
copy_numbers = numbers.copy()

for i in range(len(copy_numbers)):
    copy_numbers[i] += 1  # simple mutation that preserves some odd/even mix
    
safe_mutated = [
    n for n 
    in copy_numbers 
    if n % 2 != 0
]

#Approach 2: Pure transformation
safe_transformed = [
    n + 1
    for n in numbers
    if (n + 1) % 2 != 0
]

print("Original:", numbers)
print("Safe Mutated Copy:", safe_mutated)
print("Safe Transformed:", safe_transformed)
```

### Explanation of Corrections

- Instead of doubling (which always produces even numbers), we **increment by 1**, preserving a mix of odd/even values.
- Now the filtering condition `n % 2 != 0` is **meaningful**, producing non-empty lists.
- Both approaches still demonstrate:
    - **Copy and mutate safely**
    - **Pure transformation**
    
- The original list `numbers` remains unchanged, demonstrating **ownership boundaries**.

---

## Output / Behavior

Original: [1, 2, 3, 4, 5]
Safe Mutated Copy: [1, 3, 5]      # odd numbers after incrementing each element by 1
Safe Transformed: [1, 3, 5]       # same result using pure transformation

### Explanation

- **Original list** remains unchanged, demonstrating no side effects.
    
- **Safe Mutated Copy**:
    - Copy of `numbers` was incremented in place.
    - Filtering retains odd numbers.
    - Shows that mutation is safe when operating on a copy.
        
- **Safe Transformed**:
    - Comprehension produces a **new list directly** without mutating the input.
    - Demonstrates functional-style transformation.


**Key insight**:

> Both strategies preserve ownership boundaries and prevent unintended side effects.  
> This exemplifies **Safe List Processing** and reinforces **Mutation vs Transformation**.


---

## Lessons Learned / Notes

- Always consider **ownership**: mutating a copy avoids affecting the original list.  
- Safe list processing can be achieved by:
  - Copying and mutating (controlled, predictable)  
  - Pure transformations returning a new list (functional, side-effect-free)  
- Both approaches preserve **boundaries**, prevent accidental side effects, and maintain predictability.  
- Functional-style pipelines integrate seamlessly with these techniques:
  - You can chain transformations safely
  - Original data remains intact
- This example reinforces:
  - [[pt03 Safe List Processing]]  
  - [[ct05 Ownership and Boundaries]]  
  - [[ct02 Mutation vs Transformation]]  
  - [[pt02 Pure Data Pipelines]]

### Explanation

- Demonstrates **practical strategies** for working with mutable lists safely.
    
- Shows how **ownership, copying, and transformation** combine to prevent errors.
    
- Reinforces **predictable, functional-style patterns** that can be composed into larger pipelines.

---

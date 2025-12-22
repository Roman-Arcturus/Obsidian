## Intent

Demonstrate how to build predictable, composable, and side-effect-free pipelines
using transformations on data, avoiding in-place mutation.

### Explanation

- Focuses on **transforming data without mutation**, enabling clear and reusable pipelines.
- Highlights **why functional composition works** in Python when transformations are pure.
- Reinforces mental models:
    - Mutation vs Transformation
    - Pure Data Pipelines
    - Ownership and Boundaries
- Prepares the learner to safely chain operations like `map`, `filter`, and comprehensions.

---

## Concept / Pattern Target

- [[pt02 Pure Data Pipelines]]  
- [[ct02 Mutation vs Transformation]]  
- [[ct05 Ownership and Boundaries]]  
- [[pt03 Safe List Processing]]

### Explanation

- **Pure Data Pipelines**: Core focus — creating composable, side-effect-free transformations.
- **Mutation vs Transformation**: Demonstrates why in-place changes break predictability.
- **Ownership and Boundaries**: Shows how transformation preserves boundaries and avoids unintended mutation.
- **Safe List Processing**: Provides a practical context for safely chaining operations on lists.

This ensures the example demonstrates **both mental clarity and practical application**.

---

## Setup / Input

- A list of integers: `numbers = [1, 2, 3, 4, 5]`  
- Goal: build a sequence of pure transformations:  
  1. Filter out even numbers  
  2. Double the remaining numbers  
  3. Convert them to strings  
- No mutation of the original list  
- Use comprehensions and functional composition

### Explanation

- `numbers` is **immutable in the sense that we will not change it in place**.
- Each transformation produces a **new list**, preserving the input and enabling safe composition.
- This setup emphasizes **predictability, reusability, and side-effect-free design**.

------

## Code / Execution

```python
## Code / Execution

numbers = [1, 2, 3, 4, 5]

# Step 1: Filter out even numbers
filtered = [
    n
    for n in numbers
    if n % 2 != 0
]

# Step 2: Double the remaining numbers
doubled = [
    n * 2
    for n in filtered
]

# Step 3: Convert to strings
stringified = [
    str(n)
    for n in doubled
]

print("Original:", numbers)
print("Filtered:", filtered)
print("Doubled:", doubled)
print("Stringified:", stringified)

```

### Explanation

- Each comprehension **returns a new list**, avoiding in-place mutation.
- Steps are **composable and declarative**, reflecting functional style.
- The original list `numbers` is preserved, demonstrating **ownership boundaries**.
- This shows a **safe, predictable data pipeline** with explicit transformations.

---

## Output / Behavior

Original: [1, 2, 3, 4, 5]
Filtered: [1, 3, 5]
Doubled: [2, 6, 10]
Stringified: ['2', '6', '10']

### Explanation

- **Original list** remains unchanged, demonstrating **no mutation**.
- Each step produces a **new list**, preserving boundaries and enabling safe composition.
- Transformations are **predictable and reproducible**.
- Highlights key insights for **Pure Data Pipelines**:
    - Inputs are never mutated
    - Outputs depend only on inputs
    - Each stage can be independently reasoned about and reused

**Key insight:**

> Pure transformations are the foundation of composable, side-effect-free pipelines.

---

## Lessons Learned / Notes

- Each transformation returns a new object, preserving the original input.  
- Pure pipelines enable **safe, predictable, and composable operations**.  
- Avoiding mutation ensures that:
  - No upstream data is accidentally changed
  - Downstream stages remain consistent
  - Functional composition is reliable
- This pattern supports:
  - [[pt02 Pure Data Pipelines]]  
  - [[ct02 Mutation vs Transformation]]  
  - [[ct05 Ownership and Boundaries]]  
  - [[pt03 Safe List Processing]]

### Explanation

- Reinforces the principle that **functional-style transformations** are safer than in-place mutation for pipeline construction.
- Highlights the link between **ownership boundaries** and **predictable behavior**.
- Supports mental models needed for chaining higher-order functions and building robust Python programs.

---


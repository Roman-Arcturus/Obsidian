I am learning Python from scratch, but I have prior low-level and scripting experience (ASM / C / JS) from the past. I am comfortable with programming concepts but new to Python’s syntax, idioms, and data model.

**Teaching requirements:**

1. Teach Python using **modern best practices only**.  
    Do not show multiple ways unless explicitly asked.
    
2. Explanations must always include:
    - **What it is**
    - **Why it’s useful**
    - **A unified mental model** (how to reason about it)
    
3. I care more about **WHY something works than HOW to memorize it**.  
    Assume I am building a correct mental model, not just syntax familiarity.
    
4. Favor **functional-style thinking**:
    - map / filter / sorted
    - comprehensions
    - first-class functions
    - closures  
        but always in a **Pythonic and readable** way (clarity over cleverness).
    
5. Move **slowly and deliberately**, one concept or exercise at a time.  
    I want to practice after each step.
    
6. Always:
    - Specify **input and output types**
    - Explain **mutation vs non-mutation**
    - Avoid hidden side effects
    - Prefer defensive defaults when dealing with nested structures
        
7. Use **multi-line formatting** for complex expressions:
    
```
result = [
    expr_if_true
    if condition
    else expr_if_false
    for item in iterable
]
```
    
8. Do **not** assume knowledge of:
    - OOP beyond basics
    - Complex nested structures unless explained
    - Advanced modules (no imports unless introduced and explained)
    
9. Avoid global variables and mutation unless explicitly teaching them.
    
10. When I make a mistake:
    - First validate correct reasoning
    - Then explain the mistake precisely
    - Do not rush forward if a concept is shaky
    
11. Maintain a **focused, professional, exacting tone**.  
    No emojis. No fluff. No oversimplification.


**Current level and last topic:**

- Comfortable with lists, dicts, sets, tuples
- Comprehensions (basic and advanced)
- map / filter / sorted
- any / all
- lambdas
- first-class functions
- closures
- Just covered: **late binding in closures**
- Last exercise result: `[30, 30, 30]`

**Next step when continuing:**

- Fixing the closure late-binding issue cleanly
- Then moving to practical closure patterns and pitfalls

Please continue from this point, maintaining consistency with prior explanations.



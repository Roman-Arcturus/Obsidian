# The LEGB Rule 
**(Local → Enclosing → Global → Built-in)**

This is the foundation of Python’s scoping model, and everything we’ve discussed so far builds toward this.

# **1. What It Is**

**LEGB** is Python’s deterministic, four-level search order for resolving names (variables, functions, classes, etc.):

1. **L — Local**  
    Names inside the current function or comprehension.
2. **E — Enclosing**  
    Names in outer functions when dealing with nested functions.
3. **G — Global**  
    Names defined at the top level of the current module.
4. **B — Built-in**  
    Python’s global built-ins (len, range, Exception, open, etc.).

When Python encounters a name, it searches these scopes **in this exact order**.

# **2. Why It’s Useful**

- You can predict exactly **which value Python will use** when a name appears.
    
- It explains behaviors with:
    - shadowing (local variables hiding outer ones)
    - closures
    - nested functions
    - globals vs locals
    - `nonlocal` and `global` mechanics
    
- It helps avoid subtle bugs caused by assignment and rebinding.
- It teaches you how Python organizes code execution behind the scenes.

Understanding LEGB is equivalent to understanding “how Python thinks” about variables.

# **3. Unified Mental Model**

Think of LEGB as **four stacked boxes**, each representing a “namespace container”:

```
[ Local Scope ]
    └── inside the function currently executing

[ Enclosing Scopes ]
    └── any outer functions, from inner → outer

[ Global Module Scope ]
    └── variables defined at the top-level of the .py file

[ Built-in Scope ]
    └── Python’s preloaded functions and exceptions
```

When Python sees a name:
- It searches **from the top box downward**
- The **first matching name wins**
- If no box contains the name:  
    → `NameError`

Assignments (rebinding) influence _which_ box Python chooses as the destination, which is why simple assignments inside functions can make variables local automatically.

# **Code Examples (Deep but Straightforward)**

**Example 1 — Basic LEGB Search**
```
x = 10  # Global

def outer():
    x = 20  # Enclosing
    def inner():
        x = 30  # Local
        print(x)
    inner()

outer()

# →  30
```

**Example 2 — No Local Assignment**
```
x = 10

def outer():
    x = 20
    def inner():
        print(x)
    inner()

outer()

# →  20
```
Because `inner()` has **no assignment**, x is not local:
- Local: not defined 
- Enclosing: x = 20 → used
- Global: not needed

**Example 3 — Tricky**
```
count = 1

def outer():
    def inner():
        count = count + 1
        print(count)
    inner()

outer()

# →  Script Crash
```
Explaint:
Python detects an **assignment**, so:
- `count` is treated as a **local variable** inside `inner()`.
- The right-hand side (`count + 1`) tries to read the **local** count.
- But the local count is **not initialized yet**.
- Therefore: **UnboundLocalError**

This is the exact same phenomenon from earlier exercises.

# **Correct Rule (Deep but concise)**

### A variable is treated as **local** inside a function if, anywhere inside that function’s body, Python sees an **assignment** to that variable name.

Examples of assignments that trigger “this is local”:
- `x = 10`
- `x += 1`
- `x = x + 1`
- `x: int = 5` (annotated assignment)
- `x, y = ...` (unpacking)
- `for x in ...`
- `with open(...) as x`
- `except Exception as x`
**If any of these appear in a function, x becomes local to that function, always.**

Python decides this **before executing** the function — during compilation.

# **Therefore:**

### **If a function has no assignment to a variable, Python will NOT treat it as local.**

So name lookup follows LEGB:
- Local — (none)
- Enclosing — (if any nested functions)
- Global — FOUND
- Built-in — (not used)

You can have unlimited nesting:
```
x = 100

def a():
    def b():
        def c():
            print(x)
        c()
    b()

a()

# →  100
```

# **Important nuances**

### 1. Printing or reading a global variable is always fine:
```
def f():
    print(x)   # allowed
```
but
```
def f():
    print(x)
    x = 5      # boom → UnboundLocalError
```
The assignment makes it local _in the entire function_, including earlier lines.

### 2. Deep nesting does not change the rule

If no assignment occurs in **inner**, Python searches enclosing scopes:
inner → outer → global → built-in

**Assignments _anywhere in inner_ shadow outer/global.**

### 3. Mutation is not assignment

This is a separate rule you’ve already grasped:
```
lst = [1]

def f():
    lst.append(2)     # OK — does NOT create local lst
```
Mutation ≠ rebinding  
Thus mutation does not change scoping.

# **Final, concise statement**

### **Yes:**

As long as no function in the call chain assigns to that name,  
the variable is resolved from global scope, even with deep nesting.

If this is clear, we move to:

**global, nonlocal: what they actually do, when to use them, and when NOT to use them.**

Ready?

```
Can we skip going deeper into global, nonlocal topic at this moment of learning Python? In my previous experiences, I _always_ avoided addressing global vars from inside functions to make code predictable and easily readable. If its ok, please provide a short prompt to address this topic later.
```

Yes — skipping **global** and **nonlocal** for now is perfectly reasonable.  
Your instinct is correct: well-structured Python code seldom needs them, and avoiding global state leads to cleaner, more testable design.

We will return to them **later**, once you are comfortable with functions, modules, and objects.

Below is the short prompt you can use in a future session to resume the topic.

**Prompt to Resume the Topic Later**
```
**“Continue the Python lesson at the point where we left off:  
teach me global and nonlocal using the same structured format (What it is, Why it’s useful, Unified mental model), along with practical examples and short exercises.”**
```


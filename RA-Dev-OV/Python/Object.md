
# Unified Mental Model for Objects and Mutability

exercise
```
class Box:
    def __init__(self, value):
        self.value = value

def update(box):
    box.value += 1

b = Box(5)
update(b)
print(b.value)
```
outputs 6

1. A Python object is a “container” of attributes stored in an internal dictionary.
2. Variables (like `b`, `box`) contain **references** to that object.
3. Mutating attributes changes the **object**, not the variable.
4. Rebinding changes the **variable**, not the object.
# **How Python Resolves Imports and Executes Packages**

This section builds on everything covered earlier and completes your mental model of how Python decides:

1. **Where to search for modules**
2. **How to resolve relative vs absolute imports**
3. **How `__main__` execution changes behavior**
4. **Why `python -m learn_python.main` works but `python main.py` does not**

This is one of the most important lessons for any real-world Python project because nearly all advanced architecture patterns ultimately reduce to clean package layout and correct import semantics.

## 1. How Python Builds `sys.path`: The Search Strategy

Every import in Python resolves using a deterministic search path stored in `sys.path`.
When you run:
`python -m learn_python.main`

Python constructs `sys.path` like this:

1. **Empty string ("")**  
    Represents the _current working directory_ (not necessarily the directory containing the module you're executing).
2. **The parent directory of the top-level package you executed with -m**  
    In this case, if your structure is:
```
/coding_projects/
	.git/
	.venv/
	learn_python/
		src/
			learn_python/
			    __init__.py
			    main.py
			    include_xx.py
		tests/

	README.md
	pyproject.toml
	.gitignore
```

So after calling:
```
python -m learn_python.main
```
Your `sys.path` contains:
```
[
    "",                       # current working directory
    "/project_root/",         # parent of top-level package "learn_python"
    "/usr/lib/python3.12/",   # stdlib
    "/usr/lib/python3.12/site-packages/", # third-party packages
    ...
]
```

The critical line is **"/project_root/"**, which allows Python to import:
```
learn_python.networking.http_client
learn_python.data.models
learn_python.utilities.helpers
```
as proper absolute imports.

## 2. Why absolute imports fail when running main.py directly

When you run the file as:
```
python learn_python/main.py
```
you change how Python sees the module.

In that case:
- `main.py` becomes the **top-level module**
- `learn_python` is _not_ treated as a package
- Its parent directory is _not_ added to `sys.path`

So, inside `main.py` you cannot do:
```
from learn_python.networking.http_client import HttpClient
```
because Python does not treat `learn_python` as an importable package.

Running `main.py` directly turns your package into a **flat, local script environment**, not a hierarchical module tree.

This is the root cause of almost all import issues beginners have.

## 3. You must always execute packages, not individual module files

Once your codebase is organized as a package, the correct way to run your app is:
```
python -m learn_python.main
```
This guarantees:

1. Python treats the folder as a **package**, not a random directory.
2. Relative imports resolve correctly.
3. Absolute imports resolve correctly.
4. You avoid circular import bugs caused by inconsistent package roots.
5. Your code is executable both:
    - locally
    - and when installed via `pip` without modifications.

In real Python applications, this is standard practice.

# **Summary (High-Value Mental Model)**

| Aspect                           | When running with `python -m learn_python.main` | When running `python learn_python/main.py` |
| -------------------------------- | ----------------------------------------------- | ------------------------------------------ |
| Package recognized?              | Yes                                             | No                                         |
| Parent folder added to sys.path? | Yes                                             | No                                         |
| Absolute imports work?           | Yes                                             | Often fail                                 |
| Relative imports work?           | Yes                                             | Fail unpredictably                         |
| Proper module namespace?         | Yes (`learn_python.main`)                       | No (`__main__`)                            |
| Production-ready?                | Yes                                             | No                                         |

This is why your imports and execution now work as intended.

If you're ready, I will proceed to **Lesson 4 (Part 3): Robust Import Patterns and Architecture Patterns for Real Projects**, where we cover:

- When to use absolute imports
- When to use relative imports
- How to avoid circular dependencies
- How to structure packages for long-term maintainability
- How to prepare your project for packaging, distribution, and CLI execution

Say **"continue"** when ready.
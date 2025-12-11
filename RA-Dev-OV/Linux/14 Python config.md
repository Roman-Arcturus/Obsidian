# Building a professional-grade Python project from scratch on Linux Mint

following real-world best practices.

You already have Python, venv, pip, Git, SSH signing, VS Code, and global Git config ready.  

## 1. Create the Project Directory Structure (Pro Baseline)

```
/coding_projects/
	learn_python/
		src/
			learn_python/
			    __init__.py
			    main.py	
			    include_xx.py		
		tests/

		README.md
		pyproject.toml
		requirements.txt
		.gitignore
```

[coding_projects/learn_python]
```
git init
python3 -m venv .venv
source .venv/bin/activate
```

check folders
```
/coding_projects/
	learn_python/
		.git/
		.venv/	
		src/
			learn_python/
			    __init__.py
			    main.py
			    include_xx.py
		tests/
			test_include_xx.py
		
		README.md
		pyproject.toml
		requirements.txt
		.gitignore
```

## 2. Populate the .gitignore in project root
```
# Environment
.venv/

# Python compiled
__pycache__/
*.pyc
.env

# Packaging
dist/
build/
*.egg-info/

# IDE
.vscode/

.ruff_cache/
```

## 3. Create a minimal `pyproject.toml`

This is the modern replacement for requirements.txt, setup.py, setup.cfg, and others.  
For now, you will define **tooling only**. Dependencies will be added later.

```
[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "learn_python"
version = "0.1.0"
description = ""
requires-python = ">=3.10"

[tool.setuptools.packages.find]
where = ["src"]

[tool.black]
line-length = 88
target-version = ["py310"]

[tool.ruff]
line-length = 88
select = ["E", "F", "W"]
ignore = []

[tool.pytest.ini_options]
pythonpath = ["src"]
```

This gives you immediate support for:
- Packaging
- Black formatting
- Ruff linting
- Pytest testing
- src-layout imports

## 4. Install essential dev dependencies

```
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip setuptools wheel
pip install black ruff pytest

pip install fastapi pydantic
pip install pytest black ruff mypy
pip freeze > requirements.txt
```

## 5. First commit

```
git add . 
git cm "Initialize professional project structure"
```

Since commit signing is enforced globally, this commit will be signed automatically.

## 6. Configure VS Code to Use Your Local Environment

Create .vscode/settings.json inside project directory
```
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
  "editor.formatOnSave": true,
  "ruff.enable": true
}
```
This ensures:
- all tooling runs inside the project,
- formatting and linting occur automatically.

## 7. Add Pre-commit Hooks (pipx-installed)

With pre-commit installed via pipx, you just configure it in the project:

.pre-commit-config.yaml in the project root
```
repos:
  - repo: https://github.com/psf/black
    rev: 24.4.2
    hooks:
      - id: black

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.6.9
    hooks:
      - id: ruff

  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.11.1
    hooks:
      - id: mypy
```

refresh precommits
```
pre-commit clean
pre-commit install
pre-commit autoupdate
```

project directory structure
```
/coding_projects/
	learn_python/
		.git/
		.venv/	
		src/
			learn_python/
			    __init__.py
			    main.py
			    include_xx.py
		tests/
			test_include_xx.py
		
		README.md
		pyproject.toml
		requirements.txt
		.gitignore
		.pre-commit-config.yaml	    
		.vscode/
			settings.json
```

create a new repository on github with the same project name: learn_project
bash
```
git add .
git commit -m "Pushing to github learn_project"  

git remote add origin https://github.com/Roman-Arcturus/learn_python.git
git branch -M main
git push -u origin main
```

-----

```
# from project root
sudo apt update
sudo apt install -y python3 python3-venv python3-pip build-essential python3-distutils

# create & activate venv
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip setuptools wheel

# install project and dev tools
pip install -e .
pip install pytest black ruff pre-commit

# run tests and linters
pytest -q
ruff check src tests
black src tests

# optional: init git and pre-commit
git init
pre-commit install

```

Next steps:
- Add automated formatting and linting (Black, Ruff, isort).
- Add pre-commit hooks.
- Configure VS Code tasks for running tests, coverage, or environment automation.
- Build a proper `pyproject.toml` for packaging.
- Create CI (GitHub Actions) to run tests automatically.

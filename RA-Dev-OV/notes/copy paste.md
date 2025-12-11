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
py# Contributing

## Setting up a development environment
- Generate requirements-dev.txt file.
  ```
  python -m pip install pip-tools
  pip-compile requirements-dev.in
  ```

- Install development dependencies.

  ```
  python3 -m pip install -r requirements-dev.txt
  ```

- Install [pre-commit](https://pre-commit.com/) hooks.

  ```
  python3 -m pip install pre-commit
  python3 -m pre_commit install
  ```

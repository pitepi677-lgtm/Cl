# CLAUDE.md

Guidance for Claude Code when working in this repository.

> **Status:** The project is new and has no code yet. The tooling below is the
> intended default for a Python project. Once `pyproject.toml` and real tooling
> exist, update this file so it matches them, and trust the config files if
> the two ever disagree.

## Stack

- Python 3.12+
- [uv](https://docs.astral.sh/uv/) for environments, dependencies and running commands
- [pytest](https://docs.pytest.org/) for tests
- [ruff](https://docs.astral.sh/ruff/) for linting and formatting
- [mypy](https://mypy.readthedocs.io/) for type checking
- All configuration lives in `pyproject.toml` (no `setup.py`, `setup.cfg` or `requirements.txt`)

## Layout (intended)

```
src/<package>/     # application code (src layout)
tests/             # pytest tests, mirroring src/ structure (tests/test_<module>.py)
pyproject.toml     # project metadata, dependencies, tool config
```

## Commands

```bash
uv sync                          # create .venv and install all deps (incl. dev)
uv run pytest                    # run the full test suite
uv run pytest tests/test_x.py    # run one test file
uv run pytest -k name            # run tests matching a name
uv run ruff check .              # lint
uv run ruff check --fix .        # lint and auto-fix
uv run ruff format .             # format
uv run mypy src                  # type check
uv add <pkg>                     # add a runtime dependency
uv add --dev <pkg>               # add a dev dependency
```

Before you call a change done, run: `uv run ruff format . && uv run ruff check . && uv run mypy src && uv run pytest`

## Code style

- Format with `ruff format` (Black-compatible, 88-char lines). Don't format by hand.
- Add type hints to all function signatures and public attributes. Use modern
  syntax: `list[str]`, `X | None`, not `List[str]` or `Optional[X]`.
- Naming: `snake_case` for functions and variables, `PascalCase` for classes,
  `UPPER_SNAKE_CASE` for constants, a leading `_` for private names.
- Use absolute imports within the package. Imports are sorted by ruff (isort rules).
- Use `pathlib.Path` instead of `os.path`, and f-strings instead of `%` or `.format()`.
- Write docstrings (Google style) for public modules, classes and functions.
  Skip them for trivial private helpers.
- Use `logging` instead of `print` in library code.
- Raise specific exceptions. Never write a bare `except:`.

## Testing

- **Every feature or bug fix comes with tests.** A bug fix includes a test that
  fails without the fix.
- Put tests under `tests/`, named `test_<module>.py`, with functions named `test_<behavior>`.
- Prefer plain `assert` and pytest fixtures over `unittest.TestCase`.
- Use `pytest.mark.parametrize` for input/output tables.
- Tests must not hit the network or depend on test order. Use `tmp_path` for files.

## Git

- Write commit messages in the imperative mood, with a short summary line
  (≤ 72 chars) and a body explaining *why* when the change isn't obvious.
- Never commit `.venv/`, `__pycache__/`, `.env` or other secrets.

# Junie Guidelines for project

This file provides guidance for JetBrains IDEs with Junie integration. It helps AI assistants (Junie and others) understand the context of this project and collaborate effectively.

## Project Overview

* **Name:** <project name>
* **Description:** <project description>
* **Language:** Python 3.12+
* **Dependency Manager:** [uv](https://docs.astral.sh/uv/) (required)
* **Structure:**

  * `src/<project name>/` — core package
  * `tests/` — unit and integration tests
  * `docs/` — design notes and documentation
  * `scripts/` — helper scripts

## Development Workflow

1. **Install dependencies**

   ```bash
   uv sync --dev
   ```

2. **Run common checks**

   ```bash
   uv run python -m ruff check .
   uv run python -m mypy src
   uv run python -m pytest
   ```

3. **Format code**

   ```bash
   uv run python -m black .
   ```

4. **Coverage**

   ```bash
   PYTHONPATH=src uv run python -m pytest --cov --cov-report=term-missing
   ```

5. **Pre-commit**

   ```bash
   uv run python -m pre_commit run --all-files || uv run python -m pre-commit run --all-files
   ```

## Coding Guidelines

* **Style:**

  * Black (formatting)
  * Ruff (linting)
  * Mypy (type checking, strict mode)
* **Tests:** All code in `src/` must have corresponding tests in `tests/`.
* **Structure:** Keep functions small, modular, and well-documented.
* **Dependencies:**

  * Runtime deps in `project.dependencies`.
  * Dev tools in `[dependency-groups].dev`.

## Collaboration Guidelines

* **Commit Messages:**

  * Use [conventional commits](https://www.conventionalcommits.org/) style where possible: `feat:`, `fix:`, `chore:`, `docs:`, `test:`.
  * Keep them clear and scoped.

* **Branches:**

  * `main`: protected, stable.
  * `feat/*`, `fix/*`, `chore/*`, `docs/*`: for work branches.

* **Pull Requests:**

  * Must pass lint, type checks, and tests before review.
  * Include rationale for changes.

## Junie/AI Agent Instructions

* **When suggesting code:**

  * Follow Black/Ruff/Mypy rules.
  * Place new code in `src/<project name>/` or `tests/` as appropriate. (replace <project name> with the actual project name)
  * Respect existing project structure.

* **When writing tests:**

  * Use `pytest` style.
  * Mirror package layout in `tests/`.

* **When updating dependencies:**

  * Add runtime deps under `[project.dependencies]`.
  * Add dev tools under `[dependency-groups].dev`.
  * Always run `uv sync --dev` after editing `pyproject.toml`.

* **When generating documentation:**

  * Use Markdown.
  * Place docs in `docs/`.

## Example Tasks for Junie

* Suggest unit tests for new modules under `src/<project name>/`.
* Propose refactorings that reduce complexity while preserving behavior.
* Draft or improve documentation in `docs/`.
* Recommend dependency updates if newer stable versions exist.

---

**Note:** This file is intended to give Junie (and other AI coding assistants) clear project context and conventions, ensuring generated contributions align with the team’s standards.

## Example pyproject.toml file

Copy content to your `pyproject.toml` in the root of your project and adjust according to your needs.

```toml
[project]
name = "<project name>"
version = "0.1.0"
description = "<project description>"
readme = "README.md"
requires-python = ">=3.12"
authors = [
  { name = "<your name>", email = "<your email>" },
]
license = { text = "MIT" }
keywords = ["<project name>", "<project description>", "<other project keywords>]
classifiers = [
  "Development Status :: 2 - Pre-Alpha",
  "License :: OSI Approved :: MIT License",
  "Programming Language :: Python :: 3",
  "Programming Language :: Python :: 3 :: Only",
  "Programming Language :: Python :: 3.12",
  "Topic :: <project topic>",
]

# Just a placeholder minimal runtime deps for an empty scaffold. Actual project deps can be expanded later and will vary.
dependencies = [
  "pygame>=2.6.0",
  "numpy>=1.26", 
]


# Optional: add a console entry point for your application
[project.scripts]
<project name> = "<project name>.main:main"

[project.urls]
Homepage = "https://example.com/<project name>"
Repository = "https://github.com/<project name>/repo"
Issues = "https://github.com/<project name>/issues"

[build-system]
# uv will generate and use its own build backend; specify default for fallback
requires = ["setuptools>=69", "wheel"]
build-backend = "setuptools.build_meta"

# Tooling configuration
[tool.ruff]
line-length = 100
target-version = "py312"

[tool.ruff.lint]
select = [
  "E", "F",      # pycodestyle + pyflakes
  "I",            # import sorting
  "UP",           # pyupgrade
  "B",            # flake8-bugbear
  "C90",          # mccabe complexity
]
ignore = [
]

[tool.ruff.lint.isort]
known-first-party = ["<project name>"]

[tool.black]
line-length = 100
target-version = ["py312"]

[tool.pytest.ini_options]
minversion = "8.0"
addopts = "-q"
testpaths = ["tests"]

[tool.mypy]
python_version = "3.12"
check_untyped_defs = true
warn_unused_ignores = true
warn_redundant_casts = true
warn_unused_configs = true
disallow_untyped_defs = true
no_implicit_optional = true
strict_equality = true
mypy_path = ["src"]

# foo
[tool.coverage.run]
source = ["src"]
branch = true

[dependency-groups]
dev = [
    "black>=25.1.0",
    "mypy>=1.17.1",
    "pre-commit>=4.3.0",
    "pytest>=8.4.1",
    "pytest-cov>=6.2.1",
    "ruff>=0.12.10"
]

[tool.uv.dependency-groups]
dev = { requires-python = ">=3.12" }
```

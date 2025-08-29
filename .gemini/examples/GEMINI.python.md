# Project Context: Modern Python Application

## Core Principles

- **Python Version**: The project MUST use Python 3.12 or newer.
- **Package Management**: This project uses `uv`. All dependency management and virtual environment creation MUST be done with `uv` (`uv pip install`, `uv venv`, etc.). `requirements.txt` or `pyproject.toml` is the source of truth for dependencies.
- **Linting & Formatting**: This project uses `Ruff` for both linting and formatting (`ruff check` and `ruff format`). All code MUST pass checks before merging. A `ruff.toml` or `pyproject.toml [tool.ruff]` section defines the rules.
- **Type Checking**: All code MUST be fully type-hinted and pass `mypy` static analysis in strict mode.

---

## Application Details

- **Framework**: The web API is built using FastAPI for its performance and automatic OpenAPI documentation.
- **Data Validation**: Use Pydantic V2 for all data modeling and validation.
- **Testing**: Use `pytest` for all tests. Tests should be located in a separate `tests/` directory. Use `httpx` for testing API endpoints.
- **Configuration**: Use environment variables for configuration, parsed by Pydantic's `BaseSettings`.
- **Deployment**: The application is designed to be containerized with Docker. The Dockerfile should use a multi-stage build for a small final image size.

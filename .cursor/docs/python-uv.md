# Python + uv Documentation

## Project Setup

### Initial Configuration
```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create new project
uv init my-project
cd my-project

# Add dependencies
uv add fastapi uvicorn
uv add --dev pytest ruff mypy
```

### Project Structure
```
my-project/
├── pyproject.toml
├── src/
│   └── my_project/
│       ├── __init__.py
│       ├── main.py
│       └── api/
│           ├── __init__.py
│           └── routes.py
├── tests/
│   ├── __init__.py
│   └── test_main.py
└── README.md
```

## Package Management

### pyproject.toml Configuration
```toml
[project]
name = "my-project"
version = "0.1.0"
description = "A sample project using uv"
authors = [
    {name = "Your Name", email = "your.email@example.com"}
]
dependencies = [
    "fastapi>=0.104.0",
    "uvicorn[standard]>=0.24.0",
    "pydantic>=2.5.0",
]
requires-python = ">=3.8"

[project.optional-dependencies]
dev = [
    "pytest>=7.4.0",
    "pytest-asyncio>=0.21.0",
    "ruff>=0.1.0",
    "mypy>=1.7.0",
    "black>=23.0.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
target-version = "py38"
line-length = 88
select = [
    "E",  # pycodestyle errors
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "C4", # flake8-comprehensions
    "UP", # pyupgrade
]
ignore = [
    "E501",  # line too long, handled by black
    "B008",  # do not perform function calls in argument defaults
]

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]

[tool.mypy]
python_version = "3.8"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true
no_implicit_optional = true
warn_redundant_casts = true
warn_unused_ignores = true
warn_no_return = true
warn_unreachable = true
strict_equality = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = "-v --tb=short"
```

## Development Workflow

### Package Management Commands
```bash
# Add dependencies
uv add fastapi
uv add --dev pytest

# Remove dependencies
uv remove requests

# Sync environment
uv sync

# Run commands
uv run python main.py
uv run pytest
uv run mypy src/

# Activate virtual environment
source .venv/bin/activate  # Linux/macOS
.venv\Scripts\activate     # Windows
```

### Tooling Commands
```bash
# Linting and formatting
uvx ruff check .          # Check for issues
uvx ruff check --fix .    # Auto-fix issues
uvx ruff format .         # Format code

# Type checking
uv run mypy src/

# Testing
uv run pytest             # Run all tests
uv run pytest -v          # Verbose output
uv run pytest -k "test_name"  # Run specific test
uv run pytest --cov=src   # With coverage

# Development server
uv run uvicorn src.main:app --reload
```

## Code Examples

### FastAPI Application
```python
# src/main.py
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI(title="My API", version="1.0.0")

class User(BaseModel):
    id: int
    name: str
    email: str
    is_active: bool = True

class CreateUserRequest(BaseModel):
    name: str
    email: str

# In-memory storage (replace with database in production)
users: List[User] = []
user_id_counter = 1

@app.get("/")
async def root() -> dict[str, str]:
    return {"message": "Hello World"}

@app.get("/users", response_model=List[User])
async def get_users() -> List[User]:
    return users

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int) -> User:
    user = next((u for u in users if u.id == user_id), None)
    if user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return user

@app.post("/users", response_model=User, status_code=201)
async def create_user(user_data: CreateUserRequest) -> User:
    global user_id_counter
    
    # Check if email already exists
    if any(u.email == user_data.email for u in users):
        raise HTTPException(status_code=400, detail="Email already registered")
    
    user = User(
        id=user_id_counter,
        name=user_data.name,
        email=user_data.email
    )
    users.append(user)
    user_id_counter += 1
    return user

@app.delete("/users/{user_id}")
async def delete_user(user_id: int) -> dict[str, str]:
    global users
    original_length = len(users)
    users = [u for u in users if u.id != user_id]
    
    if len(users) == original_length:
        raise HTTPException(status_code=404, detail="User not found")
    
    return {"message": "User deleted successfully"}
```

### Testing
```python
# tests/test_main.py
import pytest
from fastapi.testclient import TestClient
from src.main import app

client = TestClient(app)

def test_read_root():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello World"}

def test_create_user():
    user_data = {"name": "John Doe", "email": "john@example.com"}
    response = client.post("/users", json=user_data)
    assert response.status_code == 201
    data = response.json()
    assert data["name"] == user_data["name"]
    assert data["email"] == user_data["email"]
    assert data["is_active"] is True
    assert "id" in data

def test_get_users():
    response = client.get("/users")
    assert response.status_code == 200
    assert isinstance(response.json(), list)

def test_get_user_not_found():
    response = client.get("/users/999")
    assert response.status_code == 404
    assert response.json()["detail"] == "User not found"

def test_create_user_duplicate_email():
    user_data = {"name": "Jane Doe", "email": "jane@example.com"}
    
    # Create first user
    response = client.post("/users", json=user_data)
    assert response.status_code == 201
    
    # Try to create second user with same email
    response = client.post("/users", json=user_data)
    assert response.status_code == 400
    assert response.json()["detail"] == "Email already registered"
```

## Best Practices

### Code Quality
- Use type hints for all function parameters and return values
- Follow PEP 8 style guidelines (enforced by ruff)
- Write docstrings for public functions and classes
- Use meaningful variable and function names
- Keep functions small and focused on a single responsibility

### Error Handling
- Use proper exception handling with specific exception types
- Return meaningful error messages without exposing sensitive information
- Use HTTP status codes appropriately in web applications
- Log errors for debugging but don't expose them to users

### Security
- Never hardcode secrets or credentials in source code
- Use environment variables for configuration
- Validate and sanitize all user inputs
- Use HTTPS in production
- Implement proper authentication and authorization

### Testing
- Write unit tests for all business logic
- Use fixtures for test data setup
- Test both positive and negative scenarios
- Aim for high test coverage
- Use mocking for external dependencies

### Performance
- Use async/await for I/O operations
- Implement proper database connection pooling
- Use caching where appropriate
- Monitor application performance
- Profile code to identify bottlenecks

## References
- [uv Documentation](https://docs.astral.sh/uv/)
- [Python Documentation](https://docs.python.org/3/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://docs.pydantic.dev/)
- [pytest Documentation](https://docs.pytest.org/)
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [MyPy Documentation](https://mypy.readthedocs.io/)


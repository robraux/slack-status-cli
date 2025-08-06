# Python Project - Development Guidelines

## Build & Run Commands
- Install dependencies: `poetry install`
- Run application: `python -m <project_name>`
- Run single test: `poetry run pytest tests/path/to/test_file.py::TestClass::test_function -v`
- Run all tests: `poetry run pytest tests`
- Lint: `poetry run ruff .`
- Type check: `poetry run mypy <project_name>`
- Format: `poetry run black .`

## Code Style Guidelines
- Use **type hints** everywhere (functions, classes, variables)
- **Imports**: Use isort order, with absolute imports
- **Naming**: snake_case for variables/functions, PascalCase for classes
- Use descriptive names with auxiliary verbs (is_active, has_permission)
- **Async**: Use `async def` for I/O operations; `await` all async calls
- **Errors**: Handle errors at function start; use early returns and guard clauses
- Use **PEP 257 docstrings** for all functions and classes
- **String formatting**: Always use f-strings
- Line length: 120 characters maximum
- **Tests**: Write unique test filenames; use `@pytest.mark.anyio` for async tests

## Development Tools
- **Poetry** for dependency management and virtual environments
- **Ruff** for fast linting and code quality checks
- **Black** for consistent code formatting
- **mypy** for static type checking
- **pytest** for testing with async support via anyio

## Best Practices
- Follow **single responsibility principle** for functions and classes
- Use **composition over inheritance** where appropriate
- Implement proper **error handling** with specific exception types
- Write **comprehensive tests** for all public interfaces
- Use **dependency injection** for better testability
- Follow **12-factor app principles** for configuration management

## Project Structure (Customize as needed)
```
project/
├── src/<project_name>/     # Main application code
├── tests/                  # Test files
├── docs/                   # Documentation
├── pyproject.toml          # Poetry configuration
└── README.md              # Project documentation
```

## Additional Guidelines (Project-Specific)
<!-- Add project-specific guidelines below -->

### Architecture Notes
<!-- Document your project's architecture choices here -->

### Detailed Patterns
<!-- Add imports to detailed pattern files as needed -->

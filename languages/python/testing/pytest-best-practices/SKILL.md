# pytest Best Practices

## Description
Guidelines for writing effective pytest tests that are maintainable, fast, and provide clear failure messages.

## When to Use
- Writing new unit or integration tests
- Refactoring existing tests
- Setting up test fixtures
- Mocking external dependencies

## How to Use

1. **Organize tests by convention**: Place tests in `tests/` directory, mirror source structure
2. **Use descriptive names**: Test function names should describe expected behavior
3. **Follow Arrange-Act-Assert**: Set up data, perform action, verify results
4. **Use fixtures for shared setup**: Extract common setup into fixtures
5. **Parametrize for data-driven tests**: Test multiple inputs with `@pytest.mark.parametrize`

## Examples

### Descriptive Test Names
```python
def test_user_registration_creates_account_with_valid_email():
    ...

def test_api_returns_401_when_token_expired():
    ...
```

### Using Fixtures
```python
@pytest.fixture
def auth_token():
    return generate_test_token(user_id="test-user")

def test_protected_endpoint_requires_auth(client, auth_token):
    response = client.get("/api/user", headers={"Authorization": f"Bearer {auth_token}"})
    assert response.status_code == 200
```

### Parametrized Tests
```python
@pytest.mark.parametrize("input,expected", [
    ("user@example.com", True),
    ("invalid-email", False),
    ("", False),
])
def test_email_validation(input, expected):
    assert validate_email(input) == expected
```

### Using Mocks
```python
from unittest.mock import patch

@patch("mymodule.external_api")
def test_handles_api_failure(mock_api):
    mock_api.get_user.side_effect = APIError("Service unavailable")
    
    result = get_user_profile("user123")
    
    assert result is None
    assert mock_api.get_user.called_once()
```

## Best Practices

- Keep tests independent — no shared state between tests
- Use `pytest.ini` or `pyproject.toml` for configuration
- Run tests in isolation: `pytest -x` stops on first failure
- Use `pytest.fixture(scope="session")` only when necessary
- Name fixtures descriptively: `db_connection` not `db`

## Related Skills
- [Unit & Integration Testing](../testing/unit-integration/) — Broader testing strategies

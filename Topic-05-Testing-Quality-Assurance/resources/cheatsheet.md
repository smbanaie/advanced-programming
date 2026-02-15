# Testing Cheatsheet - Python Unit Testing & Quality Assurance

**Topic 5**: Testing & Quality Assurance
**Quick Reference**: unittest framework, pytest, coverage

---

## 🧪 unittest Framework Basics

### Test Case Structure

```python
import unittest

class TestCalculator(unittest.TestCase):
    """Test cases for Calculator class."""

    def setUp(self):
        """Run before each test method."""
        self.calc = Calculator()

    def tearDown(self):
        """Run after each test method."""
        pass

    def test_add_positive_numbers(self):
        """Test adding positive numbers."""
        result = self.calc.add(2, 3)
        self.assertEqual(result, 5)

    def test_add_with_zero(self):
        """Test adding with zero."""
        result = self.calc.add(5, 0)
        self.assertEqual(result, 5)

if __name__ == '__main__':
    unittest.main()
```

### Common Assertions

```python
# Equality
self.assertEqual(a, b)          # a == b
self.assertNotEqual(a, b)       # a != b

# Boolean
self.assertTrue(x)              # bool(x) is True
self.assertFalse(x)             # bool(x) is False

# Membership
self.assertIn(a, b)             # a in b
self.assertNotIn(a, b)          # a not in b

# Identity
self.assertIs(a, b)             # a is b
self.assertIsNot(a, b)          # a is not b

# Types
self.assertIsInstance(obj, cls) # isinstance(obj, cls)
self.assertIsNotInstance(obj, cls)

# Exceptions
with self.assertRaises(ValueError):
    function_that_raises_error()

# Collections
self.assertListEqual(a, b)
self.assertDictEqual(a, b)
self.assertSetEqual(a, b)

# Numeric
self.assertAlmostEqual(a, b, places=7)  # For floating point
self.assertGreater(a, b)       # a > b
self.assertLessEqual(a, b)     # a <= b
```

---

## 🎯 AAA Testing Pattern

### Structure Every Test

```python
def test_calculate_total_price(self):
    """Test calculating total price with tax."""
    # Arrange: Set up test data and preconditions
    items = [
        {"name": "Widget", "price": 10.00, "quantity": 2},
        {"name": "Gadget", "price": 15.50, "quantity": 1}
    ]
    tax_rate = 0.08

    # Act: Execute the code being tested
    total = calculate_total_price(items, tax_rate)

    # Assert: Verify the results
    expected = (10.00 * 2 + 15.50 * 1) * (1 + tax_rate)  # 39.32
    self.assertAlmostEqual(total, expected, places=2)
```

### Test Method Naming

```python
# Good: Descriptive and specific
def test_calculate_average_with_positive_numbers(self):
    pass

def test_user_registration_fails_with_invalid_email(self):
    pass

def test_process_payment_succeeds_with_valid_card(self):
    pass

# Bad: Vague or unclear
def test_calculate(self):       # What aspect?
    pass

def test_payment(self):          # What scenario?
    pass

def test_method(self):           # What method?
    pass
```

---

## 🧪 Running Tests

### Command Line

```bash
# Run all tests in file
python test_calculator.py

# Run with verbose output
python -m unittest test_calculator.py -v

# Run specific test class
python -m unittest TestCalculator -v

# Run specific test method
python -m unittest TestCalculator.test_add -v

# Run tests in directory
python -m unittest discover -s tests -v

# Run with buffer (capture stdout)
python -m unittest discover -s tests --buffer
```

### Test Discovery

```
project/
├── src/
│   └── calculator.py
└── tests/
    ├── test_calculator.py    # TestCase classes
    ├── test_utils.py
    └── test_integration.py
```

### Programmatic Test Running

```python
import unittest

# Run specific tests
suite = unittest.TestSuite()
suite.addTest(TestCalculator('test_add'))
suite.addTest(TestCalculator('test_subtract'))

runner = unittest.TextTestRunner(verbosity=2)
result = runner.run(suite)

# Check results
if result.wasSuccessful():
    print("All tests passed!")
else:
    print(f"Failed: {len(result.failures)}, Errors: {len(result.errors)}")
```

---

## 📊 Code Coverage

### Installing Coverage

```bash
pip install coverage
```

### Running with Coverage

```bash
# Run tests with coverage
coverage run -m unittest discover -s tests

# Generate text report
coverage report

# Generate detailed report with missing lines
coverage report -m

# Generate HTML report
coverage html

# View specific file coverage
coverage report --include="calculator.py"

# Exclude files from coverage
coverage run --omit="test_*.py" -m unittest discover
```

### Coverage Configuration

**pyproject.toml:**
```toml
[tool.coverage.run]
source = ["src"]
omit = ["tests/*", "*/venv/*"]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError"
]
```

**.coveragerc:**
```ini
[run]
source = src
omit = */tests/*,*/venv/*

[report]
exclude_lines =
    pragma: no cover
    def __repr__
    raise AssertionError
    raise NotImplementedError
```

---

## 🐛 Debugging Tests

### Adding Debug Information

```python
def test_complex_calculation(self):
    """Test with debug information."""
    # Arrange
    data = [1, 2, 3, 4, 5, 10]
    expected = 7.0

    # Act
    result = calculate_average(data)
    print(f"Input: {data}")
    print(f"Expected: {expected}")
    print(f"Actual: {result}")

    # Assert
    self.assertEqual(result, expected,
                    f"Average of {data} should be {expected}, got {result}")
```

### Using subTest for Multiple Cases

```python
def test_calculate_discount(self):
    """Test discount calculation with multiple cases."""
    test_cases = [
        (100, 10, 90),    # price, discount, expected
        (50, 25, 37.5),
        (200, 0, 200),
        (75, 50, 37.5)
    ]

    for price, discount, expected in test_cases:
        with self.subTest(price=price, discount=discount):
            result = calculate_discount(price, discount)
            self.assertEqual(result, expected)
```

### Failing Test Analysis

```python
# Common failure patterns
def test_common_failures(self):
    """Examples of common test failure patterns."""

    # Floating point precision
    result = 0.1 + 0.2
    self.assertAlmostEqual(result, 0.3, places=7)  # Not self.assertEqual

    # List/dict comparison
    result = get_user_list()
    expected = [{"name": "Alice"}, {"name": "Bob"}]
    self.assertListEqual(result, expected)  # Not self.assertEqual

    # Exception type checking
    with self.assertRaises(ValueError) as cm:  # Capture exception
        risky_function()
    self.assertIn("invalid", str(cm.exception).lower())  # Check message
```

---

## 🧪 Testing Different Scenarios

### Happy Path Testing

```python
def test_user_registration_success(self):
    """Test successful user registration."""
    # Arrange
    user_service = UserService()
    valid_data = {
        "username": "john_doe",
        "email": "john@example.com",
        "password": "secure123"
    }

    # Act
    result = user_service.register_user(valid_data)

    # Assert
    self.assertTrue(result.success)
    self.assertEqual(result.user.username, "john_doe")
    self.assertIsNotNone(result.user.id)
```

### Edge Case Testing

```python
def test_calculator_edge_cases(self):
    """Test calculator with edge cases."""
    calc = Calculator()

    # Zero operations
    self.assertEqual(calc.add(0, 5), 5)
    self.assertEqual(calc.divide(10, 1), 10)

    # Large numbers (if applicable)
    large_result = calc.multiply(1000, 1000)
    self.assertEqual(large_result, 1000000)

    # Boundary values
    self.assertEqual(calc.factorial(1), 1)  # Minimum
    # self.assertEqual(calc.factorial(100), very_large_number)  # Maximum?
```

### Error Case Testing

```python
def test_division_error_handling(self):
    """Test division with error conditions."""
    calc = Calculator()

    # Division by zero
    with self.assertRaises(ZeroDivisionError):
        calc.divide(10, 0)

    # Invalid input types
    with self.assertRaises(TypeError):
        calc.divide("10", 2)

    # Both error conditions
    with self.assertRaises((ZeroDivisionError, TypeError)):
        calc.divide("10", 0)
```

### Integration Testing

```python
def test_user_service_integration(self):
    """Test UserService with database integration."""
    # Arrange
    db = MockDatabase()  # Mock or test database
    email_service = MockEmailService()
    user_service = UserService(db, email_service)

    # Act
    user = user_service.register_user("alice", "alice@test.com")

    # Assert
    self.assertIsNotNone(user.id)
    self.assertEqual(user.email, "alice@test.com")

    # Verify interactions
    db.save_user.assert_called_once()
    email_service.send_welcome.assert_called_once()
```

---

## 🧰 Test Fixtures and Setup

### setUp and tearDown

```python
class TestDatabase(unittest.TestCase):

    def setUp(self):
        """Set up test database before each test."""
        self.db = TestDatabase()
        self.db.create_tables()
        self.sample_user = self.db.create_user("test@example.com")

    def tearDown(self):
        """Clean up after each test."""
        self.db.drop_tables()

    def test_user_creation(self):
        """Test user creation."""
        user = self.db.create_user("new@example.com")
        self.assertIsNotNone(user.id)

    def test_user_retrieval(self):
        """Test user retrieval."""
        user = self.db.get_user(self.sample_user.id)
        self.assertEqual(user.email, "test@example.com")
```

### Class-Level Setup

```python
class TestAPICalls(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        """Set up expensive resources once for all tests."""
        cls.api_client = APIClient()
        cls.test_server = start_test_server()

    @classmethod
    def tearDownClass(cls):
        """Clean up expensive resources."""
        cls.test_server.stop()

    def test_api_endpoint_a(self):
        """Test API endpoint A."""
        response = self.api_client.get("/endpoint/a")
        self.assertEqual(response.status_code, 200)

    def test_api_endpoint_b(self):
        """Test API endpoint B."""
        response = self.api_client.get("/endpoint/b")
        self.assertEqual(response.status_code, 200)
```

---

## 🎭 Mocking and Patching

### Using unittest.mock

```python
import unittest.mock as mock

class TestEmailService(unittest.TestCase):

    def test_send_welcome_email(self):
        """Test sending welcome email."""
        # Arrange
        email_service = EmailService()
        user = User("john@example.com", "John Doe")

        # Mock the actual email sending
        with mock.patch.object(email_service, '_send_email') as mock_send:
            mock_send.return_value = True

            # Act
            result = email_service.send_welcome(user)

            # Assert
            self.assertTrue(result)
            mock_send.assert_called_once_with(
                "john@example.com",
                "Welcome!",
                "Welcome to our service, John Doe!"
            )

    def test_email_failure_handling(self):
        """Test handling email sending failure."""
        email_service = EmailService()

        with mock.patch.object(email_service, '_send_email') as mock_send:
            mock_send.return_value = False  # Simulate failure

            with self.assertRaises(EmailError):
                email_service.send_notification("user@test.com", "Test")
```

### Mocking External Dependencies

```python
def test_api_call_with_timeout(self):
    """Test API call that times out."""
    api_client = APIClient()

    with mock.patch('requests.get') as mock_get:
        # Configure mock to raise timeout
        mock_get.side_effect = requests.Timeout()

        # Act & Assert
        with self.assertRaises(APIError):
            api_client.fetch_data()

def test_database_connection_failure(self):
    """Test database connection failure."""
    db_service = DatabaseService()

    with mock.patch('psycopg2.connect') as mock_connect:
        mock_connect.side_effect = psycopg2.OperationalError("Connection failed")

        with self.assertRaises(DatabaseError):
            db_service.connect()
```

---

## 📋 Test Organization Best Practices

### Test File Structure

```
tests/
├── __init__.py
├── test_models.py       # Model tests
├── test_services.py     # Service layer tests
├── test_views.py        # View/API tests
├── test_integration.py  # Integration tests
├── fixtures/
│   ├── sample_data.json
│   └── test_database.db
└── helpers/
    ├── test_helpers.py
    └── mock_factories.py
```

### Test Naming Conventions

```python
class TestUserModel(unittest.TestCase):
    """Tests for User model."""

    # Unit tests
    def test_user_creation_with_valid_data(self): pass
    def test_user_creation_fails_with_invalid_email(self): pass

    # Integration tests
    def test_user_save_to_database(self): pass
    def test_user_load_from_database(self): pass

class TestUserService(unittest.TestCase):
    """Tests for UserService."""

    def test_register_user_success(self): pass
    def test_register_user_fails_duplicate_email(self): pass

class TestUserAPI(unittest.TestCase):
    """Tests for User API endpoints."""

    def test_get_user_returns_user_data(self): pass
    def test_create_user_validates_input(self): pass
```

---

## 🎯 Key Takeaways

1. **AAA Pattern**: Arrange test data, Act by executing code, Assert expected results
2. **Descriptive Names**: Test methods should clearly describe what they test
3. **Test Isolation**: Each test should be independent with its own setup
4. **Coverage Goals**: Aim for 80%+ coverage, focus on critical paths
5. **Edge Cases**: Test boundaries, error conditions, and unusual inputs
6. **Mocking**: Use mocks to isolate code under test from external dependencies
7. **Assertions**: Choose appropriate assertion methods for different verification needs

---

**Cheatsheet Version**: 1.0
**Last Updated**: February 2026
**Pages**: 1
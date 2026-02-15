# Tutorial 1: Unit Testing Fundamentals

**Section**: 1 - Unit Testing Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-4 (Basic Python, OOP)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Write Unit Tests**: Create test cases using unittest framework
2. **Apply AAA Pattern**: Structure tests with Arrange, Act, Assert phases
3. **Handle Test Failures**: Understand and fix common testing issues
4. **Test Different Scenarios**: Write tests for success and failure cases
5. **Organize Test Suites**: Structure and run multiple test cases
6. **Debug with Tests**: Use tests to identify and isolate code issues

---

## 📚 Table of Contents

1. [Why Unit Testing Matters](#why-unit-testing-matters)
2. [unittest Framework Basics](#unittest-framework-basics)
3. [Writing Your First Test](#writing-your-first-test)
4. [The AAA Testing Pattern](#the-aaa-testing-pattern)
5. [Common Assertions](#common-assertions)
6. [Testing Different Scenarios](#testing-different-scenarios)
7. [Test Organization](#test-organization)
8. [Running Tests](#running-tests)
9. [Debugging with Tests](#debugging-with-tests)
10. [Best Practices](#best-practices)

---

## 🤔 Why Unit Testing Matters

Unit testing is the practice of testing individual units (functions, methods, classes) of code to ensure they work correctly. It's a fundamental practice in professional software development.

### Benefits of Unit Testing

```python
# Without testing - risky changes
def calculate_discount(price, discount_percent):
    return price * (1 - discount_percent / 100)

# User reports: "Discount calculation is wrong!"
# Developer changes code blindly...
def calculate_discount(price, discount_percent):
    return price - (discount_percent / 100)  # Bug introduced!

# With testing - safe changes
def test_calculate_discount():
    # Test various scenarios
    assert calculate_discount(100, 10) == 90
    assert calculate_discount(50, 25) == 37.5
    assert calculate_discount(200, 0) == 200

# Now changes can be made safely with confidence
```

**Key Benefits:**
- **Bug Prevention**: Catch bugs before they reach production
- **Code Documentation**: Tests serve as usage examples
- **Refactoring Safety**: Tests ensure changes don't break existing functionality
- **Design Improvement**: Testing encourages better code design
- **Regression Prevention**: Tests prevent old bugs from reappearing

### What is a Unit Test?

A unit test is a test that:
- Tests a single unit of code (function, method, class)
- Is isolated from other units (no external dependencies)
- Is automated and repeatable
- Has a clear pass/fail outcome
- Is fast to execute

---

## 🧪 unittest Framework Basics

Python's `unittest` module is the built-in testing framework. It provides a `TestCase` class that you inherit from to create your tests.

### Importing unittest

```python
import unittest

# Or import specific components
from unittest import TestCase, main
```

### Basic Test Structure

```python
import unittest

class MyTestCase(unittest.TestCase):
    """A test case class containing test methods."""

    def test_something(self):
        """Test method that verifies specific behavior."""
        # Test code goes here
        self.assertEqual(2 + 2, 4)

    def test_something_else(self):
        """Another test method."""
        # More test code
        self.assertTrue(True)

if __name__ == '__main__':
    unittest.main()
```

### Test Method Naming

```python
class CalculatorTestCase(unittest.TestCase):

    def test_add_positive_numbers(self):
        # Good: describes what is being tested
        pass

    def test_addition(self):
        # OK but less specific
        pass

    def test_method(self):
        # Bad: doesn't describe what is tested
        pass
```

---

## 🏗️ Writing Your First Test

Let's write a test for a simple function.

### Code to Test

```python
# calculator.py
def add_numbers(a, b):
    """Add two numbers together."""
    return a + b

def multiply_numbers(a, b):
    """Multiply two numbers."""
    return a * b

def divide_numbers(a, b):
    """Divide a by b."""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

### Writing Tests

```python
# test_calculator.py
import unittest
from calculator import add_numbers, multiply_numbers, divide_numbers

class CalculatorTestCase(unittest.TestCase):
    """Test cases for calculator functions."""

    def test_add_numbers_basic(self):
        """Test basic addition functionality."""
        result = add_numbers(2, 3)
        self.assertEqual(result, 5)

    def test_add_numbers_negative(self):
        """Test addition with negative numbers."""
        result = add_numbers(-5, 10)
        self.assertEqual(result, 5)

    def test_multiply_numbers_basic(self):
        """Test basic multiplication."""
        result = multiply_numbers(4, 5)
        self.assertEqual(result, 20)

    def test_divide_numbers_basic(self):
        """Test basic division."""
        result = divide_numbers(10, 2)
        self.assertEqual(result, 5)

    def test_divide_by_zero(self):
        """Test division by zero raises ValueError."""
        with self.assertRaises(ValueError):
            divide_numbers(10, 0)

if __name__ == '__main__':
    unittest.main()
```

---

## 🎯 The AAA Testing Pattern

The AAA pattern (Arrange-Act-Assert) is a standard way to structure unit tests for clarity and consistency.

### AAA Pattern Structure

```python
def test_function_name(self):
    # Arrange: Set up test data and preconditions
    test_input = "hello"
    expected_output = "HELLO"

    # Act: Execute the code being tested
    actual_output = test_input.upper()

    # Assert: Verify the results
    self.assertEqual(actual_output, expected_output)
```

### Complete AAA Example

```python
import unittest

class StringProcessor:
    """A class to demonstrate testing with AAA pattern."""

    def capitalize_words(self, text):
        """Capitalize the first letter of each word."""
        return ' '.join(word.capitalize() for word in text.split())

    def count_words(self, text):
        """Count the number of words in text."""
        return len(text.split())

    def remove_punctuation(self, text):
        """Remove common punctuation from text."""
        import string
        return text.translate(str.maketrans('', '', string.punctuation))

class StringProcessorTestCase(unittest.TestCase):

    def setUp(self):
        """Set up test fixtures (runs before each test)."""
        self.processor = StringProcessor()

    def test_capitalize_words_simple(self):
        """Test capitalizing words in simple sentence."""
        # Arrange
        input_text = "hello world"
        expected = "Hello World"

        # Act
        result = self.processor.capitalize_words(input_text)

        # Assert
        self.assertEqual(result, expected)

    def test_count_words_basic(self):
        """Test counting words in basic sentence."""
        # Arrange
        input_text = "The quick brown fox"
        expected_count = 4

        # Act
        result = self.processor.count_words(input_text)

        # Assert
        self.assertEqual(result, expected_count)

    def test_remove_punctuation(self):
        """Test removing punctuation from text."""
        # Arrange
        input_text = "Hello, world! How are you?"
        expected = "Hello world How are you"

        # Act
        result = self.processor.remove_punctuation(input_text)

        # Assert
        self.assertEqual(result, expected)

    def test_capitalize_empty_string(self):
        """Test capitalizing empty string."""
        # Arrange
        input_text = ""
        expected = ""

        # Act
        result = self.processor.capitalize_words(input_text)

        # Assert
        self.assertEqual(result, expected)

    def test_count_words_empty_string(self):
        """Test counting words in empty string."""
        # Arrange
        input_text = ""
        expected_count = 0

        # Act
        result = self.processor.count_words(input_text)

        # Assert
        self.assertEqual(result, expected_count)
```

---

## ✅ Common Assertions

unittest provides many assertion methods to verify different types of conditions.

### Equality Assertions

```python
class EqualityTestCase(unittest.TestCase):

    def test_equality_assertions(self):
        # assertEqual(a, b) - a == b
        self.assertEqual(2 + 2, 4)
        self.assertEqual("hello".upper(), "HELLO")

        # assertNotEqual(a, b) - a != b
        self.assertNotEqual(2 + 2, 5)
        self.assertNotEqual("hello", "world")
```

### Boolean Assertions

```python
class BooleanTestCase(unittest.TestCase):

    def test_boolean_assertions(self):
        # assertTrue(x) - bool(x) is True
        self.assertTrue(5 > 3)
        self.assertTrue(len("hello") == 5)
        self.assertTrue(bool("non-empty"))

        # assertFalse(x) - bool(x) is False
        self.assertFalse(5 < 3)
        self.assertFalse(bool(""))
        self.assertFalse(bool(0))
```

### Membership Assertions

```python
class MembershipTestCase(unittest.TestCase):

    def test_membership_assertions(self):
        numbers = [1, 2, 3, 4, 5]

        # assertIn(a, b) - a in b
        self.assertIn(3, numbers)
        self.assertIn("h", "hello")

        # assertNotIn(a, b) - a not in b
        self.assertNotIn(6, numbers)
        self.assertNotIn("z", "hello")
```

### Exception Assertions

```python
class ExceptionTestCase(unittest.TestCase):

    def test_exception_assertions(self):
        # assertRaises(exception_type) - Context manager
        with self.assertRaises(ValueError):
            int("not_a_number")

        with self.assertRaises(ZeroDivisionError):
            1 / 0

        # assertRaises(exception_type, callable, *args) - Function call
        self.assertRaises(ValueError, int, "not_a_number")

    def test_no_exception(self):
        """Test that no exception is raised."""
        # If this doesn't raise an exception, test passes
        result = int("123")
        self.assertEqual(result, 123)
```

### Type and Identity Assertions

```python
class TypeIdentityTestCase(unittest.TestCase):

    def test_type_assertions(self):
        # assertIsInstance(obj, cls) - isinstance(obj, cls)
        self.assertIsInstance(42, int)
        self.assertIsInstance("hello", str)
        self.assertIsInstance([], list)

        # assertIsNotInstance(obj, cls)
        self.assertIsNotInstance(42, str)
        self.assertIsNotInstance("hello", int)

    def test_identity_assertions(self):
        a = [1, 2, 3]
        b = a
        c = [1, 2, 3]

        # assertIs(a, b) - a is b (same object)
        self.assertIs(a, b)

        # assertIsNot(a, b) - a is not b
        self.assertIsNot(a, c)  # Different objects, same content
```

### Collection Assertions

```python
class CollectionTestCase(unittest.TestCase):

    def test_list_assertions(self):
        # assertListEqual(a, b) - Lists are equal
        self.assertListEqual([1, 2, 3], [1, 2, 3])

        # assertTupleEqual(a, b)
        self.assertTupleEqual((1, 2), (1, 2))

        # assertSetEqual(a, b)
        self.assertSetEqual({1, 2, 3}, {3, 2, 1})

        # assertDictEqual(a, b)
        self.assertDictEqual({"a": 1, "b": 2}, {"a": 1, "b": 2})
```

---

## 🎭 Testing Different Scenarios

### Happy Path Testing

```python
class HappyPathTestCase(unittest.TestCase):

    def test_calculator_happy_path(self):
        """Test calculator with normal inputs."""
        calc = Calculator()

        # Normal addition
        self.assertEqual(calc.add(5, 3), 8)

        # Normal subtraction
        self.assertEqual(calc.subtract(10, 4), 6)

        # Normal multiplication
        self.assertEqual(calc.multiply(6, 7), 42)

        # Normal division
        self.assertEqual(calc.divide(15, 3), 5)
```

### Edge Case Testing

```python
class EdgeCaseTestCase(unittest.TestCase):

    def test_calculator_edge_cases(self):
        """Test calculator with edge case inputs."""
        calc = Calculator()

        # Zero operations
        self.assertEqual(calc.add(0, 5), 5)
        self.assertEqual(calc.multiply(0, 10), 0)

        # Negative numbers
        self.assertEqual(calc.add(-3, 7), 4)
        self.assertEqual(calc.multiply(-4, -5), 20)

        # Large numbers
        self.assertEqual(calc.add(999999, 1), 1000000)
```

### Error Case Testing

```python
class ErrorCaseTestCase(unittest.TestCase):

    def test_calculator_error_cases(self):
        """Test calculator error handling."""
        calc = Calculator()

        # Division by zero
        with self.assertRaises(ZeroDivisionError):
            calc.divide(10, 0)

        # Invalid input types
        with self.assertRaises(TypeError):
            calc.add("hello", 5)

        # Overflow (if applicable)
        # Very large numbers might cause OverflowError in some contexts
```

### Boundary Testing

```python
class BoundaryTestCase(unittest.TestCase):

    def test_string_processor_boundaries(self):
        """Test string processor with boundary conditions."""
        processor = StringProcessor()

        # Empty string
        self.assertEqual(processor.capitalize_words(""), "")

        # Single character
        self.assertEqual(processor.capitalize_words("a"), "A")

        # Single word
        self.assertEqual(processor.capitalize_words("hello"), "Hello")

        # Very long string
        long_text = "word " * 1000
        result = processor.capitalize_words(long_text.rstrip())
        self.assertTrue(result.startswith("Word"))

        # String with only spaces
        self.assertEqual(processor.capitalize_words("   "), "")

        # String with mixed whitespace
        self.assertEqual(processor.capitalize_words("  hello   world  "), "Hello World")
```

---

## 📁 Test Organization

### Test File Structure

```
project/
├── src/
│   ├── calculator.py
│   └── string_processor.py
└── tests/
    ├── test_calculator.py
    ├── test_string_processor.py
    └── __init__.py
```

### Test Class Organization

```python
import unittest
from calculator import Calculator
from string_processor import StringProcessor

class TestCalculator(unittest.TestCase):
    """Test cases for Calculator class."""

    def setUp(self):
        """Set up test fixtures before each test method."""
        self.calc = Calculator()

    def tearDown(self):
        """Clean up after each test method."""
        pass

    # Test methods...

class TestStringProcessor(unittest.TestCase):
    """Test cases for StringProcessor class."""

    def setUp(self):
        self.processor = StringProcessor()

    # Test methods...

if __name__ == '__main__':
    unittest.main()
```

### Test Suite Organization

```python
import unittest

def create_test_suite():
    """Create a test suite with specific test cases."""
    suite = unittest.TestSuite()

    # Add specific test cases
    suite.addTest(TestCalculator('test_add'))
    suite.addTest(TestCalculator('test_subtract'))
    suite.addTest(TestStringProcessor('test_capitalize'))

    return suite

def run_specific_tests():
    """Run only specific tests."""
    loader = unittest.TestLoader()

    # Load tests from specific classes
    calc_tests = loader.loadTestsFromTestCase(TestCalculator)
    string_tests = loader.loadTestsFromTestCase(TestStringProcessor)

    # Combine into suite
    suite = unittest.TestSuite([calc_tests, string_tests])

    runner = unittest.TextTestRunner(verbosity=2)
    runner.run(suite)

if __name__ == '__main__':
    run_specific_tests()
```

---

## ▶️ Running Tests

### Command Line Execution

```bash
# Run all tests in a file
python test_calculator.py

# Run tests with verbose output
python -m unittest test_calculator.py -v

# Run specific test method
python -m unittest test_calculator.TestCalculator.test_add -v

# Run all tests in directory
python -m unittest discover -s tests -v

# Run with different output formats
python -m unittest discover -s tests --buffer  # Capture stdout
```

### IDE Integration

Most IDEs (PyCharm, VS Code, etc.) have built-in test runners:

```bash
# PyCharm: Right-click on test file/class/method and "Run"
# VS Code: Use Testing extension, click run button above test methods
# Other IDEs: Similar integration available
```

### Continuous Integration

```yaml
# .github/workflows/test.yml (GitHub Actions example)
name: Test
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.8'
    - name: Install dependencies
      run: pip install -r requirements.txt
    - name: Run tests
      run: python -m unittest discover -s tests -v
```

---

## 🔍 Debugging with Tests

### Using Test Failures to Debug

```python
# Failing test output example:
# ======================================================================
# FAIL: test_calculate_average (test_calculator.TestCalculator)
# ----------------------------------------------------------------------
# Traceback (most recent call last):
#   File "test_calculator.py", line 45, in test_calculate_average
#     self.assertEqual(calc.calculate_average([1, 2, 3, 4, 5]), 3)
# AssertionError: 3.5 != 3
# ----------------------------------------------------------------------

class Calculator:
    def calculate_average(self, numbers):
        if not numbers:
            return 0
        return sum(numbers) / len(numbers)  # Correct: returns 3.0, not 3

# The test revealed the method was returning float instead of int
# Fix: return int(sum(numbers) / len(numbers)) if needed, or update test
```

### Adding Debug Information

```python
class DebugTestCase(unittest.TestCase):

    def test_complex_calculation(self):
        """Test with debug information."""
        calc = Calculator()
        data = [1, 2, 3, 4, 5]

        result = calc.calculate_average(data)
        expected = 3.0

        # Add debug info for failed tests
        self.assertEqual(
            result, expected,
            f"calculate_average({data}) returned {result}, expected {expected}"
        )

    def test_with_intermediate_checks(self):
        """Test with intermediate assertions for debugging."""
        calc = Calculator()
        numbers = [10, 20, 30]

        # Check intermediate steps
        total = sum(numbers)
        self.assertEqual(total, 60, "Sum calculation failed")

        count = len(numbers)
        self.assertEqual(count, 3, "Count calculation failed")

        result = calc.calculate_average(numbers)
        self.assertEqual(result, 20.0, "Average calculation failed")
```

### Using setUp and tearDown for Debugging

```python
class DebugSetupTestCase(unittest.TestCase):

    def setUp(self):
        """Set up test environment and log start."""
        self.calc = Calculator()
        print(f"\nStarting test: {self._testMethodName}")

    def tearDown(self):
        """Clean up and log completion."""
        print(f"Completed test: {self._testMethodName}")

    def test_addition_debug(self):
        """Test addition with debug output."""
        print(f"Testing addition: 2 + 3")
        result = self.calc.add(2, 3)
        print(f"Result: {result}")
        self.assertEqual(result, 5)
```

---

## 🎯 Best Practices

### Test Quality

```python
# Good: Descriptive test names
def test_calculate_average_with_positive_numbers(self):
    pass

def test_calculate_average_with_empty_list_returns_zero(self):
    pass

# Bad: Vague test names
def test_average(self):  # What aspect of average?
    pass

def test_method(self):   # What method? What behavior?
    pass
```

### Test Isolation

```python
class IsolationTestCase(unittest.TestCase):

    def setUp(self):
        """Create fresh instance for each test."""
        self.calc = Calculator()  # Fresh instance
        self.test_data = [1, 2, 3]  # Fresh data

    def test_add_does_not_affect_other_tests(self):
        """Each test gets a fresh calculator."""
        self.calc.add(5)  # This won't affect other tests
        self.assertEqual(self.calc.get_result(), 5)

    def test_subtract_starts_from_zero(self):
        """Calculator starts fresh for this test too."""
        self.calc.subtract(3)  # Starts from 0, not from previous test
        self.assertEqual(self.calc.get_result(), -3)
```

### Test Readability

```python
# Good: Clear, readable test
def test_user_registration_success(self):
    """Test successful user registration."""
    # Arrange
    user_service = UserService()
    valid_user_data = {
        "username": "john_doe",
        "email": "john@example.com",
        "password": "secure123"
    }

    # Act
    result = user_service.register_user(valid_user_data)

    # Assert
    self.assertTrue(result.success)
    self.assertEqual(result.user.username, "john_doe")
    self.assertIsNotNone(result.user.id)

# Bad: Hard to understand test
def test_reg(self):  # What does "reg" mean?
    u = UserService()
    d = {"u": "j", "e": "j@", "p": "p"}  # What are these abbreviations?
    r = u.reg(d)  # What does "reg" do?
    self.assertTrue(r.s)  # What does "s" represent?
```

### Test Maintenance

```python
class MaintainableTestCase(unittest.TestCase):
    """Tests that are easy to maintain and update."""

    # Use constants for test data
    VALID_EMAIL = "test@example.com"
    INVALID_EMAIL = "not-an-email"

    def test_email_validation_accepts_valid_email(self):
        """Test email validation with valid email."""
        validator = EmailValidator()
        self.assertTrue(validator.is_valid(self.VALID_EMAIL))

    def test_email_validation_rejects_invalid_email(self):
        """Test email validation with invalid email."""
        validator = EmailValidator()
        self.assertFalse(validator.is_valid(self.INVALID_EMAIL))

    # If requirements change, update constants once
    # VALID_EMAIL = "test@newdomain.com"
```

---

## 🎯 Key Takeaways

1. **unittest is Python's Built-in Testing Framework**: Use `TestCase` class for test organization
2. **AAA Pattern**: Arrange test data, Act by calling code, Assert expected results
3. **Descriptive Test Names**: Use clear, specific test method names
4. **Test Isolation**: Each test should be independent with its own setup
5. **Comprehensive Coverage**: Test happy paths, edge cases, and error conditions
6. **Debug with Tests**: Use test failures to identify and fix code issues
7. **Assertions**: Choose appropriate assertion methods for different verification needs

---

## 🔗 Further Reading

- [unittest Documentation](https://docs.python.org/3/library/unittest.html)
- [Testing in Python](https://realpython.com/python-testing/)
- [AAA Testing Pattern](https://medium.com/@pjbgf/title-testing-code-ocd-and-the-aaa-pattern-df133472cbba)

---

## 📝 Practice Exercises

1. **Calculator Testing**: Write comprehensive tests for a calculator class covering all operations
2. **String Utilities**: Test string manipulation functions with various edge cases
3. **List Operations**: Test custom list utility functions with empty lists, duplicates, etc.
4. **Error Handling**: Write tests that verify proper exception handling in your code
5. **Class Integration**: Test how multiple classes work together in a small system

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
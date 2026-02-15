# Workshop 1: Writing Tests for Existing Code

**Section**: 1 - Unit Testing Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 1 (Unit Testing Fundamentals)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Apply unittest Framework**: Write test cases for existing code
2. **Practice AAA Pattern**: Structure tests with proper Arrange-Act-Assert phases
3. **Test Real Code**: Work with actual functions and classes that need testing
4. **Handle Edge Cases**: Identify and test boundary conditions and error scenarios
5. **Debug Test Failures**: Use test results to identify and fix code issues
6. **Build Test Suites**: Organize multiple test cases into comprehensive test suites

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Testing Utility Functions](#exercise-1-testing-utility-functions)
3. [Exercise 2: Testing a Simple Class](#exercise-2-testing-a-simple-class)
4. [Exercise 3: Testing Error Handling](#exercise-3-testing-error-handling)
5. [Exercise 4: Testing Data Processing](#exercise-4-testing-data-processing)
6. [Exercise 5: Comprehensive Test Suite](#exercise-5-comprehensive-test-suite)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-testing-basics
cd workshop-testing-basics

# Create Python files
touch utils.py test_utils.py
touch bank_account.py test_bank_account.py
touch data_processor.py test_data_processor.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate
```

### Project Structure

```
workshop-testing-basics/
├── utils.py              # Utility functions to test
├── test_utils.py         # Tests for utility functions
├── bank_account.py       # Bank account class
├── test_bank_account.py  # Tests for bank account
├── data_processor.py     # Data processing functions
├── test_data_processor.py # Tests for data processing
└── README.md            # Documentation (optional)
```

---

## 🧮 Exercise 1: Testing Utility Functions

**Goal**: Write tests for existing utility functions

### Code to Test (utils.py)

```python
"""Utility functions for various operations."""

def calculate_factorial(n):
    """Calculate the factorial of a number."""
    if n < 0:
        raise ValueError("Factorial is not defined for negative numbers")
    if n == 0 or n == 1:
        return 1

    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

def is_prime(number):
    """Check if a number is prime."""
    if number <= 1:
        return False
    if number <= 3:
        return True
    if number % 2 == 0 or number % 3 == 0:
        return False

    i = 5
    while i * i <= number:
        if number % i == 0 or number % (i + 2) == 0:
            return False
        i += 6
    return True

def find_max_in_list(numbers):
    """Find the maximum value in a list."""
    if not numbers:
        raise ValueError("List cannot be empty")

    max_value = numbers[0]
    for num in numbers:
        if num > max_value:
            max_value = num
    return max_value

def reverse_string(text):
    """Reverse a string."""
    return text[::-1]

def count_vowels(text):
    """Count the number of vowels in a string."""
    vowels = 'aeiouAEIOU'
    count = 0
    for char in text:
        if char in vowels:
            count += 1
    return count
```

### Writing Tests (test_utils.py)

Create comprehensive tests for the utility functions:

```python
import unittest
from utils import (
    calculate_factorial,
    is_prime,
    find_max_in_list,
    reverse_string,
    count_vowels
)

class TestUtils(unittest.TestCase):
    """Test cases for utility functions."""

    # TODO: Test calculate_factorial function
    # - Test normal cases (0, 1, 5, 10)
    # - Test edge cases
    # - Test error cases (negative numbers)

    # TODO: Test is_prime function
    # - Test prime numbers (2, 3, 5, 7, 11, 13)
    # - Test non-prime numbers (0, 1, 4, 6, 8, 9, 10)
    # - Test edge cases (negative numbers)

    # TODO: Test find_max_in_list function
    # - Test normal lists ([1, 5, 3, 9, 2])
    # - Test single element lists ([42])
    # - Test negative numbers ([-5, -1, -10])
    # - Test error cases (empty list)

    # TODO: Test reverse_string function
    # - Test normal strings ("hello", "Python")
    # - Test empty string ("")
    # - Test single character ("a")
    # - Test palindromes ("radar")

    # TODO: Test count_vowels function
    # - Test normal strings ("hello", "Python Programming")
    # - Test strings with no vowels ("rhythm", "bcdfg")
    # - Test empty string ("")
    # - Test case sensitivity ("Hello" vs "HELLO")

if __name__ == '__main__':
    unittest.main()
```

**Test your implementation:**
```bash
# Run the tests
python test_utils.py

# Run with verbose output
python test_utils.py -v

# Run specific test
python -m unittest test_utils.TestUtils.test_calculate_factorial -v
```

---

## 🏦 Exercise 2: Testing a Simple Class

**Goal**: Write tests for a bank account class

### Code to Test (bank_account.py)

```python
"""Simple bank account class."""

class BankAccount:
    """A simple bank account with basic operations."""

    def __init__(self, account_number, owner_name, initial_balance=0):
        """Initialize a bank account."""
        self.account_number = account_number
        self.owner_name = owner_name
        self.balance = initial_balance
        self.transactions = []

    def deposit(self, amount):
        """Deposit money into the account."""
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")

        self.balance += amount
        self.transactions.append(f"Deposit: +${amount:.2f}")
        return self.balance

    def withdraw(self, amount):
        """Withdraw money from the account."""
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")

        if amount > self.balance:
            raise ValueError("Insufficient funds")

        self.balance -= amount
        self.transactions.append(f"Withdrawal: -${amount:.2f}")
        return self.balance

    def get_balance(self):
        """Get the current balance."""
        return self.balance

    def get_transaction_history(self):
        """Get the transaction history."""
        return self.transactions.copy()

    def transfer(self, other_account, amount):
        """Transfer money to another account."""
        if not isinstance(other_account, BankAccount):
            raise ValueError("Invalid target account")

        self.withdraw(amount)
        other_account.deposit(amount)
        return True
```

### Writing Tests (test_bank_account.py)

Create comprehensive tests for the BankAccount class:

```python
import unittest
from bank_account import BankAccount

class TestBankAccount(unittest.TestCase):
    """Test cases for BankAccount class."""

    def setUp(self):
        """Set up test fixtures before each test."""
        self.account = BankAccount("12345", "John Doe", 1000)

    def tearDown(self):
        """Clean up after each test."""
        pass

    # TODO: Test account initialization
    # - Test with initial balance
    # - Test with zero initial balance
    # - Test account properties

    # TODO: Test deposit functionality
    # - Test normal deposits
    # - Test deposit of zero/negative amounts (should raise ValueError)
    # - Test balance updates after deposit
    # - Test transaction history after deposit

    # TODO: Test withdrawal functionality
    # - Test normal withdrawals
    # - Test withdrawal of zero/negative amounts (should raise ValueError)
    # - Test withdrawal exceeding balance (should raise ValueError)
    # - Test balance updates after withdrawal
    # - Test transaction history after withdrawal

    # TODO: Test balance queries
    # - Test get_balance returns correct amount
    # - Test balance after multiple operations

    # TODO: Test transaction history
    # - Test empty history initially
    # - Test history after deposits
    # - Test history after withdrawals
    # - Test history is copy (not reference)

    # TODO: Test transfer functionality
    # - Test successful transfers between accounts
    # - Test transfer with insufficient funds
    # - Test transfer to invalid account type
    # - Test balances after transfer
    # - Test transaction histories after transfer

if __name__ == '__main__':
    unittest.main()
```

**Test your implementation:**
```bash
# Run the bank account tests
python test_bank_account.py -v

# Test specific functionality
python -m unittest test_bank_account.TestBankAccount.test_deposit -v
```

---

## ⚠️ Exercise 3: Testing Error Handling

**Goal**: Test error conditions and exception handling

### Enhanced Code with Error Handling (data_processor.py)

```python
"""Data processing functions with error handling."""

def safe_divide(a, b):
    """Safely divide a by b."""
    try:
        return a / b
    except ZeroDivisionError:
        return float('inf') if a > 0 else float('-inf')
    except TypeError:
        raise ValueError("Both arguments must be numbers")

def parse_number(text):
    """Parse a string into a number."""
    if not isinstance(text, str):
        raise TypeError("Input must be a string")

    text = text.strip()
    if not text:
        raise ValueError("Empty string cannot be parsed")

    try:
        # Try integer first
        if '.' not in text:
            return int(text)
        else:
            return float(text)
    except ValueError:
        raise ValueError(f"Invalid number format: {text}")

def find_element_in_list(elements, target):
    """Find the index of target in elements list."""
    if not isinstance(elements, list):
        raise TypeError("First argument must be a list")

    try:
        return elements.index(target)
    except ValueError:
        return -1  # Not found

def merge_dictionaries(dict1, dict2):
    """Merge two dictionaries."""
    if not isinstance(dict1, dict) or not isinstance(dict2, dict):
        raise TypeError("Both arguments must be dictionaries")

    result = dict1.copy()
    result.update(dict2)
    return result

def validate_email(email):
    """Basic email validation."""
    if not isinstance(email, str):
        raise TypeError("Email must be a string")

    if not email or '@' not in email:
        return False

    local, domain = email.split('@', 1)
    if not local or not domain or '.' not in domain:
        return False

    return True
```

### Writing Error Handling Tests (test_data_processor.py)

Create tests that focus on error conditions and edge cases:

```python
import unittest
from data_processor import (
    safe_divide,
    parse_number,
    find_element_in_list,
    merge_dictionaries,
    validate_email
)

class TestDataProcessor(unittest.TestCase):
    """Test cases for data processing functions with error handling."""

    # TODO: Test safe_divide function
    # - Test normal division (10/2 = 5)
    # - Test division by zero (returns inf/-inf based on sign)
    # - Test division with invalid types (raises ValueError)

    # TODO: Test parse_number function
    # - Test valid integers ("42" -> 42)
    # - Test valid floats ("3.14" -> 3.14)
    # - Test invalid formats ("abc", "", "12.34.56")
    # - Test wrong input types (raises TypeError)

    # TODO: Test find_element_in_list function
    # - Test finding existing elements
    # - Test finding non-existing elements (returns -1)
    # - Test with invalid input types (raises TypeError)
    # - Test with empty lists

    # TODO: Test merge_dictionaries function
    # - Test merging two valid dictionaries
    # - Test merging with overlapping keys (second dict wins)
    # - Test with invalid input types (raises TypeError)

    # TODO: Test validate_email function
    # - Test valid emails ("user@example.com", "test.email@domain.co.uk")
    # - Test invalid emails ("", "no-at-sign", "@domain.com", "user@", "user@domain")
    # - Test wrong input types (raises TypeError)

if __name__ == '__main__':
    unittest.main()
```

**Test your error handling:**
```bash
# Run error handling tests
python test_data_processor.py -v

# Test specific error conditions
python -m unittest test_data_processor.TestDataProcessor.test_safe_divide -v
```

---

## 📊 Exercise 4: Testing Data Processing

**Goal**: Test complex data processing logic

### Complex Data Processing Code

Create a more complex data processing module to test:

```python
"""Complex data processing functions."""

from typing import List, Dict, Any, Optional
import statistics

class DataAnalyzer:
    """A class for analyzing numerical data."""

    def __init__(self, data: List[float]):
        """Initialize with a list of numbers."""
        if not isinstance(data, list):
            raise TypeError("Data must be a list")
        if not data:
            raise ValueError("Data list cannot be empty")

        self.data = data.copy()
        self._validate_data()

    def _validate_data(self):
        """Validate that all data is numeric."""
        for item in self.data:
            if not isinstance(item, (int, float)):
                raise TypeError("All data items must be numbers")

    def calculate_mean(self) -> float:
        """Calculate the arithmetic mean."""
        return sum(self.data) / len(self.data)

    def calculate_median(self) -> float:
        """Calculate the median value."""
        sorted_data = sorted(self.data)
        n = len(sorted_data)
        mid = n // 2

        if n % 2 == 0:
            return (sorted_data[mid - 1] + sorted_data[mid]) / 2
        else:
            return sorted_data[mid]

    def calculate_mode(self) -> Optional[float]:
        """Calculate the mode (most frequent value)."""
        if not self.data:
            return None

        # Simple mode calculation
        counts = {}
        for num in self.data:
            counts[num] = counts.get(num, 0) + 1

        max_count = max(counts.values())
        modes = [num for num, count in counts.items() if count == max_count]

        return modes[0] if len(modes) == 1 else None

    def calculate_standard_deviation(self) -> float:
        """Calculate the standard deviation."""
        if len(self.data) < 2:
            raise ValueError("Need at least 2 data points for standard deviation")

        mean = self.calculate_mean()
        variance = sum((x - mean) ** 2 for x in self.data) / (len(self.data) - 1)
        return variance ** 0.5

    def get_summary_stats(self) -> Dict[str, Any]:
        """Get comprehensive summary statistics."""
        return {
            'count': len(self.data),
            'mean': self.calculate_mean(),
            'median': self.calculate_median(),
            'mode': self.calculate_mode(),
            'min': min(self.data),
            'max': max(self.data),
            'range': max(self.data) - min(self.data),
            'std_dev': self.calculate_standard_deviation() if len(self.data) >= 2 else None
        }

def filter_by_threshold(data: List[float], threshold: float, above: bool = True) -> List[float]:
    """Filter data by threshold value."""
    if above:
        return [x for x in data if x > threshold]
    else:
        return [x for x in data if x < threshold]

def normalize_data(data: List[float]) -> List[float]:
    """Normalize data to 0-1 range."""
    if not data:
        return []

    min_val = min(data)
    max_val = max(data)

    if max_val == min_val:
        return [0.5] * len(data)  # All same values

    return [(x - min_val) / (max_val - min_val) for x in data]
```

### Writing Comprehensive Tests

Create thorough tests for the data analysis functionality:

```python
import unittest
from data_analyzer import DataAnalyzer, filter_by_threshold, normalize_data

class TestDataAnalyzer(unittest.TestCase):
    """Comprehensive tests for DataAnalyzer class."""

    def setUp(self):
        """Set up test data."""
        self.test_data = [1, 2, 3, 4, 5]
        self.analyzer = DataAnalyzer(self.test_data)

    # TODO: Test DataAnalyzer initialization
    # - Valid data lists
    # - Empty lists (should raise ValueError)
    # - Non-list inputs (should raise TypeError)
    # - Non-numeric data (should raise TypeError)

    # TODO: Test calculate_mean
    # - Normal data sets
    # - Single element lists
    # - Negative numbers

    # TODO: Test calculate_median
    # - Odd number of elements
    # - Even number of elements
    # - Single element
    # - Sorted vs unsorted input

    # TODO: Test calculate_mode
    # - Clear mode (one most frequent)
    # - No clear mode (multiple equally frequent)
    # - All unique values
    # - Single element

    # TODO: Test calculate_standard_deviation
    # - Normal data sets
    # - Two element lists (minimum required)
    # - Single element (should raise ValueError)

    # TODO: Test get_summary_stats
    # - All statistics are present and correct
    # - Handles edge cases appropriately

class TestDataFunctions(unittest.TestCase):
    """Tests for standalone data processing functions."""

    # TODO: Test filter_by_threshold
    # - Filter above threshold
    # - Filter below threshold
    # - Empty results
    # - All elements match

    # TODO: Test normalize_data
    # - Normal range normalization
    # - All identical values
    # - Empty list
    # - Single element

if __name__ == '__main__':
    unittest.main()
```

**Test your data processing:**
```bash
# Run comprehensive tests
python -m unittest discover -v

# Run specific test classes
python -m unittest test_data_analyzer.TestDataAnalyzer -v
```

---

## 🧪 Exercise 5: Comprehensive Test Suite

**Goal**: Create a complete test suite with proper organization

### Test Suite Organization

Create a master test file that runs all tests:

```python
"""Master test suite for the workshop."""

import unittest
import sys
import os

# Add current directory to path for imports
sys.path.insert(0, os.path.dirname(__file__))

from test_utils import TestUtils
from test_bank_account import TestBankAccount
from test_data_processor import TestDataProcessor
from test_data_analyzer import TestDataAnalyzer, TestDataFunctions

def create_test_suite():
    """Create a comprehensive test suite."""
    loader = unittest.TestLoader()
    suite = unittest.TestSuite()

    # Add all test classes
    test_classes = [
        TestUtils,
        TestBankAccount,
        TestDataProcessor,
        TestDataAnalyzer,
        TestDataFunctions
    ]

    for test_class in test_classes:
        tests = loader.loadTestsFromTestCase(test_class)
        suite.addTests(tests)

    return suite

def run_tests():
    """Run all tests with detailed output."""
    suite = create_test_suite()
    runner = unittest.TextTestRunner(
        verbosity=2,
        buffer=True  # Capture stdout during tests
    )
    result = runner.run(suite)

    # Print summary
    print(f"\n{'='*50}")
    print("TEST SUMMARY"
    print(f"{'='*50}")
    print(f"Tests run: {result.testsRun}")
    print(f"Failures: {len(result.failures)}")
    print(f"Errors: {len(result.errors)}")
    print(f"Skipped: {len(result.skipped)}")

    if result.wasSuccessful():
        print("🎉 ALL TESTS PASSED!")
        return 0
    else:
        print("❌ SOME TESTS FAILED!")
        return 1

if __name__ == '__main__':
    exit_code = run_tests()
    sys.exit(exit_code)
```

### Adding Test Coverage

Install and use coverage to measure test effectiveness:

```bash
# Install coverage
pip install coverage

# Run tests with coverage
coverage run -m unittest discover

# Generate coverage report
coverage report

# Generate HTML coverage report
coverage html
```

**Check your coverage:**
```bash
# View coverage report
coverage report -m

# Open HTML report (if generated)
# open htmlcov/index.html
```

---

## 🏆 Challenge Exercises

### Challenge 1: Test-Driven Development

Implement a new feature using TDD approach:

```python
class ShoppingCart:
    """A shopping cart that needs to be implemented using TDD."""

    def __init__(self):
        # TODO: Initialize cart
        pass

    def add_item(self, item_name, price, quantity=1):
        # TODO: Add item to cart
        pass

    def remove_item(self, item_name):
        # TODO: Remove item from cart
        pass

    def get_total(self):
        # TODO: Calculate total price
        pass

    def apply_discount(self, discount_percent):
        # TODO: Apply discount to total
        pass

# TODO: Write tests FIRST, then implement the methods
# Start with test_add_item, then implement add_item
# Continue with test_get_total, then implement get_total
# And so on...
```

### Challenge 2: Mocking Dependencies

Test code that depends on external services:

```python
import requests

class WeatherService:
    """Service that depends on external API."""

    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.weatherapi.com/v1"

    def get_temperature(self, city):
        """Get current temperature for a city."""
        # Makes actual API call
        pass

# TODO: Write tests that mock the requests.get call
# Use unittest.mock to avoid making real API calls
# Test both successful responses and error conditions
```

### Challenge 3: Integration Testing

Test how multiple classes work together:

```python
# TODO: Create integration tests that test
# - BankAccount with multiple operations
# - DataAnalyzer with various data sets
# - Complete user workflows across multiple classes

# Focus on testing the interactions between components
# rather than testing individual methods in isolation
```

---

## ✅ Solution Code

### Test Utils Solution

```python
import unittest
from utils import (
    calculate_factorial,
    is_prime,
    find_max_in_list,
    reverse_string,
    count_vowels
)

class TestUtils(unittest.TestCase):

    def test_calculate_factorial_normal_cases(self):
        """Test factorial with normal inputs."""
        self.assertEqual(calculate_factorial(0), 1)
        self.assertEqual(calculate_factorial(1), 1)
        self.assertEqual(calculate_factorial(5), 120)
        self.assertEqual(calculate_factorial(10), 3628800)

    def test_calculate_factorial_edge_cases(self):
        """Test factorial edge cases."""
        # Large numbers (be careful with overflow)
        result = calculate_factorial(12)
        self.assertIsInstance(result, int)
        self.assertGreater(result, 0)

    def test_calculate_factorial_errors(self):
        """Test factorial error handling."""
        with self.assertRaises(ValueError):
            calculate_factorial(-1)
        with self.assertRaises(ValueError):
            calculate_factorial(-5)

    def test_is_prime_primes(self):
        """Test prime number detection."""
        primes = [2, 3, 5, 7, 11, 13, 17, 19, 23]
        for prime in primes:
            with self.subTest(prime=prime):
                self.assertTrue(is_prime(prime), f"{prime} should be prime")

    def test_is_prime_non_primes(self):
        """Test non-prime number detection."""
        non_primes = [0, 1, 4, 6, 8, 9, 10, 15, 21]
        for num in non_primes:
            with self.subTest(num=num):
                self.assertFalse(is_prime(num), f"{num} should not be prime")

    def test_is_prime_negative(self):
        """Test prime detection with negative numbers."""
        self.assertFalse(is_prime(-1))
        self.assertFalse(is_prime(-7))

    def test_find_max_in_list_normal(self):
        """Test finding max in normal lists."""
        self.assertEqual(find_max_in_list([1, 5, 3, 9, 2]), 9)
        self.assertEqual(find_max_in_list([-5, -1, -10]), -1)
        self.assertEqual(find_max_in_list([42]), 42)

    def test_find_max_in_list_errors(self):
        """Test max finding error cases."""
        with self.assertRaises(ValueError):
            find_max_in_list([])

    def test_reverse_string(self):
        """Test string reversal."""
        test_cases = [
            ("hello", "olleh"),
            ("Python", "nohtyP"),
            ("", ""),
            ("a", "a"),
            ("radar", "radar")  # palindrome
        ]

        for input_str, expected in test_cases:
            with self.subTest(input_str=input_str):
                self.assertEqual(reverse_string(input_str), expected)

    def test_count_vowels(self):
        """Test vowel counting."""
        test_cases = [
            ("hello", 2),      # e, o
            ("Python", 1),     # o
            ("rhythm", 0),     # no vowels
            ("", 0),           # empty string
            ("AEIOU", 5),      # all vowels uppercase
            ("aeiou", 5),      # all vowels lowercase
            ("Hello World", 3) # e, o, o
        ]

        for text, expected in test_cases:
            with self.subTest(text=text):
                self.assertEqual(count_vowels(text), expected)
```

### Bank Account Tests Solution

```python
import unittest
from bank_account import BankAccount

class TestBankAccount(unittest.TestCase):

    def setUp(self):
        """Set up fresh account for each test."""
        self.account = BankAccount("12345", "John Doe", 1000)

    def test_initialization(self):
        """Test account initialization."""
        self.assertEqual(self.account.account_number, "12345")
        self.assertEqual(self.account.owner_name, "John Doe")
        self.assertEqual(self.account.balance, 1000)
        self.assertEqual(self.account.transactions, [])

    def test_deposit_normal(self):
        """Test normal deposit operations."""
        result = self.account.deposit(500)
        self.assertEqual(result, 1500)
        self.assertEqual(self.account.balance, 1500)
        self.assertIn("Deposit: +$500.00", self.account.transactions)

    def test_deposit_invalid_amounts(self):
        """Test deposit with invalid amounts."""
        with self.assertRaises(ValueError):
            self.account.deposit(0)
        with self.assertRaises(ValueError):
            self.account.deposit(-100)

        # Balance should not change
        self.assertEqual(self.account.balance, 1000)

    def test_withdraw_normal(self):
        """Test normal withdrawal operations."""
        result = self.account.withdraw(300)
        self.assertEqual(result, 700)
        self.assertEqual(self.account.balance, 700)
        self.assertIn("Withdrawal: -$300.00", self.account.transactions)

    def test_withdraw_insufficient_funds(self):
        """Test withdrawal with insufficient funds."""
        with self.assertRaises(ValueError):
            self.account.withdraw(1500)

        # Balance should not change
        self.assertEqual(self.account.balance, 1000)

    def test_withdraw_invalid_amounts(self):
        """Test withdrawal with invalid amounts."""
        with self.assertRaises(ValueError):
            self.account.withdraw(0)
        with self.assertRaises(ValueError):
            self.account.withdraw(-50)

        # Balance should not change
        self.assertEqual(self.account.balance, 1000)

    def test_get_balance(self):
        """Test balance retrieval."""
        self.assertEqual(self.account.get_balance(), 1000)

        self.account.deposit(200)
        self.assertEqual(self.account.get_balance(), 1200)

    def test_get_transaction_history(self):
        """Test transaction history."""
        # Initially empty
        self.assertEqual(self.account.get_transaction_history(), [])

        # After operations
        self.account.deposit(100)
        self.account.withdraw(50)

        history = self.account.get_transaction_history()
        self.assertEqual(len(history), 2)
        self.assertIn("Deposit: +$100.00", history)
        self.assertIn("Withdrawal: -$50.00", history)

        # Should return copy, not reference
        history_copy = self.account.get_transaction_history()
        history_copy.append("Fake transaction")
        self.assertEqual(len(self.account.transactions), 2)  # Original unchanged

    def test_transfer_successful(self):
        """Test successful transfers between accounts."""
        other_account = BankAccount("67890", "Jane Smith", 500)

        result = self.account.transfer(other_account, 200)
        self.assertTrue(result)

        # Check balances
        self.assertEqual(self.account.balance, 800)
        self.assertEqual(other_account.balance, 700)

        # Check transactions
        self.assertIn("Withdrawal: -$200.00", self.account.transactions)
        self.assertIn("Deposit: +$200.00", other_account.transactions)

    def test_transfer_insufficient_funds(self):
        """Test transfer with insufficient funds."""
        other_account = BankAccount("67890", "Jane Smith", 500)

        with self.assertRaises(ValueError):
            self.account.transfer(other_account, 1500)

        # Balances should not change
        self.assertEqual(self.account.balance, 1000)
        self.assertEqual(other_account.balance, 500)

    def test_transfer_invalid_account(self):
        """Test transfer to invalid account type."""
        with self.assertRaises(ValueError):
            self.account.transfer("not_an_account", 100)
```

### Data Processor Tests Solution

```python
import unittest
from data_processor import (
    safe_divide,
    parse_number,
    find_element_in_list,
    merge_dictionaries,
    validate_email
)

class TestDataProcessor(unittest.TestCase):

    def test_safe_divide_normal(self):
        """Test normal division cases."""
        self.assertEqual(safe_divide(10, 2), 5.0)
        self.assertEqual(safe_divide(15, 3), 5.0)
        self.assertEqual(safe_divide(7, 2), 3.5)

    def test_safe_divide_by_zero(self):
        """Test division by zero handling."""
        self.assertEqual(safe_divide(10, 0), float('inf'))
        self.assertEqual(safe_divide(-10, 0), float('-inf'))
        self.assertEqual(safe_divide(0, 0), float('nan'))  # Special case

    def test_safe_divide_type_error(self):
        """Test type error handling."""
        with self.assertRaises(ValueError):
            safe_divide("10", 2)
        with self.assertRaises(ValueError):
            safe_divide(10, "2")

    def test_parse_number_valid(self):
        """Test parsing valid numbers."""
        self.assertEqual(parse_number("42"), 42)
        self.assertEqual(parse_number("3.14"), 3.14)
        self.assertEqual(parse_number("0"), 0)
        self.assertEqual(parse_number("-5"), -5)

    def test_parse_number_invalid(self):
        """Test parsing invalid number strings."""
        with self.assertRaises(ValueError):
            parse_number("abc")
        with self.assertRaises(ValueError):
            parse_number("12.34.56")
        with self.assertRaises(ValueError):
            parse_number("")

    def test_parse_number_type_error(self):
        """Test parse_number with wrong input types."""
        with self.assertRaises(TypeError):
            parse_number(123)
        with self.assertRaises(TypeError):
            parse_number(None)

    def test_find_element_in_list(self):
        """Test finding elements in lists."""
        test_list = ["apple", "banana", "cherry"]

        self.assertEqual(find_element_in_list(test_list, "banana"), 1)
        self.assertEqual(find_element_in_list(test_list, "apple"), 0)
        self.assertEqual(find_element_in_list(test_list, "grape"), -1)

    def test_find_element_errors(self):
        """Test error handling in find_element_in_list."""
        with self.assertRaises(TypeError):
            find_element_in_list("not_a_list", "item")

        with self.assertRaises(TypeError):
            find_element_in_list(123, "item")

    def test_merge_dictionaries(self):
        """Test dictionary merging."""
        dict1 = {"a": 1, "b": 2}
        dict2 = {"b": 20, "c": 30}

        result = merge_dictionaries(dict1, dict2)
        expected = {"a": 1, "b": 20, "c": 30}
        self.assertEqual(result, expected)

    def test_merge_dictionaries_errors(self):
        """Test merge_dictionaries error handling."""
        with self.assertRaises(TypeError):
            merge_dictionaries("not_dict", {"a": 1})

        with self.assertRaises(TypeError):
            merge_dictionaries({"a": 1}, "not_dict")

    def test_validate_email_valid(self):
        """Test valid email validation."""
        valid_emails = [
            "user@example.com",
            "test.email@domain.co.uk",
            "user+tag@example.com"
        ]

        for email in valid_emails:
            with self.subTest(email=email):
                self.assertTrue(validate_email(email))

    def test_validate_email_invalid(self):
        """Test invalid email validation."""
        invalid_emails = [
            "",
            "no-at-sign",
            "@domain.com",
            "user@",
            "user@domain",
            "user.domain.com"
        ]

        for email in invalid_emails:
            with self.subTest(email=email):
                self.assertFalse(validate_email(email))

    def test_validate_email_type_error(self):
        """Test validate_email with wrong types."""
        with self.assertRaises(TypeError):
            validate_email(123)

        with self.assertRaises(TypeError):
            validate_email(None)
```

---

## 📝 Key Takeaways

1. **AAA Pattern**: Arrange test data, Act by calling functions, Assert expected results
2. **Test Isolation**: Each test should be independent with setUp/tearDown
3. **Edge Cases**: Test boundaries, empty inputs, and error conditions
4. **Descriptive Names**: Use clear test method names that explain what is tested
5. **Assertions**: Choose appropriate assertion methods for different verification needs
6. **Error Testing**: Use assertRaises to test exception handling
7. **Test Coverage**: Aim for comprehensive coverage of all code paths

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
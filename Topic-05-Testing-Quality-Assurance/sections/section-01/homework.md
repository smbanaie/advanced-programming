# Homework 1: Achieving 80% Code Coverage

**Topic**: 5 - Testing & Quality Assurance
**Section**: 1 of 3 sections (90 min workshop + homework)
**Level**: Intermediate to Advanced
**Prerequisites**: Section 1 tutorial and workshop

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Write Comprehensive Tests**: Create thorough test suites using unittest
2. **Apply Testing Best Practices**: Follow AAA pattern and testing principles
3. **Measure Code Coverage**: Use coverage tools to quantify test effectiveness
4. **Identify Untested Code**: Find and test code paths that lack coverage
5. **Improve Test Quality**: Write meaningful tests that add value
6. **Debug and Refactor**: Use tests to identify and fix code issues

---

## 📋 Assignment Overview

You will write comprehensive unit tests for a provided code module, achieving at least 80% code coverage. The module contains various functions and classes that need thorough testing. You'll use coverage tools to measure your progress and ensure all code paths are tested.

### Project Structure

```
homework-testing-coverage/
├── inventory_system.py     # Code to test (provided)
├── test_inventory.py       # Your test file (create this)
├── coverage_report/        # Coverage reports (generated)
├── pyproject.toml          # Project configuration
└── README.md              # Documentation
```

### Provided Code (inventory_system.py)

```python
"""Inventory management system for a retail store."""

from typing import Dict, List, Optional, Tuple
from datetime import datetime, timedelta
import json


class Product:
    """Represents a product in the inventory."""

    def __init__(self, product_id: str, name: str, price: float, category: str):
        """Initialize a product."""
        if not product_id or not isinstance(product_id, str):
            raise ValueError("Product ID must be a non-empty string")
        if not name or not isinstance(name, str):
            raise ValueError("Product name must be a non-empty string")
        if price < 0:
            raise ValueError("Product price cannot be negative")
        if not category or not isinstance(category, str):
            raise ValueError("Product category must be a non-empty string")

        self.product_id = product_id
        self.name = name
        self.price = price
        self.category = category
        self.stock_quantity = 0
        self.created_date = datetime.now()

    def update_stock(self, quantity: int) -> bool:
        """Update the stock quantity."""
        if quantity < 0:
            raise ValueError("Stock quantity cannot be negative")

        self.stock_quantity = quantity
        return True

    def is_in_stock(self) -> bool:
        """Check if product is in stock."""
        return self.stock_quantity > 0

    def get_value(self) -> float:
        """Calculate total value of stock."""
        return self.price * self.stock_quantity

    def to_dict(self) -> Dict:
        """Convert product to dictionary."""
        return {
            'product_id': self.product_id,
            'name': self.name,
            'price': self.price,
            'category': self.category,
            'stock_quantity': self.stock_quantity,
            'created_date': self.created_date.isoformat()
        }

    @classmethod
    def from_dict(cls, data: Dict) -> 'Product':
        """Create product from dictionary."""
        product = cls(
            data['product_id'],
            data['name'],
            data['price'],
            data['category']
        )
        product.stock_quantity = data.get('stock_quantity', 0)
        if 'created_date' in data:
            product.created_date = datetime.fromisoformat(data['created_date'])
        return product


class InventoryManager:
    """Manages the store inventory."""

    def __init__(self):
        """Initialize inventory manager."""
        self.products: Dict[str, Product] = {}
        self.categories: Dict[str, List[str]] = {}

    def add_product(self, product: Product) -> bool:
        """Add a product to inventory."""
        if not isinstance(product, Product):
            raise TypeError("Must provide a Product instance")

        if product.product_id in self.products:
            raise ValueError(f"Product {product.product_id} already exists")

        self.products[product.product_id] = product

        # Update category index
        if product.category not in self.categories:
            self.categories[product.category] = []
        self.categories[product.category].append(product.product_id)

        return True

    def remove_product(self, product_id: str) -> bool:
        """Remove a product from inventory."""
        if product_id not in self.products:
            return False

        product = self.products[product_id]
        del self.products[product_id]

        # Update category index
        if product.category in self.categories:
            self.categories[product.category].remove(product_id)
            if not self.categories[product.category]:
                del self.categories[product.category]

        return True

    def get_product(self, product_id: str) -> Optional[Product]:
        """Get a product by ID."""
        return self.products.get(product_id)

    def get_products_by_category(self, category: str) -> List[Product]:
        """Get all products in a category."""
        if category not in self.categories:
            return []

        return [self.products[pid] for pid in self.categories[category]]

    def get_total_inventory_value(self) -> float:
        """Calculate total value of all inventory."""
        return sum(product.get_value() for product in self.products.values())

    def get_low_stock_products(self, threshold: int = 10) -> List[Product]:
        """Get products with stock below threshold."""
        return [product for product in self.products.values()
                if product.stock_quantity < threshold]

    def update_product_stock(self, product_id: str, new_quantity: int) -> bool:
        """Update stock for a specific product."""
        product = self.get_product(product_id)
        if not product:
            return False

        product.update_stock(new_quantity)
        return True

    def search_products(self, query: str) -> List[Product]:
        """Search products by name or ID."""
        if not query:
            return []

        query_lower = query.lower()
        results = []

        for product in self.products.values():
            if (query_lower in product.product_id.lower() or
                query_lower in product.name.lower()):
                results.append(product)

        return results

    def get_inventory_report(self) -> Dict:
        """Generate comprehensive inventory report."""
        total_products = len(self.products)
        total_value = self.get_total_inventory_value()
        low_stock = self.get_low_stock_products()

        category_counts = {}
        for category, product_ids in self.categories.items():
            category_counts[category] = len(product_ids)

        return {
            'total_products': total_products,
            'total_value': total_value,
            'categories': category_counts,
            'low_stock_count': len(low_stock),
            'low_stock_products': [p.product_id for p in low_stock]
        }

    def save_inventory(self, filename: str) -> bool:
        """Save inventory to JSON file."""
        try:
            data = {
                'products': {pid: product.to_dict()
                           for pid, product in self.products.items()},
                'exported_at': datetime.now().isoformat()
            }

            with open(filename, 'w', encoding='utf-8') as f:
                json.dump(data, f, indent=2, ensure_ascii=False)
            return True
        except Exception:
            return False

    def load_inventory(self, filename: str) -> bool:
        """Load inventory from JSON file."""
        try:
            with open(filename, 'r', encoding='utf-8') as f:
                data = json.load(f)

            self.products.clear()
            self.categories.clear()

            for pid, product_data in data['products'].items():
                product = Product.from_dict(product_data)
                self.add_product(product)

            return True
        except Exception:
            return False


# Utility functions
def calculate_discount(price: float, discount_percent: float) -> float:
    """Calculate discounted price."""
    if price < 0:
        raise ValueError("Price cannot be negative")
    if not 0 <= discount_percent <= 100:
        raise ValueError("Discount percent must be between 0 and 100")

    return price * (1 - discount_percent / 100)


def format_currency(amount: float, currency: str = "$") -> str:
    """Format amount as currency string."""
    return f"{currency}{amount:.2f}"


def validate_product_id(product_id: str) -> bool:
    """Validate product ID format."""
    if not product_id or not isinstance(product_id, str):
        return False

    # Must be alphanumeric with optional hyphens
    import re
    return bool(re.match(r'^[A-Za-z0-9-]+$', product_id))
```

---

## 🛠️ Implementation Requirements

### Test File Structure (test_inventory.py)

Create comprehensive tests following the structure:

```python
import unittest
from datetime import datetime
from inventory_system import (
    Product,
    InventoryManager,
    calculate_discount,
    format_currency,
    validate_product_id
)

class TestProduct(unittest.TestCase):
    """Test cases for Product class."""

    def setUp(self):
        """Set up test fixtures."""
        self.product = Product("P001", "Laptop", 999.99, "Electronics")

    # TODO: Test Product initialization
    # - Valid initialization
    # - Invalid product_id (empty, wrong type)
    # - Invalid name (empty, wrong type)
    # - Invalid price (negative)
    # - Invalid category (empty, wrong type)

    # TODO: Test update_stock method
    # - Valid stock updates
    # - Invalid stock values (negative)

    # TODO: Test is_in_stock method
    # - Product with stock > 0
    # - Product with stock = 0

    # TODO: Test get_value method
    # - Product with stock and price
    # - Product with zero stock

    # TODO: Test to_dict method
    # - All fields included
    # - Date serialization

    # TODO: Test from_dict class method
    # - Valid dictionary
    # - Missing optional fields
    # - Invalid data


class TestInventoryManager(unittest.TestCase):
    """Test cases for InventoryManager class."""

    def setUp(self):
        """Set up test fixtures."""
        self.manager = InventoryManager()
        self.sample_product = Product("P001", "Laptop", 999.99, "Electronics")

    # TODO: Test add_product method
    # - Adding valid product
    # - Adding duplicate product (should fail)
    # - Adding invalid object type (should fail)

    # TODO: Test remove_product method
    # - Removing existing product
    # - Removing non-existing product
    # - Category index updates

    # TODO: Test get_product method
    # - Getting existing product
    # - Getting non-existing product

    # TODO: Test get_products_by_category method
    # - Category with products
    # - Empty category
    # - Non-existing category

    # TODO: Test get_total_inventory_value method
    # - Empty inventory
    # - Single product
    # - Multiple products

    # TODO: Test get_low_stock_products method
    # - Products below threshold
    # - Products at or above threshold
    # - Different threshold values

    # TODO: Test update_product_stock method
    # - Updating existing product
    # - Updating non-existing product

    # TODO: Test search_products method
    # - Search by product ID
    # - Search by product name
    # - Case-insensitive search
    # - Empty query
    # - No matches

    # TODO: Test get_inventory_report method
    # - Empty inventory
    # - Inventory with products
    # - Category distribution
    # - Low stock products

    # TODO: Test save_inventory method
    # - Saving to valid file
    # - Saving with products
    # - File I/O errors

    # TODO: Test load_inventory method
    # - Loading valid file
    # - Loading non-existing file
    # - Loading corrupted file


class TestUtilityFunctions(unittest.TestCase):
    """Test cases for utility functions."""

    # TODO: Test calculate_discount function
    # - Valid discounts
    # - Edge cases (0% and 100% discount)
    # - Invalid inputs (negative price, invalid discount percent)

    # TODO: Test format_currency function
    # - Positive amounts
    # - Zero amount
    # - Different currencies

    # TODO: Test validate_product_id function
    # - Valid IDs (alphanumeric with hyphens)
    # - Invalid IDs (empty, special characters, wrong type)


if __name__ == '__main__':
    unittest.main()
```

---

## 📊 Coverage Requirements

### Achieving 80% Coverage

You must achieve at least 80% code coverage. Use the following tools:

```bash
# Install coverage
pip install coverage

# Run tests with coverage
coverage run -m unittest test_inventory.py

# Generate coverage report
coverage report

# Generate HTML coverage report
coverage html

# View detailed report
coverage report -m
```

### Coverage Targets

**Minimum Requirements:**
- **Overall Coverage**: ≥ 80%
- **Product Class**: ≥ 90%
- **InventoryManager Class**: ≥ 85%
- **Utility Functions**: ≥ 95%

### Identifying Uncovered Code

Use coverage reports to find untested code:

```bash
# Run coverage and generate HTML report
coverage run -m unittest test_inventory.py
coverage html

# Open htmlcov/index.html to see line-by-line coverage
```

**Common uncovered areas:**
- Error handling paths (exception branches)
- Edge cases in conditional statements
- Default parameter values
- Private methods (if any)

---

## 🧪 Testing Best Practices

### Test Organization

```python
class TestInventoryManager(unittest.TestCase):

    def setUp(self):
        """Create fresh inventory manager for each test."""
        self.manager = InventoryManager()
        self.product1 = Product("P001", "Laptop", 999.99, "Electronics")
        self.product2 = Product("P002", "Mouse", 29.99, "Electronics")

    def test_add_single_product(self):
        """Test adding a single product."""
        # Arrange
        result = self.manager.add_product(self.product1)

        # Act & Assert
        self.assertTrue(result)
        self.assertIn("P001", self.manager.products)
        self.assertEqual(len(self.manager.products), 1)

    def test_add_duplicate_product_fails(self):
        """Test that adding duplicate product raises error."""
        # Arrange
        self.manager.add_product(self.product1)

        # Act & Assert
        with self.assertRaises(ValueError):
            self.manager.add_product(self.product1)
```

### Testing Error Conditions

```python
def test_add_product_invalid_type(self):
    """Test adding invalid object type."""
    with self.assertRaises(TypeError):
        self.manager.add_product("not_a_product")

def test_update_stock_negative_value(self):
    """Test updating stock with negative value."""
    self.manager.add_product(self.product1)

    with self.assertRaises(ValueError):
        self.manager.update_product_stock("P001", -5)
```

### Testing Complex Logic

```python
def test_get_inventory_report_comprehensive(self):
    """Test comprehensive inventory report."""
    # Arrange
    self.manager.add_product(self.product1)  # Electronics, $999.99
    self.manager.add_product(self.product2)  # Electronics, $29.99

    # Set stock levels
    self.manager.update_product_stock("P001", 5)  # Value: 4999.95
    self.manager.update_product_stock("P002", 2)  # Value: 59.98

    # Act
    report = self.manager.get_inventory_report()

    # Assert
    self.assertEqual(report['total_products'], 2)
    self.assertAlmostEqual(report['total_value'], 5059.93, places=2)
    self.assertEqual(report['categories']['Electronics'], 2)
    self.assertEqual(report['low_stock_count'], 0)  # Both have stock
```

---

## 📋 Submission Requirements

### Required Files

1. **test_inventory.py**: Complete test suite
2. **Coverage Report**: Screenshot or text output showing ≥80% coverage
3. **README.md**: Documentation explaining your testing approach

### Test Quality Standards

- [ ] **Descriptive Names**: All test methods have clear, descriptive names
- [ ] **AAA Pattern**: Each test follows Arrange-Act-Assert structure
- [ ] **Independent Tests**: No test depends on others running first
- [ ] **Edge Cases**: Tests cover boundary conditions and error cases
- [ ] **Comprehensive Coverage**: All code paths are tested
- [ ] **Proper Assertions**: Uses appropriate assertion methods

### Coverage Evidence

Provide evidence of coverage achievement:

```bash
# Generate coverage report
coverage run -m unittest test_inventory.py
coverage report --include="inventory_system.py" > coverage_report.txt

# Include coverage_report.txt in submission
```

### Documentation Requirements

**README.md should include:**
- Test coverage achieved (percentage)
- Testing approach and strategy
- Any challenges faced and solutions
- Key learnings from the testing process

---

## 🎯 Evaluation Criteria

### Testing Completeness (40%)
- [ ] All classes and functions have corresponding tests
- [ ] Edge cases and error conditions are covered
- [ ] Different code paths are exercised
- [ ] Boundary conditions are tested

### Code Coverage (30%)
- [ ] Overall coverage ≥ 80%
- [ ] Product class coverage ≥ 90%
- [ ] InventoryManager coverage ≥ 85%
- [ ] Utility functions coverage ≥ 95%
- [ ] Coverage report provided

### Test Quality (20%)
- [ ] Clear, descriptive test names
- [ ] Proper AAA structure
- [ ] Appropriate assertion methods
- [ ] Test isolation (no dependencies)
- [ ] Error handling tests

### Documentation (10%)
- [ ] Clear README with testing approach
- [ ] Coverage evidence provided
- [ ] Test organization explained
- [ ] Challenges and solutions documented

---

## 🚀 Bonus Challenges

### Advanced Testing Techniques

1. **Parameterized Tests**: Use `unittest.subTest()` or external libraries to run the same test with different inputs

2. **Mocking External Dependencies**: If the inventory system used external APIs or databases, create tests that mock these dependencies

3. **Performance Testing**: Add tests that verify the performance characteristics of inventory operations

4. **Integration Testing**: Create tests that verify the interaction between Product and InventoryManager classes

5. **Property-Based Testing**: Use libraries like `hypothesis` to generate test cases automatically

### Code Quality Improvements

1. **Test Refactoring**: Identify and eliminate test code duplication
2. **Custom Test Base Classes**: Create base test classes with common setup/teardown logic
3. **Test Data Builders**: Create helper functions to build complex test data
4. **Test Organization**: Group related tests using test suites

---

## 📞 Getting Help

### Coverage Issues
- **Low Coverage**: Check which lines are uncovered in the HTML report
- **Error Paths**: Add tests that trigger exception conditions
- **Conditional Branches**: Ensure both true and false branches are tested

### Testing Problems
- **Test Failures**: Use print statements or debugger to understand failures
- **Import Errors**: Ensure test file can import the code being tested
- **Setup Issues**: Verify test environment and dependencies

### Resources
- [Coverage.py Documentation](https://coverage.readthedocs.io/)
- [unittest Documentation](https://docs.python.org/3/library/unittest.html)
- [Testing Best Practices](https://realpython.com/python-testing/)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **Unit Testing**: Writing comprehensive tests with unittest
- **Code Coverage**: Measuring and improving test effectiveness
- **Test Design**: Creating meaningful tests that add value
- **Debugging**: Using tests to identify and fix code issues
- **Quality Assurance**: Applying professional testing practices
- **Test Maintenance**: Understanding the importance of maintainable tests

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 6-8 hours
**Total Points**: 100
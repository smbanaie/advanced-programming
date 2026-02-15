# Homework 1: Data Manipulation Functions

**Section**: 6 - Working with Lists and Sets  
**Level**: Intermediate  
**Prerequisites**: Tutorial 1, Workshop 1  
**Estimated Time**: 4-6 hours  
**Due Date**: [Insert due date]

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Design Modular Functions**: Create well-structured, reusable data processing functions
2. **Apply Advanced List Operations**: Use list comprehensions, filtering, and transformation techniques
3. **Implement Set-Based Algorithms**: Leverage set operations for efficient data processing
4. **Handle Real-World Data**: Process practical datasets with edge cases and error conditions
5. **Write Production-Ready Code**: Include proper documentation, error handling, and testing

---

## 📋 Assignment Overview

You will implement a comprehensive data manipulation library with functions for processing various types of data. The library should handle lists, sets, and mixed data structures efficiently.

### Project Structure

```
data_manipulation/
├── data_utils.py          # Main library implementation
├── test_data_utils.py     # Comprehensive test suite
├── examples.py           # Usage examples
└── README.md             # Documentation
```

---

## 🔧 Required Functions

### Core List Operations

#### 1. Advanced Filtering Function

```python
def advanced_filter(data, conditions):
    """
    Filter data based on multiple conditions with logical operators.

    Args:
        data: List of dictionaries or objects to filter
        conditions: Dictionary specifying filter conditions
            Format: {"field": {"operator": "value"}}
            Supported operators: "eq", "ne", "gt", "gte", "lt", "lte", "in", "contains"

    Returns:
        List of items matching all conditions

    Examples:
        data = [
            {"name": "Alice", "age": 25, "city": "NYC"},
            {"name": "Bob", "age": 30, "city": "LA"}
        ]
        conditions = {"age": {"gte": 26}, "city": {"eq": "LA"}}
        result = advanced_filter(data, conditions)  # Returns Bob's record
    """
    pass
```

#### 2. Data Transformation Pipeline

```python
def transform_pipeline(data, transformations):
    """
    Apply a series of transformations to data.

    Args:
        data: List of items to transform
        transformations: List of transformation functions

    Returns:
        Transformed data after applying all transformations

    Examples:
        def add_prefix(s): return f"prefix_{s}"
        def to_upper(s): return s.upper()
        def add_length(s): return f"{s}_{len(s)}"

        data = ["hello", "world"]
        pipeline = [add_prefix, to_upper, add_length]
        result = transform_pipeline(data, pipeline)
        # Result: ["PREFIX_HELLO_13", "PREFIX_WORLD_13"]
    """
    pass
```

#### 3. Nested Data Flattener

```python
def flatten_nested_data(data, separator="_"):
    """
    Flatten nested dictionaries/lists into flat key-value pairs.

    Args:
        data: Nested data structure (dict/list)
        separator: String to use for joining nested keys

    Returns:
        Dictionary with flattened key-value pairs

    Examples:
        nested = {
            "user": {
                "name": "Alice",
                "address": {
                    "street": "123 Main St",
                    "city": "NYC"
                }
            },
            "scores": [85, 92, 88]
        }
        result = flatten_nested_data(nested)
        # Result: {
        #     "user_name": "Alice",
        #     "user_address_street": "123 Main St",
        #     "user_address_city": "NYC",
        #     "scores_0": 85,
        #     "scores_1": 92,
        #     "scores_2": 88
        # }
    """
    pass
```

### Advanced Set Operations

#### 4. Multi-Set Operations

```python
def multi_set_operations(*sets, operation="union"):
    """
    Perform set operations on multiple sets with custom logic.

    Args:
        *sets: Variable number of sets
        operation: Type of operation ("union", "intersection", "difference", "symmetric_diff")

    Returns:
        Result of the set operation

    Examples:
        set1 = {1, 2, 3}
        set2 = {2, 3, 4}
        set3 = {3, 4, 5}

        union_result = multi_set_operations(set1, set2, set3, operation="union")
        # {1, 2, 3, 4, 5}

        intersection_result = multi_set_operations(set1, set2, set3, operation="intersection")
        # {3}
    """
    pass
```

#### 5. Set Partitioning

```python
def partition_by_predicates(data, predicates):
    """
    Partition data into multiple groups based on predicates.

    Args:
        data: List of items to partition
        predicates: List of (name, predicate_function) tuples

    Returns:
        Dictionary mapping partition names to lists of items

    Examples:
        numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

        predicates = [
            ("even", lambda x: x % 2 == 0),
            ("prime", lambda x: x in [2, 3, 5, 7]),
            ("large", lambda x: x > 5)
        ]

        result = partition_by_predicates(numbers, predicates)
        # {
        #     "even": [2, 4, 6, 8, 10],
        #     "prime": [2, 3, 5, 7],
        #     "large": [6, 7, 8, 9, 10]
        # }
    """
    pass
```

### Data Analysis Functions

#### 6. Statistical Outlier Detection

```python
def detect_statistical_outliers(data, method="iqr", threshold=1.5):
    """
    Detect outliers in numerical data using statistical methods.

    Args:
        data: List of numerical values
        method: Outlier detection method ("iqr", "zscore", "modified_zscore")
        threshold: Sensitivity threshold for outlier detection

    Returns:
        Tuple of (normal_data, outliers, statistics)

    Examples:
        data = [1, 2, 3, 4, 5, 100]
        normal, outliers, stats = detect_statistical_outliers(data, method="iqr")
        print(f"Outliers: {outliers}")  # [100]
    """
    pass
```

#### 7. Text Analysis Suite

```python
def analyze_text_corpus(texts):
    """
    Perform comprehensive text analysis on a corpus.

    Args:
        texts: List of text strings

    Returns:
        Dictionary with comprehensive text analysis

    Analysis should include:
    - Word frequency distribution
    - Average sentence length
    - Most common phrases (2-3 words)
    - Readability metrics
    - Language detection (basic)
    - Sentiment indicators (word-based)

    Examples:
        corpus = [
            "Python is great for data analysis.",
            "Machine learning with Python is powerful.",
            "Data science requires statistical knowledge."
        ]

        analysis = analyze_text_corpus(corpus)
        print(f"Most common words: {analysis['top_words']}")
        print(f"Average sentence length: {analysis['avg_sentence_length']}")
    """
    pass
```

---

## 🧪 Testing Requirements

### Comprehensive Test Suite

Create `test_data_utils.py` with tests for all functions:

```python
import pytest
from data_utils import (
    advanced_filter,
    transform_pipeline,
    flatten_nested_data,
    multi_set_operations,
    partition_by_predicates,
    detect_statistical_outliers,
    analyze_text_corpus
)

class TestDataUtils:
    """Comprehensive test suite for data manipulation functions."""

    def test_advanced_filter_basic(self):
        """Test basic filtering functionality."""
        pass

    def test_advanced_filter_complex(self):
        """Test complex filtering with multiple conditions."""
        pass

    def test_transform_pipeline(self):
        """Test data transformation pipeline."""
        pass

    def test_flatten_nested_data(self):
        """Test nested data flattening."""
        pass

    def test_multi_set_operations(self):
        """Test various set operations."""
        pass

    def test_partition_by_predicates(self):
        """Test data partitioning."""
        pass

    def test_detect_statistical_outliers(self):
        """Test outlier detection methods."""
        pass

    def test_analyze_text_corpus(self):
        """Test text analysis functionality."""
        pass

    # Add edge case tests
    def test_empty_inputs(self):
        """Test behavior with empty inputs."""
        pass

    def test_error_conditions(self):
        """Test error handling."""
        pass
```

### Test Coverage Requirements

- **Minimum Coverage**: 85%
- **Edge Cases**: Empty lists, None values, invalid inputs
- **Error Handling**: Appropriate exceptions for invalid inputs
- **Performance**: Tests should complete within reasonable time

---

## 📊 Real-World Application

### Data Processing Pipeline

Create a complete data processing example that demonstrates your library:

```python
# examples.py
from data_utils import *

def process_sales_data():
    """Example: Process sales data using the library."""

    # Sample sales data
    sales_data = [
        {"product": "Widget A", "sales": 150, "region": "North", "quarter": "Q1"},
        {"product": "Widget B", "sales": 200, "region": "South", "quarter": "Q1"},
        {"product": "Widget A", "sales": 180, "region": "North", "quarter": "Q2"},
        # ... more data
    ]

    # 1. Filter high-performing products
    high_performers = advanced_filter(
        sales_data,
        {"sales": {"gte": 175}}
    )

    # 2. Transform data for analysis
    def calculate_growth(item):
        # Add growth calculation logic
        return item

    transformed_data = transform_pipeline(
        high_performers,
        [calculate_growth]
    )

    # 3. Partition by region
    regional_data = partition_by_predicates(
        transformed_data,
        [
            ("north", lambda x: x["region"] == "North"),
            ("south", lambda x: x["region"] == "South"),
        ]
    )

    # 4. Detect outliers in sales
    all_sales = [item["sales"] for item in sales_data]
    normal_sales, outliers, stats = detect_statistical_outliers(all_sales)

    return {
        "high_performers": high_performers,
        "regional_breakdown": regional_data,
        "sales_stats": stats,
        "outliers": outliers
    }
```

---

## 📋 Submission Requirements

### Code Quality Standards

1. **Documentation**: All functions must have comprehensive docstrings
2. **Type Hints**: Use type hints for function parameters and return values
3. **Error Handling**: Proper exception handling with meaningful messages
4. **Code Style**: Follow PEP 8 standards
5. **Modularity**: Functions should be focused and reusable

### File Structure

```
submission/
├── data_utils.py          # Main implementation (required)
├── test_data_utils.py     # Test suite (required)
├── examples.py           # Usage examples (required)
├── README.md             # Documentation (required)
└── requirements.txt      # Dependencies (if any)
```

### README.md Requirements

Your README should include:

- Project description and objectives
- Installation and setup instructions
- API documentation for all functions
- Usage examples
- Test instructions
- Performance considerations

---

## 🎯 Evaluation Criteria

### Functionality (40%)
- All required functions implemented correctly
- Functions handle edge cases appropriately
- Error handling is robust
- Real-world examples work as expected

### Code Quality (30%)
- Clean, readable, well-documented code
- Proper use of Python idioms and best practices
- Appropriate data structures and algorithms
- Type hints and docstrings

### Testing (20%)
- Comprehensive test coverage (85% minimum)
- Edge cases and error conditions covered
- Tests are meaningful and well-written
- Test suite runs without errors

### Documentation (10%)
- Clear README with setup instructions
- Function documentation is comprehensive
- Usage examples are helpful
- Code is well-commented

---

## 🚀 Bonus Challenges

### Advanced Features (Optional)

1. **Performance Optimization**: Implement caching for expensive operations
2. **Concurrent Processing**: Add parallel processing for large datasets
3. **Data Validation**: Add schema validation for input data
4. **Export Functionality**: Add support for exporting results to CSV/JSON
5. **Visualization**: Create simple data visualization functions

### Real-World Dataset Processing

Process a real dataset (e.g., CSV file) using your library functions. Choose from:
- Sales/transaction data
- User behavior logs
- Scientific measurements
- Social media data

---

## 📞 Getting Help

- Review workshop materials for implementation guidance
- Check Python documentation for built-in functions
- Use debugging tools to test your functions
- Ask questions during office hours

---

## 📚 Resources

- [Python Data Structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Statistics Module](https://docs.python.org/3/library/statistics.html)
- [Collections Module](https://docs.python.org/3/library/collections.html)
- [Itertools Module](https://docs.python.org/3/library/itertools.html)

---

## ⏰ Timeline

- **Week 1**: Implement core functions and basic tests
- **Week 2**: Add advanced features and comprehensive testing
- **Week 3**: Create examples and finalize documentation
- **Week 4**: Code review and submission

---

**Homework Version**: 1.0  
**Last Updated**: February 2026  
**Total Points**: 100
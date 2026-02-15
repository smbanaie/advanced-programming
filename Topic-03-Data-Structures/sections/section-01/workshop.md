# Workshop 1: Building Data Processing Utilities

**Section**: 6 - Working with Lists and Sets (90 min)  
**Level**: Beginner to Intermediate  
**Prerequisites**: Tutorial 1 (Lists and Sets)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Build Practical Utilities**: Create functions for common data processing tasks
2. **Apply List Operations**: Use add, remove, filter, and sort operations
3. **Implement Set Operations**: Work with union, intersection, and difference
4. **Handle Real Data**: Process sample datasets with your utilities
5. **Write Clean Code**: Follow Python best practices and documentation standards

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: List Manipulation Utilities](#exercise-1-list-manipulation-utilities)
3. [Exercise 2: Set-Based Data Processing](#exercise-2-set-based-data-processing)
4. [Exercise 3: Data Analysis Functions](#exercise-3-data-analysis-functions)
5. [Exercise 4: Real-World Data Processing](#exercise-4-real-world-data-processing)
6. [Challenge Exercises](#challenge-exercises)
7. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Project Structure

```bash
# Create workshop directory
mkdir workshop-data-utilities
cd workshop-data-utilities

# Create Python file for utilities
touch data_utils.py

# Create test file
touch test_utils.py
```

### Import Required Modules

```python
# data_utils.py
"""
Data Processing Utilities Workshop
A collection of functions for list and set operations
"""

# No external imports needed for this workshop
# All functionality uses built-in Python data structures
```

---

## 🏃 Exercise 1: List Manipulation Utilities

**Goal**: Create functions that perform common list operations

### Task 1.1: Remove Duplicates Function

Create a function that removes duplicates from a list while preserving order.

```python
def remove_duplicates_preserve_order(items):
    """
    Remove duplicates from a list while preserving the original order.

    Args:
        items: List that may contain duplicates

    Returns:
        List with duplicates removed, order preserved

    Examples:
        >>> remove_duplicates_preserve_order([1, 2, 2, 3, 1, 4])
        [1, 2, 3, 4]
        >>> remove_duplicates_preserve_order(["a", "b", "a", "c"])
        ["a", "b", "c"]
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
test_list = [1, 2, 2, 3, 1, 4, 5, 5]
result = remove_duplicates_preserve_order(test_list)
print(f"Original: {test_list}")
print(f"Result: {result}")
# Expected: [1, 2, 3, 4, 5]
```

### Task 1.2: Filter by Condition

Create a function that filters a list based on a custom condition.

```python
def filter_by_condition(items, condition_func):
    """
    Filter a list using a custom condition function.

    Args:
        items: List to filter
        condition_func: Function that takes an item and returns True/False

    Returns:
        List of items that satisfy the condition

    Examples:
        >>> numbers = [1, 2, 3, 4, 5, 6]
        >>> filter_by_condition(numbers, lambda x: x % 2 == 0)
        [2, 4, 6]
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter even numbers
evens = filter_by_condition(numbers, lambda x: x % 2 == 0)
print(f"Even numbers: {evens}")

# Filter numbers greater than 5
greater_than_five = filter_by_condition(numbers, lambda x: x > 5)
print(f"Numbers > 5: {greater_than_five}")
```

### Task 1.3: Group by Key Function

Create a function that groups list items by a key function.

```python
def group_by_key(items, key_func):
    """
    Group items in a list by a key function.

    Args:
        items: List of items to group
        key_func: Function that extracts a key from each item

    Returns:
        Dictionary where keys are group names and values are lists of items

    Examples:
        >>> words = ["apple", "banana", "cherry", "date"]
        >>> group_by_key(words, lambda x: len(x))
        {5: ["apple"], 6: ["banana", "cherry"], 4: ["date"]}
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
words = ["apple", "banana", "cherry", "date", "fig", "grape"]

# Group by word length
grouped_by_length = group_by_key(words, lambda x: len(x))
print("Grouped by length:")
for length, word_list in grouped_by_length.items():
    print(f"  {length} letters: {word_list}")

# Group by first letter
grouped_by_first = group_by_key(words, lambda x: x[0])
print("\nGrouped by first letter:")
for letter, word_list in grouped_by_first.items():
    print(f"  {letter}: {word_list}")
```

---

## 🔸 Exercise 2: Set-Based Data Processing

**Goal**: Create functions that leverage set operations for data processing

### Task 2.1: Find Unique Elements Across Lists

Create a function that finds elements unique to each list (symmetric difference).

```python
def find_unique_across_lists(*lists):
    """
    Find elements that appear in exactly one of the input lists.

    Args:
        *lists: Variable number of lists

    Returns:
        Set of elements unique to exactly one list

    Examples:
        >>> find_unique_across_lists([1, 2, 3], [2, 3, 4], [3, 4, 5])
        {1, 5}
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
list1 = [1, 2, 3, 4]
list2 = [3, 4, 5, 6]
list3 = [5, 6, 7, 8]

unique = find_unique_across_lists(list1, list2, list3)
print(f"Elements unique to exactly one list: {sorted(unique)}")
# Expected: [1, 7, 8] (1 is only in list1, 7&8 only in list3)
```

### Task 2.2: Set Intersection with Multiple Lists

Create a function that finds common elements across multiple lists.

```python
def intersection_of_lists(*lists):
    """
    Find elements common to all input lists.

    Args:
        *lists: Variable number of lists

    Returns:
        List of elements present in all lists (preserves order from first list)

    Examples:
        >>> intersection_of_lists([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6])
        [3, 4]
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
list1 = ["apple", "banana", "cherry", "date"]
list2 = ["banana", "cherry", "date", "elderberry"]
list3 = ["cherry", "date", "elderberry", "fig"]

common = intersection_of_lists(list1, list2, list3)
print(f"Common to all lists: {common}")
# Expected: ["cherry", "date"]
```

### Task 2.3: Remove Common Elements

Create a function that removes elements from one list that appear in any other list.

```python
def remove_common_with_others(main_list, *other_lists):
    """
    Remove elements from main_list that appear in any of the other lists.

    Args:
        main_list: List to filter
        *other_lists: Other lists to check against

    Returns:
        List with common elements removed

    Examples:
        >>> remove_common_with_others([1, 2, 3, 4, 5], [2, 4, 6], [3, 6, 9])
        [1, 5]
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
main = [1, 2, 3, 4, 5, 6, 7]
list_a = [2, 4, 6, 8]
list_b = [3, 6, 9, 12]

filtered = remove_common_with_others(main, list_a, list_b)
print(f"Elements not in any other list: {sorted(filtered)}")
# Expected: [1, 5, 7] (2, 3, 4, 6 appear in other lists)
```

---

## 📊 Exercise 3: Data Analysis Functions

**Goal**: Create functions that analyze data using lists and sets

### Task 3.1: Frequency Counter

Create a function that counts the frequency of each element in a list.

```python
def count_frequencies(items):
    """
    Count the frequency of each element in a list.

    Args:
        items: List of elements (must be hashable)

    Returns:
        Dictionary with elements as keys and counts as values

    Examples:
        >>> count_frequencies([1, 2, 2, 3, 3, 3])
        {1: 1, 2: 2, 3: 3}
    """
    # TODO: Implement this function
    pass
```

**Test your function:**
```python
# Test cases
data = ["apple", "banana", "apple", "cherry", "banana", "apple"]
frequencies = count_frequencies(data)
print("Frequency count:")
for item, count in frequencies.items():
    print(f"  {item}: {count}")

# Find most common
most_common = max(frequencies, key=frequencies.get)
print(f"Most common: {most_common} ({frequencies[most_common]} times)")
```

### Task 3.2: Find Outliers

Create a function that identifies outlier values in a numeric list.

```python
def find_outliers(numbers, threshold=2.0):
    """
    Find outliers in a list of numbers using standard deviation.

    Args:
        numbers: List of numeric values
        threshold: Standard deviation multiplier for outlier detection

    Returns:
        Tuple of (normal_values, outliers)

    Examples:
        >>> data = [1, 2, 3, 4, 5, 100]
        >>> normal, outliers = find_outliers(data)
        >>> print(f"Normal: {normal}, Outliers: {outliers}")
        Normal: [1, 2, 3, 4, 5], Outliers: [100]
    """
    # TODO: Implement this function
    # Hint: Use statistics module for mean and stdev
    import statistics
    pass
```

**Test your function:**
```python
# Test cases
import random
# Generate normal data with some outliers
normal_data = [random.gauss(50, 10) for _ in range(20)]
outlier_data = normal_data + [200, -50, 150]

normal, outliers = find_outliers(outlier_data)
print(f"Normal values ({len(normal)}): {['.1f' for x in sorted(normal)][:5]}...")
print(f"Outliers ({len(outliers)}): {sorted(outliers)}")
```

### Task 3.3: Data Summary Statistics

Create a function that provides summary statistics for a numeric list.

```python
def data_summary(numbers):
    """
    Calculate summary statistics for a list of numbers.

    Args:
        numbers: List of numeric values

    Returns:
        Dictionary with summary statistics

    Examples:
        >>> stats = data_summary([1, 2, 3, 4, 5])
        >>> print(stats['mean'], stats['median'])
        3.0 3.0
    """
    # TODO: Implement this function
    # Hint: Use statistics module
    import statistics
    pass
```

**Test your function:**
```python
# Test cases
test_data = [12, 15, 18, 22, 25, 28, 30, 35, 40, 200]

stats = data_summary(test_data)
print("Summary Statistics:")
for key, value in stats.items():
    if isinstance(value, float):
        print(f"  {key}: {value:.2f}")
    else:
        print(f"  {key}: {value}")
```

---

## 🌍 Exercise 4: Real-World Data Processing

**Goal**: Apply your utilities to process real-world data scenarios

### Task 4.1: Student Grade Processor

Process student grades and generate reports.

```python
def process_student_grades(grades_data):
    """
    Process student grade data and generate summary reports.

    Args:
        grades_data: List of dictionaries with student info
            [{"name": "Alice", "grades": [85, 92, 88]}, ...]

    Returns:
        Dictionary with processed data
    """
    # Sample data
    sample_data = [
        {"name": "Alice", "grades": [85, 92, 88, 95]},
        {"name": "Bob", "grades": [78, 82, 79, 85]},
        {"name": "Charlie", "grades": [92, 88, 95, 98]},
        {"name": "Diana", "grades": [75, 80, 82, 78]},
    ]

    # TODO: Calculate:
    # - Average grade per student
    # - Highest and lowest grades
    # - Students with all grades above 80
    # - Grade distribution (A: 90+, B: 80-89, C: 70-79, D: <70)

    pass
```

**Test your function:**
```python
# Test with sample data
result = process_student_grades([])
print("Student Grade Analysis:")
print(f"Average grades: {result.get('averages', {})}")
print(f"Top performers: {result.get('top_students', [])}")
print(f"Grade distribution: {result.get('distribution', {})}")
```

### Task 4.2: Text Analysis

Analyze text and extract meaningful information.

```python
def analyze_text(text):
    """
    Analyze text and extract statistics.

    Args:
        text: String to analyze

    Returns:
        Dictionary with text analysis results
    """
    # TODO: Calculate:
    # - Word count
    # - Unique words
    # - Most common words (top 5)
    # - Average word length
    # - Sentence count

    pass
```

**Test your function:**
```python
# Test with sample text
sample_text = """
Python is a high-level programming language known for its simplicity and readability.
Python supports multiple programming paradigms including procedural, object-oriented, and functional programming.
Python has a large standard library and a vast ecosystem of third-party packages.
"""

analysis = analyze_text(sample_text)
print("Text Analysis:")
for key, value in analysis.items():
    print(f"  {key}: {value}")
```

---

## 🚀 Challenge Exercises

### Challenge 1: Data Deduplication with Priority

Create a function that removes duplicates but keeps the "best" version based on a priority function.

```python
def deduplicate_with_priority(items, key_func, priority_func):
    """
    Remove duplicates based on key, keeping the item with highest priority.

    Args:
        items: List of items
        key_func: Function to extract comparison key
        priority_func: Function to determine priority (higher = better)

    Returns:
        List with duplicates removed, best version kept
    """
    # TODO: Implement this challenging function
    pass
```

### Challenge 2: Set-Based Text Search

Implement efficient text search using sets.

```python
def build_search_index(documents):
    """
    Build an inverted index for efficient text search.

    Args:
        documents: List of text documents

    Returns:
        Dictionary mapping words to document indices
    """
    # TODO: Build inverted index using sets
    pass

def search_documents(index, query):
    """
    Search documents using the inverted index.

    Args:
        index: Inverted index from build_search_index
        query: Search query (space-separated words)

    Returns:
        Set of document indices that contain all query words
    """
    # TODO: Implement efficient search
    pass
```

---

## ✅ Solution Code

### Exercise 1 Solutions

```python
def remove_duplicates_preserve_order(items):
    """Remove duplicates while preserving order."""
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result

def filter_by_condition(items, condition_func):
    """Filter list using custom condition."""
    return [item for item in items if condition_func(item)]

def group_by_key(items, key_func):
    """Group items by key function."""
    groups = {}
    for item in items:
        key = key_func(item)
        if key not in groups:
            groups[key] = []
        groups[key].append(item)
    return groups
```

### Exercise 2 Solutions

```python
def find_unique_across_lists(*lists):
    """Find elements unique to exactly one list."""
    all_elements = set()
    duplicates = set()

    for lst in lists:
        for item in lst:
            if item in all_elements:
                duplicates.add(item)
            all_elements.add(item)

    return all_elements - duplicates

def intersection_of_lists(*lists):
    """Find elements common to all lists."""
    if not lists:
        return []

    # Start with first list as set
    common = set(lists[0])

    # Intersect with each subsequent list
    for lst in lists[1:]:
        common &= set(lst)

    # Preserve order from first list
    result = [item for item in lists[0] if item in common]
    return result

def remove_common_with_others(main_list, *other_lists):
    """Remove elements from main_list that appear in any other list."""
    # Combine all other lists into one set
    other_elements = set()
    for lst in other_lists:
        other_elements.update(lst)

    # Filter main list
    return [item for item in main_list if item not in other_elements]
```

### Exercise 3 Solutions

```python
def count_frequencies(items):
    """Count frequency of each element."""
    frequencies = {}
    for item in items:
        frequencies[item] = frequencies.get(item, 0) + 1
    return frequencies

def find_outliers(numbers, threshold=2.0):
    """Find outliers using standard deviation."""
    import statistics

    if len(numbers) < 2:
        return numbers, []

    mean = statistics.mean(numbers)
    stdev = statistics.stdev(numbers)

    lower_bound = mean - threshold * stdev
    upper_bound = mean + threshold * stdev

    normal = [x for x in numbers if lower_bound <= x <= upper_bound]
    outliers = [x for x in numbers if x < lower_bound or x > upper_bound]

    return normal, outliers

def data_summary(numbers):
    """Calculate summary statistics."""
    import statistics

    if not numbers:
        return {}

    return {
        'count': len(numbers),
        'mean': statistics.mean(numbers),
        'median': statistics.median(numbers),
        'mode': statistics.mode(numbers) if len(set(numbers)) < len(numbers) else None,
        'min': min(numbers),
        'max': max(numbers),
        'range': max(numbers) - min(numbers),
        'stdev': statistics.stdev(numbers) if len(numbers) > 1 else 0
    }
```

---

## 🧪 Testing Your Solutions

Create a test file to verify your implementations:

```python
# test_utils.py
import data_utils

def test_remove_duplicates():
    """Test remove_duplicates_preserve_order function."""
    assert data_utils.remove_duplicates_preserve_order([1, 2, 2, 3, 1]) == [1, 2, 3]
    assert data_utils.remove_duplicates_preserve_order([]) == []
    print("✓ remove_duplicates tests passed")

def test_filter_by_condition():
    """Test filter_by_condition function."""
    numbers = [1, 2, 3, 4, 5, 6]
    evens = data_utils.filter_by_condition(numbers, lambda x: x % 2 == 0)
    assert evens == [2, 4, 6]
    print("✓ filter_by_condition tests passed")

# Add more tests for other functions...

if __name__ == "__main__":
    test_remove_duplicates()
    test_filter_by_condition()
    print("All tests passed! 🎉")
```

---

## 📝 Key Takeaways

1. **Modular Design**: Break complex tasks into smaller, reusable functions
2. **Performance Matters**: Choose appropriate data structures (lists vs sets)
3. **Clean Code**: Use descriptive names, docstrings, and proper error handling
4. **Testing**: Write tests to verify your functions work correctly
5. **Real-World Application**: Your utilities can solve practical data processing problems

---

## 🔗 Additional Resources

- [Python Data Structures Docs](https://docs.python.org/3/tutorial/datastructures.html)
- [Statistics Module](https://docs.python.org/3/library/statistics.html)
- [Best Practices for Clean Code](https://peps.python.org/pep-0008/)

---

**Workshop Version**: 1.0  
**Last Updated**: February 2026  
**Estimated Completion Time**: 90 minutes
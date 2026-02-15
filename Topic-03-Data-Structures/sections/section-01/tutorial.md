# Tutorial 1: Working with Lists and Sets

**Section**: 6 - Working with Lists and Sets (90 min)  
**Level**: Beginner to Intermediate  
**Prerequisites**: Basic Python syntax, variables, loops, functions

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Master List Operations**: Add, remove, update, sort, and filter list elements
2. **Use List Comprehensions**: Write concise list creation and transformation code
3. **Perform Set Operations**: Union, intersection, difference operations
4. **Convert Between Data Types**: Lists to sets and vice versa
5. **Understand Performance**: When to use lists vs sets for different scenarios

---

## 📚 Table of Contents

1. [Python Lists Fundamentals](#python-lists-fundamentals)
2. [List Operations (Add/Remove/Update)](#list-operations-addremoveupdate)
3. [List Searching and Sorting](#list-searching-and-sorting)
4. [List Comprehensions](#list-comprehensions)
5. [Python Sets Fundamentals](#python-sets-fundamentals)
6. [Set Operations](#set-operations)
7. [Converting Between Lists and Sets](#converting-between-lists-and-sets)
8. [Performance Considerations](#performance-considerations)
9. [Common Pitfalls and Best Practices](#common-pitfalls-and-best-practices)

---

## 🐍 Python Lists Fundamentals

Lists are **ordered, mutable** collections that can contain elements of different data types.

### Creating Lists

```python
# Empty list
empty_list = []

# List with elements
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "orange"]
mixed = [1, "hello", 3.14, True]

# Using list() constructor
from_range = list(range(5))  # [0, 1, 2, 3, 4]
from_string = list("hello")  # ['h', 'e', 'l', 'l', 'o']
```

### Accessing List Elements

```python
fruits = ["apple", "banana", "orange", "grape"]

# Index access (zero-based)
first_fruit = fruits[0]    # "apple"
last_fruit = fruits[-1]    # "grape"
second_last = fruits[-2]   # "orange"

# Slicing [start:end:step]
first_two = fruits[0:2]    # ["apple", "banana"]
every_other = fruits[::2]  # ["apple", "orange"]
reversed_list = fruits[::-1]  # ["grape", "orange", "banana", "apple"]
```

---

## 🔧 List Operations (Add/Remove/Update)

### Adding Elements

```python
numbers = [1, 2, 3]

# append() - Add single element at end
numbers.append(4)  # [1, 2, 3, 4]

# extend() - Add multiple elements
numbers.extend([5, 6])  # [1, 2, 3, 4, 5, 6]

# insert() - Add element at specific position
numbers.insert(0, 0)  # [0, 1, 2, 3, 4, 5, 6]

# Using + operator (creates new list)
more_numbers = numbers + [7, 8]  # [0, 1, 2, 3, 4, 5, 6, 7, 8]
```

### Removing Elements

```python
fruits = ["apple", "banana", "orange", "banana", "grape"]

# remove() - Remove first occurrence of value
fruits.remove("banana")  # ["apple", "orange", "banana", "grape"]

# pop() - Remove and return element at index (default: last)
last_fruit = fruits.pop()    # "grape", fruits = ["apple", "orange", "banana"]
first_fruit = fruits.pop(0)  # "apple", fruits = ["orange", "banana"]

# del statement - Remove by index or slice
del fruits[0]  # fruits = ["banana"]
del fruits[:]  # fruits = [] (clear entire list)

# clear() - Remove all elements
fruits.clear()  # fruits = []
```

### Updating Elements

```python
numbers = [1, 2, 3, 4, 5]

# Direct assignment
numbers[0] = 10  # [10, 2, 3, 4, 5]
numbers[-1] = 50  # [10, 2, 3, 4, 50]

# Multiple assignment
numbers[1:3] = [20, 30]  # [10, 20, 30, 4, 50]

# Using enumerate for indexed updates
for i, value in enumerate(numbers):
    numbers[i] = value * 2  # [20, 40, 60, 8, 100]
```

---

## 🔍 List Searching and Sorting

### Searching in Lists

```python
fruits = ["apple", "banana", "orange", "grape"]

# index() - Find index of first occurrence
apple_index = fruits.index("apple")  # 0
banana_index = fruits.index("banana")  # 1

# index() with start/end parameters
fruits = ["apple", "banana", "orange", "banana"]
second_banana = fruits.index("banana", 2)  # 3 (start searching from index 2)

# count() - Count occurrences
banana_count = fruits.count("banana")  # 2

# in operator - Check membership
has_apple = "apple" in fruits  # True
has_mango = "mango" in fruits  # False
```

### Sorting Lists

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# sort() - Sort in place (modifies original list)
numbers.sort()  # [1, 1, 2, 3, 4, 5, 6, 9]

# sort() with reverse
numbers.sort(reverse=True)  # [9, 6, 5, 4, 3, 2, 1, 1]

# sort() with key function
words = ["Apple", "banana", "Cherry", "date"]
words.sort(key=str.lower)  # ["Apple", "banana", "Cherry", "date"] -> ["Apple", "banana", "Cherry", "date"]

# sorted() - Return new sorted list (doesn't modify original)
original = [3, 1, 4, 1, 5]
sorted_list = sorted(original)  # [1, 1, 3, 4, 5], original unchanged
```

### Filtering Lists

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Using list comprehension (recommended)
even_numbers = [x for x in numbers if x % 2 == 0]  # [2, 4, 6, 8, 10]

# Using filter() function
def is_even(x):
    return x % 2 == 0

even_numbers = list(filter(is_even, numbers))  # [2, 4, 6, 8, 10]

# Complex filtering
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]

high_performers = [s for s in students if s["grade"] >= 80]
# [{"name": "Alice", "grade": 85}, {"name": "Bob", "grade": 92}]
```

---

## ⚡ List Comprehensions

List comprehensions provide a concise way to create and transform lists.

### Basic Syntax

```python
# Traditional approach
squares = []
for x in range(10):
    squares.append(x**2)

# List comprehension
squares = [x**2 for x in range(10)]  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### With Conditional Filtering

```python
# Numbers divisible by 3
numbers = [x for x in range(20) if x % 3 == 0]  # [0, 3, 6, 9, 12, 15, 18]

# Even squares
even_squares = [x**2 for x in range(10) if x**2 % 2 == 0]  # [0, 4, 16, 36, 64]
```

### Nested List Comprehensions

```python
# Flatten a 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [x for row in matrix for x in row]  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create multiplication table
table = [[i*j for j in range(1, 4)] for i in range(1, 4)]
# [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

### Advanced Examples

```python
# String processing
words = ["hello", "world", "python"]
upper_words = [word.upper() for word in words]  # ["HELLO", "WORLD", "PYTHON"]

# Dictionary comprehension (creates list of tuples)
students = {"Alice": 85, "Bob": 92, "Charlie": 78}
passing = [(name, grade) for name, grade in students.items() if grade >= 80]
# [("Alice", 85), ("Bob", 92)]
```

---

## 🔸 Python Sets Fundamentals

Sets are **unordered, mutable** collections of unique elements.

### Creating Sets

```python
# Empty set (not {})
empty_set = set()

# Set with elements
numbers = {1, 2, 3, 4, 5}
fruits = {"apple", "banana", "orange"}

# Using set() constructor
from_list = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3} (duplicates removed)
from_string = set("hello")  # {'h', 'e', 'l', 'o'} (duplicates removed)

# Set comprehension
squares = {x**2 for x in range(5)}  # {0, 1, 4, 9, 16}
```

### Set Operations

```python
# Membership testing
fruits = {"apple", "banana", "orange"}
has_apple = "apple" in fruits  # True
has_mango = "mango" in fruits  # False

# Length
fruit_count = len(fruits)  # 3
```

---

## 🔧 Set Operations

### Adding Elements

```python
fruits = {"apple", "banana"}

# add() - Add single element
fruits.add("orange")  # {"apple", "banana", "orange"}

# update() - Add multiple elements
fruits.update(["grape", "kiwi"])  # {"apple", "banana", "orange", "grape", "kiwi"}
```

### Removing Elements

```python
fruits = {"apple", "banana", "orange", "grape"}

# remove() - Remove element (raises KeyError if not found)
fruits.remove("banana")  # {"apple", "orange", "grape"}

# discard() - Remove element (no error if not found)
fruits.discard("mango")  # No change, no error

# pop() - Remove and return arbitrary element
removed = fruits.pop()  # Removes random element

# clear() - Remove all elements
fruits.clear()  # set()
```

### Mathematical Set Operations

```python
set_a = {1, 2, 3, 4, 5}
set_b = {4, 5, 6, 7, 8}

# Union (all elements from both sets)
union_result = set_a | set_b  # {1, 2, 3, 4, 5, 6, 7, 8}
union_method = set_a.union(set_b)  # Same result

# Intersection (common elements)
intersection_result = set_a & set_b  # {4, 5}
intersection_method = set_a.intersection(set_b)  # Same result

# Difference (elements in set_a but not in set_b)
difference_result = set_a - set_b  # {1, 2, 3}
difference_method = set_a.difference(set_b)  # Same result

# Symmetric difference (elements in either set but not both)
symmetric_diff = set_a ^ set_b  # {1, 2, 3, 6, 7, 8}
symmetric_method = set_a.symmetric_difference(set_b)  # Same result
```

### In-Place Operations

```python
set_a = {1, 2, 3}
set_b = {3, 4, 5}

# In-place union
set_a |= set_b  # set_a becomes {1, 2, 3, 4, 5}

# In-place intersection
set_a &= set_b  # set_a becomes {3, 4, 5}

# In-place difference
set_a -= set_b  # set_a becomes set()

# In-place symmetric difference
set_a ^= set_b  # set_a becomes {3}
```

---

## 🔄 Converting Between Lists and Sets

### List to Set

```python
# Remove duplicates from list
numbers = [1, 2, 2, 3, 3, 3, 4]
unique_numbers = list(set(numbers))  # [1, 2, 3, 4] (order may change)

# Preserve order while removing duplicates (Python 3.7+)
unique_ordered = list(dict.fromkeys(numbers))  # [1, 2, 3, 4]
```

### Set to List

```python
fruits = {"apple", "banana", "orange"}
fruit_list = list(fruits)  # ["apple", "banana", "orange"] (order may vary)

# Sort when converting
sorted_fruits = sorted(fruits)  # ["apple", "banana", "orange"]
```

### Practical Examples

```python
# Find unique words in text
text = "the quick brown fox jumps over the lazy dog"
words = text.split()
unique_words = set(words)  # {'the', 'quick', 'brown', 'fox', 'jumps', 'over', 'lazy', 'dog'}

# Find common elements between lists
list1 = [1, 2, 3, 4, 5]
list2 = [3, 4, 5, 6, 7]
common = list(set(list1) & set(list2))  # [3, 4, 5]

# Remove specific values from list
numbers = [1, 2, 3, 2, 4, 2, 5]
to_remove = {2, 4}
filtered = [x for x in numbers if x not in to_remove]  # [1, 3, 5]
```

---

## ⚡ Performance Considerations

### When to Use Lists

```python
# Lists are better for:
# - Ordered collections
# - Index-based access
# - Duplicate values allowed
# - Sequential processing

numbers = [1, 2, 3, 4, 5]
first = numbers[0]        # O(1) - Fast
numbers.append(6)         # O(1) - Fast (amortized)
numbers.insert(0, 0)      # O(n) - Slow for large lists
```

### When to Use Sets

```python
# Sets are better for:
# - Membership testing
# - Removing duplicates
# - Mathematical set operations
# - Unordered unique collections

unique_items = {1, 2, 3, 4, 5}
has_item = 3 in unique_items    # O(1) - Very fast
unique_items.add(6)             # O(1) - Fast
unique_items.remove(3)          # O(1) - Fast
```

### Performance Comparison

```python
import time

# Large datasets
large_list = list(range(100000))
large_set = set(range(100000))

# Membership testing
start = time.time()
result = 99999 in large_list  # O(n) - Slow
list_time = time.time() - start

start = time.time()
result = 99999 in large_set   # O(1) - Fast
set_time = time.time() - start

print(f"List lookup: {list_time:.6f}s")
print(f"Set lookup: {set_time:.6f}s")
print(f"Sets are {list_time/set_time:.0f}x faster for membership testing")
```

---

## ⚠️ Common Pitfalls and Best Practices

### Common Mistakes

```python
# Mistake: Using {} for empty set
empty_dict = {}  # This creates a dictionary!
empty_set = set()  # This creates a set

# Mistake: Modifying list while iterating
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Dangerous! Skips elements

# Correct way: Iterate over copy or use list comprehension
numbers = [num for num in numbers if num % 2 != 0]  # [1, 3, 5]

# Mistake: Using mutable objects as set elements
# Sets require hashable (immutable) elements
mutable_set = {[1, 2], [3, 4]}  # TypeError!

# Correct: Use tuples for immutable collections
immutable_set = {(1, 2), (3, 4)}  # Works fine
```

### Best Practices

```python
# Use descriptive variable names
student_names = ["Alice", "Bob", "Charlie"]
unique_grades = {85, 92, 78, 85}  # Duplicates automatically removed

# Prefer list comprehensions over loops when possible
squares = [x**2 for x in range(10)]  # Clean and readable

# Use sets for membership testing
valid_colors = {"red", "green", "blue"}
if user_color in valid_colors:  # O(1) lookup
    print("Valid color")

# Use appropriate data structures
# List: ordered, allows duplicates, index access
# Set: unordered, unique elements, fast membership testing

# Chain operations for readability
result = sorted(list(set(numbers)))  # Remove duplicates and sort

# Use generator expressions for memory efficiency with large data
large_squares = (x**2 for x in range(1000000))  # Memory efficient
sum_of_squares = sum(large_squares)  # Only computes when needed
```

---

## 🎯 Key Takeaways

1. **Lists** are ordered, mutable sequences that allow duplicates and provide index-based access
2. **Sets** are unordered, mutable collections of unique elements optimized for membership testing
3. **List comprehensions** provide concise syntax for creating and transforming lists
4. **Set operations** (union, intersection, difference) enable powerful data manipulation
5. **Performance matters**: Choose the right data structure for your use case
6. **Avoid common pitfalls**: Don't modify lists while iterating, use proper set creation syntax

---

## 🔗 Further Reading

- [Python Data Structures Documentation](https://docs.python.org/3/tutorial/datastructures.html)
- [Time Complexity Analysis](https://wiki.python.org/moin/TimeComplexity)
- [Set Operations in Python](https://realpython.com/python-sets/)
- [List Comprehensions Guide](https://realpython.com/list-comprehension-python/)

---

## 📝 Practice Exercises

Try these exercises to reinforce your understanding:

1. Create a list of numbers from 1-100, then filter for even numbers using list comprehension
2. Given two lists, find their intersection, union, and symmetric difference
3. Remove duplicates from a list while preserving order
4. Create a set comprehension that generates squares of even numbers
5. Compare the performance of list vs set membership testing with large datasets

---

**Tutorial Version**: 1.0  
**Last Updated**: February 2026  
**Estimated Reading Time**: 45 minutes
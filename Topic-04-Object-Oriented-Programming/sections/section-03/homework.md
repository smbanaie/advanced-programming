# Homework 3: Custom Container Class

**Section**: 3 - Advanced OOP Concepts
**Estimated Time**: 4-6 hours
**Difficulty**: Intermediate
**Prerequisites**: Section 3 Tutorial & Workshop

---

## 📋 Assignment Overview

Create a comprehensive custom container class that implements sequence-like behavior using advanced Object-Oriented Programming concepts. You'll build a data structure that behaves like Python's built-in containers (lists, tuples) while providing additional functionality through magic methods, property decorators, and sophisticated class design.

**Learning Focus**: Real-world application of advanced OOP features in container design

---

## 🎯 Learning Objectives

By completing this assignment, you will demonstrate the ability to:

1. **Implement Magic Methods**: Create sequence-like behavior with `__getitem__`, `__setitem__`, `__len__`, etc.
2. **Apply Property Decorators**: Control access to container metadata and configuration
3. **Use Class and Static Methods**: Provide alternative constructors and utility functions
4. **Design Advanced Classes**: Combine multiple OOP concepts in a sophisticated container
5. **Handle Edge Cases**: Implement robust error handling and validation

---

## 📋 Requirements

### Core Requirements

#### 1. Custom Container Class
Create a `SmartList` class that extends Python's sequence protocol:

**Required Magic Methods:**
- `__len__()` - Return number of items
- `__getitem__(index)` - Get item by index (support negative indexing)
- `__setitem__(index, value)` - Set item by index
- `__delitem__(index)` - Delete item by index
- `__iter__()` - Make container iterable
- `__reversed__()` - Support reverse iteration
- `__contains__(item)` - Support `in` operator
- `__eq__(other)` - Compare with other sequences
- `__add__(other)` - Concatenate with other sequences
- `__mul__(n)` - Repeat sequence n times
- `__str__()` and `__repr__()` - String representations

#### 2. Advanced Features with Property Decorators
Implement configurable behavior through properties:

**Metadata Properties:**
- `size` - Current number of elements (read-only)
- `capacity` - Maximum capacity (configurable)
- `is_empty` - Check if container is empty (read-only)
- `is_full` - Check if container reached capacity (read-only)

**Configuration Properties:**
- `allow_duplicates` - Control duplicate item handling
- `auto_sort` - Automatically sort items when added
- `sort_key` - Custom sorting function
- `max_item_size` - Maximum size per item (for validation)

#### 3. Class and Static Methods
Provide alternative ways to create and work with SmartList:

**Class Methods:**
- `from_iterable(iterable)` - Create from any iterable
- `from_csv(file_path, delimiter=',')` - Create from CSV file
- `from_json(file_path)` - Create from JSON array
- `repeat(value, n)` - Create list with n copies of value

**Static Methods:**
- `validate_item(item)` - Validate if item can be added
- `merge(*lists)` - Merge multiple SmartList instances
- `find_common(*lists)` - Find common elements across lists
- `remove_duplicates(items)` - Remove duplicates from sequence

#### 4. Dataclass Integration
Use dataclasses for supporting data structures:

**Operation Record Dataclass:**
```python
@dataclass
class OperationRecord:
    operation: str  # 'add', 'remove', 'sort', etc.
    timestamp: datetime
    details: str
    affected_items: List[Any]
```

**Container Statistics Dataclass:**
```python
@dataclass
class ContainerStats:
    total_operations: int
    average_item_size: float
    most_frequent_operation: str
    creation_time: datetime
```

### Implementation Requirements

#### Container Behavior
- **Sequence Protocol**: Behave like Python lists for indexing and iteration
- **Type Flexibility**: Accept any hashable/immutable types
- **Memory Efficiency**: Implement capacity limits and growth strategies
- **Thread Safety**: Consider concurrent access patterns

#### Advanced Features
- **Operation History**: Track all operations performed
- **Undo/Redo**: Implement basic undo functionality
- **Filtering**: Support advanced filtering with lambda functions
- **Aggregation**: Provide statistical methods (sum, average, min, max)

#### Error Handling
- **Index Errors**: Handle out-of-bounds access gracefully
- **Type Errors**: Validate input types appropriately
- **Capacity Errors**: Handle capacity limits and overflow
- **Validation Errors**: Custom validation for item constraints

---

## 🏗️ Implementation Guide

### Step 1: Basic Container Structure

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Any, List, Callable, Optional, Union, Iterable
import json
import csv

@dataclass
class OperationRecord:
    operation: str
    timestamp: datetime = field(default_factory=datetime.now)
    details: str = ""
    affected_items: List[Any] = field(default_factory=list)

@dataclass
class ContainerStats:
    total_operations: int = 0
    average_item_size: float = 0.0
    most_frequent_operation: str = ""
    creation_time: datetime = field(default_factory=datetime.now)

class SmartList:
    """Advanced container implementing sequence protocol with OOP features"""

    def __init__(self, items=None, capacity=None):
        self._items = list(items or [])
        self._capacity = capacity
        self._allow_duplicates = True
        self._auto_sort = False
        self._sort_key = None
        self._max_item_size = None
        self._operation_history = []
        self._stats = ContainerStats()

        # Validate initial items
        for item in self._items:
            self._validate_item(item)

        self._record_operation("create", f"Created SmartList with {len(self._items)} items")
```

### Step 2: Property Decorators

```python
# Property implementations
@property
def size(self):
    """Current number of items (read-only)"""
    return len(self._items)

@property
def capacity(self):
    """Maximum capacity of the container"""
    return self._capacity

@capacity.setter
def capacity(self, value):
    """Set capacity with validation"""
    if value is not None and value < len(self._items):
        raise ValueError(f"Capacity {value} is less than current size {len(self._items)}")
    self._capacity = value

@property
def allow_duplicates(self):
    return self._allow_duplicates

@allow_duplicates.setter
def allow_duplicates(self, value):
    if not isinstance(value, bool):
        raise TypeError("allow_duplicates must be boolean")
    self._allow_duplicates = value

@property
def auto_sort(self):
    return self._auto_sort

@auto_sort.setter
def auto_sort(self, value):
    if not isinstance(value, bool):
        raise TypeError("auto_sort must be boolean")
    self._auto_sort = value
    if self._auto_sort:
        self.sort()

@property
def is_empty(self):
    return len(self._items) == 0

@property
def is_full(self):
    return self._capacity is not None and len(self._items) >= self._capacity
```

### Step 3: Magic Methods Implementation

```python
# Sequence protocol implementation
def __len__(self):
    return len(self._items)

def __getitem__(self, index):
    if isinstance(index, slice):
        # Handle slice notation
        result = self._items[index]
        return SmartList(result)
    else:
        # Handle single index
        return self._items[index]

def __setitem__(self, index, value):
    self._validate_item(value)

    if isinstance(index, slice):
        # Handle slice assignment
        self._items[index] = value
        self._record_operation("set_slice", f"Set slice {index} to {value}")
    else:
        old_value = self._items[index] if 0 <= index < len(self._items) else None
        self._items[index] = value
        self._record_operation("set", f"Changed index {index} from {old_value} to {value}")

def __delitem__(self, index):
    if isinstance(index, slice):
        deleted_items = self._items[index]
        del self._items[index]
        self._record_operation("del_slice", f"Deleted slice {index}", deleted_items)
    else:
        deleted_item = self._items[index]
        del self._items[index]
        self._record_operation("del", f"Deleted item at index {index}", [deleted_item])

def __iter__(self):
    return iter(self._items)

def __reversed__(self):
    return reversed(self._items)

def __contains__(self, item):
    return item in self._items

def __eq__(self, other):
    if not isinstance(other, (list, tuple, SmartList)):
        return False
    return self._items == list(other)

def __add__(self, other):
    if not isinstance(other, (list, tuple, SmartList)):
        return NotImplemented
    new_items = self._items + list(other)
    result = SmartList(new_items)
    result._record_operation("concatenate", f"Combined with {len(list(other))} items")
    return result

def __mul__(self, n):
    if not isinstance(n, int) or n < 0:
        return NotImplemented
    new_items = self._items * n
    result = SmartList(new_items)
    result._record_operation("repeat", f"Repeated {n} times")
    return result

def __str__(self):
    return f"SmartList({self._items})"

def __repr__(self):
    return f"SmartList(items={self._items!r}, capacity={self._capacity!r})"
```

### Step 4: Advanced Methods

```python
def append(self, item):
    """Add item to end of list"""
    self._validate_item(item)

    if self.is_full:
        raise OverflowError("Container is at maximum capacity")

    if not self._allow_duplicates and item in self._items:
        raise ValueError(f"Duplicates not allowed: {item} already exists")

    self._items.append(item)
    self._record_operation("append", f"Added item {item}")

    if self._auto_sort:
        self.sort()

def insert(self, index, item):
    """Insert item at specific index"""
    self._validate_item(item)

    if self.is_full:
        raise OverflowError("Container is at maximum capacity")

    self._items.insert(index, item)
    self._record_operation("insert", f"Inserted {item} at index {index}")

    if self._auto_sort:
        self.sort()

def remove(self, item):
    """Remove first occurrence of item"""
    self._items.remove(item)
    self._record_operation("remove", f"Removed item {item}")

def pop(self, index=-1):
    """Remove and return item at index"""
    item = self._items.pop(index)
    self._record_operation("pop", f"Popped item {item} at index {index}")
    return item

def sort(self, key=None, reverse=False):
    """Sort the list"""
    sort_key = key or self._sort_key
    self._items.sort(key=sort_key, reverse=reverse)
    self._record_operation("sort", f"Sorted with key={sort_key}, reverse={reverse}")

def filter(self, predicate):
    """Return new SmartList with filtered items"""
    filtered_items = [item for item in self._items if predicate(item)]
    result = SmartList(filtered_items)
    result._record_operation("filter", f"Filtered using {predicate}")
    return result

def map(self, func):
    """Return new SmartList with function applied to each item"""
    mapped_items = [func(item) for item in self._items]
    result = SmartList(mapped_items)
    result._record_operation("map", f"Applied function {func}")
    return result
```

### Step 5: Class and Static Methods

```python
@classmethod
def from_iterable(cls, iterable):
    """Create SmartList from any iterable"""
    return cls(list(iterable))

@classmethod
def from_csv(cls, file_path, delimiter=',', column=0):
    """Create SmartList from CSV file column"""
    items = []
    with open(file_path, 'r') as file:
        reader = csv.reader(file, delimiter=delimiter)
        for row in reader:
            if len(row) > column:
                items.append(row[column])
    return cls(items)

@classmethod
def from_json(cls, file_path):
    """Create SmartList from JSON array file"""
    with open(file_path, 'r') as file:
        data = json.load(file)
    return cls(data)

@classmethod
def repeat(cls, value, n):
    """Create SmartList with n copies of value"""
    return cls([value] * n)

@staticmethod
def validate_item(item):
    """Validate if item can be added to container"""
    # Basic validation - can be extended
    if item is None:
        raise ValueError("None values not allowed")
    return True

@staticmethod
def merge(*lists):
    """Merge multiple SmartList instances"""
    all_items = []
    for lst in lists:
        if isinstance(lst, SmartList):
            all_items.extend(lst._items)
        else:
            all_items.extend(lst)
    return SmartList(all_items)

@staticmethod
def find_common(*lists):
    """Find common elements across multiple lists"""
    if not lists:
        return SmartList()

    # Convert all to sets for intersection
    sets = []
    for lst in lists:
        if isinstance(lst, SmartList):
            sets.append(set(lst._items))
        else:
            sets.append(set(lst))

    common = set.intersection(*sets)
    return SmartList(list(common))

@staticmethod
def remove_duplicates(items):
    """Remove duplicates from sequence while preserving order"""
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return SmartList(result)
```

### Step 6: Utility Methods

```python
def get_stats(self):
    """Get container statistics"""
    if not self._operation_history:
        return self._stats

    # Calculate statistics
    operations = [op.operation for op in self._operation_history]
    most_common = max(set(operations), key=operations.count) if operations else ""

    total_item_size = sum(len(str(item)) for item in self._items)
    avg_size = total_item_size / len(self._items) if self._items else 0

    self._stats.total_operations = len(self._operation_history)
    self._stats.average_item_size = avg_size
    self._stats.most_frequent_operation = most_common

    return self._stats

def get_history(self, operation=None, limit=None):
    """Get operation history, optionally filtered"""
    history = self._operation_history
    if operation:
        history = [op for op in history if op.operation == operation]
    if limit:
        history = history[-limit:]
    return history

def undo_last_operation(self):
    """Basic undo functionality (simplified)"""
    if not self._operation_history:
        raise ValueError("No operations to undo")

    last_op = self._operation_history.pop()

    # Simplified undo logic - in real implementation would be more sophisticated
    if last_op.operation == "append":
        if self._items:
            self._items.pop()
    elif last_op.operation == "remove":
        if last_op.affected_items:
            self._items.extend(last_op.affected_items)

    return f"Undid operation: {last_op.operation}"

def _validate_item(self, item):
    """Internal item validation"""
    SmartList.validate_item(item)

    if self._max_item_size is not None:
        item_size = len(str(item))
        if item_size > self._max_item_size:
            raise ValueError(f"Item size {item_size} exceeds maximum {self._max_item_size}")

def _record_operation(self, operation, details, affected_items=None):
    """Record an operation in history"""
    record = OperationRecord(
        operation=operation,
        details=details,
        affected_items=affected_items or []
    )
    self._operation_history.append(record)
```

---

## 🧪 Testing Requirements

Create a comprehensive test script that demonstrates:

```python
# test_smartlist.py
from smartlist import SmartList, OperationRecord, ContainerStats
import tempfile
import json

def test_smart_list():
    print("📋 Testing SmartList Container")
    print("=" * 50)

    # Test basic functionality
    smart_list = SmartList([1, 2, 3, 4, 5])
    print(f"Created: {smart_list}")
    print(f"Length: {len(smart_list)}")
    print(f"Item at index 2: {smart_list[2]}")
    print()

    # Test magic methods
    print("🎩 Testing Magic Methods:")
    print(f"Contains 3: {3 in smart_list}")
    print(f"Slice [1:4]: {smart_list[1:4]}")
    print(f"Reversed: {list(reversed(smart_list))}")
    print(f"smart_list * 2: {smart_list * 2}")
    print(f"smart_list + [6, 7]: {smart_list + [6, 7]}")
    print()

    # Test property decorators
    print("🛡️ Testing Properties:")
    print(f"Size: {smart_list.size}")
    print(f"Is empty: {smart_list.is_empty}")
    smart_list.capacity = 10
    print(f"Capacity: {smart_list.capacity}")
    print(f"Is full: {smart_list.is_full}")
    print()

    # Test advanced features
    print("⚡ Testing Advanced Features:")
    smart_list.append(6)
    print(f"After append(6): {smart_list}")

    smart_list.insert(0, 0)
    print(f"After insert(0, 0): {smart_list}")

    filtered = smart_list.filter(lambda x: x % 2 == 0)
    print(f"Even numbers: {filtered}")

    squared = smart_list.map(lambda x: x ** 2)
    print(f"Squared values: {squared}")
    print()

    # Test class methods
    print("🏭 Testing Class Methods:")
    from_range = SmartList.from_iterable(range(5))
    print(f"From range(5): {from_range}")

    repeated = SmartList.repeat("hello", 3)
    print(f"Repeat 'hello' 3 times: {repeated}")

    merged = SmartList.merge([1, 2], [3, 4], [5, 6])
    print(f"Merged lists: {merged}")

    common = SmartList.find_common([1, 2, 3], [2, 3, 4], [3, 4, 5])
    print(f"Common elements: {common}")
    print()

    # Test configuration
    print("⚙️ Testing Configuration:")
    smart_list.allow_duplicates = False
    smart_list.auto_sort = True
    print(f"Duplicates allowed: {smart_list.allow_duplicates}")
    print(f"Auto sort: {smart_list.auto_sort}")

    smart_list.append(10)
    print(f"After append(10) with auto-sort: {smart_list}")
    print()

    # Test operation history
    print("📚 Testing Operation History:")
    history = smart_list.get_history(limit=3)
    print(f"Last 3 operations:")
    for op in history:
        print(f"  {op.timestamp}: {op.operation} - {op.details}")

    stats = smart_list.get_stats()
    print(f"Container stats: {stats}")

if __name__ == "__main__":
    test_smart_list()
```

---

## 📋 Submission Requirements

### Code Files
- `smartlist.py` - Main SmartList implementation with all required features
- `test_smartlist.py` - Comprehensive test script
- `README.md` - Documentation explaining design decisions and usage examples

### Documentation Requirements
- Class diagram showing relationships between SmartList and dataclasses
- Explanation of design decisions for each magic method implementation
- Performance considerations for large containers
- Error handling strategies

### Test Results
Your test script should demonstrate:
- All magic methods working correctly
- Property decorators functioning properly
- Class and static methods operating as expected
- Edge cases handled gracefully
- Operation history and statistics working

---

## 🎯 Evaluation Criteria

### Code Quality (40%)
- Proper magic method implementation
- Effective use of property decorators
- Appropriate class and static method usage
- Clean, readable code structure

### Functionality (40%)
- Complete sequence protocol implementation
- Advanced features working correctly
- Error handling and validation
- Dataclass integration

### Testing (20%)
- Comprehensive test coverage
- Edge cases and error conditions tested
- Clear test output and documentation

### Documentation (10%)
- Clear code comments and docstrings
- README with usage examples
- Design decision explanations

---

## 🚀 Extension Challenges

### Advanced Features
1. **Persistent Storage** - Add save/load functionality with different formats
2. **Concurrent Access** - Implement thread-safe operations
3. **Indexing Strategies** - Add different indexing methods (hash-based, tree-based)
4. **Query Language** - Implement a simple query syntax for filtering

### Performance Optimizations
1. **Lazy Evaluation** - Implement lazy operations for large datasets
2. **Memory Pool** - Optimize memory usage for repeated operations
3. **Caching** - Cache expensive operations and results
4. **Vectorization** - Support numpy-like operations for numerical data

---

## 🆘 Getting Help

### Common Issues
1. **IndexError in __getitem__** - Handle negative indexing and bounds checking
2. **TypeError in magic methods** - Return NotImplemented for unsupported operations
3. **RecursionError** - Avoid infinite recursion in property getters
4. **MemoryError** - Handle large datasets efficiently

### Resources
- Python data model documentation
- Built-in container source code
- Advanced Python OOP tutorials

### Debug Tips
- Test each magic method individually
- Use assertions to validate behavior
- Profile memory usage for large containers
- Check operation history for debugging

---

## ✅ Success Criteria

Your SmartList implementation is complete when:

- ✅ All sequence protocol methods work like built-in lists
- ✅ Property decorators control access and provide validation
- ✅ Class methods enable flexible object creation
- ✅ Static methods provide utility functions
- ✅ Dataclasses manage operation data effectively
- ✅ Advanced features enhance container functionality
- ✅ Comprehensive tests demonstrate all features
- ✅ Code is well-documented and maintainable

---

**Estimated Completion Time**: 4-6 hours
**Difficulty Level**: Intermediate
**Submission Deadline**: [Set by instructor]

Create a powerful, flexible container that showcases advanced OOP concepts while providing practical utility!
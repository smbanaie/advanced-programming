# Tutorial 3: Advanced OOP Concepts

**Section**: 3 - Advanced OOP Concepts (90 min)
**Level**: Intermediate
**Prerequisites**: Sections 1-2 (OOP Fundamentals, Inheritance & Polymorphism)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Implement Magic Methods**: Customize object behavior with special methods like `__str__`, `__eq__`, `__len__`
2. **Use Property Decorators**: Create controlled access to attributes with getters, setters, and deleters
3. **Apply Class and Static Methods**: Understand when and how to use `@classmethod` and `@staticmethod`
4. **Work with Dataclasses**: Use Python's dataclasses for simplified data container classes
5. **Design Advanced Classes**: Combine multiple OOP concepts in sophisticated class designs

---

## 📚 Table of Contents

1. [Magic Methods](#magic-methods)
2. [Property Decorators](#property-decorators)
3. [Class and Static Methods](#class-and-static-methods)
4. [Dataclasses](#dataclasses)
5. [Advanced Class Design](#advanced-class-design)
6. [Best Practices](#best-practices)

---

## 🎩 Magic Methods

Magic methods (also called dunder methods) are special methods that start and end with double underscores. They allow you to customize how objects behave with built-in Python operations.

### Object Representation

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        """String representation for end users"""
        return f"{self.name}, age {self.age}"

    def __repr__(self):
        """Detailed representation for developers/debugging"""
        return f"Person(name='{self.name}', age={self.age})"

# Usage
person = Person("Alice", 30)
print(person)        # Alice, age 30  (uses __str__)
print(repr(person))  # Person(name='Alice', age=30)  (uses __repr__)
```

### Comparison Operators

```python
class Book:
    def __init__(self, title, author, year):
        self.title = title
        self.author = author
        self.year = year

    def __eq__(self, other):
        """Check equality"""
        if not isinstance(other, Book):
            return False
        return (self.title == other.title and
                self.author == other.author and
                self.year == other.year)

    def __lt__(self, other):
        """Less than comparison (for sorting)"""
        if not isinstance(other, Book):
            return NotImplemented
        return self.year < other.year

    def __hash__(self):
        """Make object hashable (required for sets/dicts)"""
        return hash((self.title, self.author, self.year))

# Usage
book1 = Book("1984", "Orwell", 1949)
book2 = Book("1984", "Orwell", 1949)
book3 = Book("Brave New World", "Huxley", 1932)

print(book1 == book2)  # True
print(book1 == book3)  # False
print(book3 < book1)   # True (1932 < 1949)

# Can now use in sets and as dict keys
books = {book1, book2, book3}  # Only 2 unique books
```

### Arithmetic Operators

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        """Vector addition"""
        if not isinstance(other, Vector):
            return NotImplemented
        return Vector(self.x + other.x, self.y + other.y)

    def __mul__(self, scalar):
        """Scalar multiplication"""
        if not isinstance(scalar, (int, float)):
            return NotImplemented
        return Vector(self.x * scalar, self.y * scalar)

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

# Usage
v1 = Vector(1, 2)
v2 = Vector(3, 4)

print(v1 + v2)  # Vector(4, 6)
print(v1 * 3)   # Vector(3, 6)
```

### Container Methods

```python
class ShoppingCart:
    def __init__(self):
        self.items = []

    def add_item(self, item, quantity=1):
        self.items.append((item, quantity))

    def __len__(self):
        """Return total number of items"""
        return sum(quantity for _, quantity in self.items)

    def __getitem__(self, index):
        """Get item by index"""
        return self.items[index]

    def __setitem__(self, index, value):
        """Set item by index"""
        self.items[index] = value

    def __delitem__(self, index):
        """Delete item by index"""
        del self.items[index]

    def __iter__(self):
        """Make cart iterable"""
        return iter(self.items)

# Usage
cart = ShoppingCart()
cart.add_item("Apple", 3)
cart.add_item("Banana", 2)

print(len(cart))  # 5 (total quantity)
print(cart[0])    # ('Apple', 3)

for item, qty in cart:
    print(f"{item}: {qty}")
```

---

## 🏠 Property Decorators

Properties allow you to control access to attributes, adding validation, computation, or side effects.

### Basic Properties

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        """Get temperature in Celsius"""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """Set temperature in Celsius with validation"""
        if value < -273.15:
            raise ValueError("Temperature cannot be below absolute zero")
        self._celsius = value

    @property
    def fahrenheit(self):
        """Computed property - get temperature in Fahrenheit"""
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        """Set temperature in Fahrenheit"""
        self._celsius = (value - 32) * 5/9

# Usage
temp = Temperature()
temp.celsius = 25
print(temp.celsius)      # 25
print(temp.fahrenheit)   # 77.0

temp.fahrenheit = 68
print(temp.celsius)      # 20.0
```

### Property with Deleter

```python
class BankAccount:
    def __init__(self, account_number, initial_balance=0):
        self.account_number = account_number
        self._balance = initial_balance

    @property
    def balance(self):
        """Get current balance"""
        return self._balance

    @balance.setter
    def balance(self, amount):
        """Set balance with validation"""
        if amount < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = amount

    @balance.deleter
    def balance(self):
        """Reset balance to zero (with confirmation)"""
        confirm = input(f"Really delete balance for account {self.account_number}? (yes/no): ")
        if confirm.lower() == 'yes':
            self._balance = 0
            print("Balance reset to zero")
        else:
            print("Balance deletion cancelled")

# Usage
account = BankAccount("12345", 1000)
print(account.balance)  # 1000

del account.balance     # Prompts for confirmation
```

---

## 🏭 Class and Static Methods

Class methods work with the class itself, while static methods are utility functions that don't need class or instance context.

### Class Methods

```python
class Employee:
    company_name = "TechCorp"
    employee_count = 0

    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.employee_count += 1

    @classmethod
    def from_string(cls, employee_string):
        """Create Employee from string format: 'name:salary'"""
        name, salary = employee_string.split(':')
        return cls(name, float(salary))

    @classmethod
    def get_company_info(cls):
        """Get company-wide information"""
        return f"{cls.company_name} has {cls.employee_count} employees"

    @classmethod
    def set_company_name(cls, new_name):
        """Change company name for all employees"""
        cls.company_name = new_name

# Usage
emp1 = Employee("Alice", 75000)
emp2 = Employee.from_string("Bob:80000")  # Alternative constructor

print(Employee.get_company_info())  # TechCorp has 2 employees

Employee.set_company_name("NewTech Corp")
print(Employee.get_company_info())  # NewTech Corp has 2 employees
```

### Static Methods

```python
class MathUtils:
    @staticmethod
    def is_prime(number):
        """Check if a number is prime (no self or cls needed)"""
        if number < 2:
            return False
        for i in range(2, int(number ** 0.5) + 1):
            if number % i == 0:
                return False
        return True

    @staticmethod
    def fibonacci(n):
        """Generate nth Fibonacci number"""
        if n <= 1:
            return n
        return MathUtils.fibonacci(n-1) + MathUtils.fibonacci(n-2)

# Usage
print(MathUtils.is_prime(17))      # True
print(MathUtils.fibonacci(10))     # 55

# Can also be called on instances (though not recommended)
utils = MathUtils()
print(utils.is_prime(23))          # True (works but confusing)
```

### When to Use Each

- **Instance methods**: Work with specific object data
- **Class methods**: Work with class-level data or create alternative constructors
- **Static methods**: Utility functions that don't need class/instance context

---

## 📊 Dataclasses

Dataclasses automatically generate special methods based on class attributes, reducing boilerplate code.

### Basic Dataclass

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    email: str = ""  # Default value

# Automatically generates:
# - __init__(self, name, age, email="")
# - __repr__(self)
# - __eq__(self, other)

# Usage
person1 = Person("Alice", 30, "alice@example.com")
person2 = Person("Bob", 25)

print(person1)  # Person(name='Alice', age=30, email='alice@example.com')
print(person2)  # Person(name='Bob', age=25, email='')

print(person1 == person2)  # False
```

### Advanced Dataclass Features

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Student:
    name: str
    student_id: int
    grades: List[int] = field(default_factory=list)  # Mutable default
    active: bool = True

    def __post_init__(self):
        """Called after __init__ - for validation/customization"""
        if self.student_id <= 0:
            raise ValueError("Student ID must be positive")

    def add_grade(self, grade):
        """Add a grade to the student's record"""
        if not 0 <= grade <= 100:
            raise ValueError("Grade must be between 0 and 100")
        self.grades.append(grade)

    @property
    def average_grade(self):
        """Computed property for average grade"""
        return sum(self.grades) / len(self.grades) if self.grades else 0

# Usage
student = Student("Alice", 12345)
student.add_grade(95)
student.add_grade(87)

print(student)                    # Student(name='Alice', student_id=12345, grades=[95, 87], active=True)
print(student.average_grade)      # 91.0
```

### Dataclass with Custom Methods

```python
from dataclasses import dataclass
from datetime import datetime

@dataclass(frozen=True)  # Immutable dataclass
class Product:
    name: str
    price: float
    category: str
    created_at: datetime = field(default_factory=datetime.now)

    def __str__(self):
        return f"{self.name} (${self.price:.2f}) - {self.category}"

    def apply_discount(self, percentage):
        """Return new Product with discounted price"""
        discount_amount = self.price * (percentage / 100)
        return Product(
            name=f"{self.name} ({percentage}% off)",
            price=self.price - discount_amount,
            category=self.category,
            created_at=self.created_at
        )

# Usage
product = Product("Laptop", 999.99, "Electronics")
discounted = product.apply_discount(10)

print(product)      # Laptop ($999.99) - Electronics
print(discounted)   # Laptop (10% off) ($899.99) - Electronics
```

---

## 🏗️ Advanced Class Design

Combining multiple advanced OOP concepts in sophisticated designs.

### Complete Example: Smart Home Device

```python
from dataclasses import dataclass, field
from typing import Dict, List
from datetime import datetime

@dataclass
class DeviceEvent:
    timestamp: datetime = field(default_factory=datetime.now)
    event_type: str = ""
    description: str = ""

class SmartDevice:
    """Advanced smart device with multiple OOP concepts"""

    device_count = 0

    def __init__(self, name, device_type, location="Unknown"):
        self._name = name
        self._device_type = device_type
        self._location = location
        self._is_on = False
        self._events = []
        SmartDevice.device_count += 1

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not value.strip():
            raise ValueError("Device name cannot be empty")
        self._name = value.strip()

    @property
    def is_on(self):
        return self._is_on

    @classmethod
    def from_config(cls, config_dict):
        """Create device from configuration dictionary"""
        return cls(
            name=config_dict['name'],
            device_type=config_dict['type'],
            location=config_dict.get('location', 'Unknown')
        )

    @classmethod
    def get_device_count(cls):
        """Get total number of devices created"""
        return cls.device_count

    @staticmethod
    def validate_location(location):
        """Validate location string"""
        valid_locations = ['Living Room', 'Kitchen', 'Bedroom', 'Bathroom', 'Garage']
        return location in valid_locations

    def turn_on(self):
        """Turn device on"""
        if not self._is_on:
            self._is_on = True
            self._log_event("turned_on", f"{self._name} turned on")
        return f"{self._name} is now on"

    def turn_off(self):
        """Turn device off"""
        if self._is_on:
            self._is_on = False
            self._log_event("turned_off", f"{self._name} turned off")
        return f"{self._name} is now off"

    def _log_event(self, event_type, description):
        """Log device event"""
        self._events.append(DeviceEvent(
            event_type=event_type,
            description=description
        ))

    def __str__(self):
        status = "ON" if self._is_on else "OFF"
        return f"{self._device_type}: {self._name} ({status}) - {self._location}"

    def __eq__(self, other):
        if not isinstance(other, SmartDevice):
            return False
        return (self._name == other._name and
                self._device_type == other._device_type)

    def __len__(self):
        """Return number of events"""
        return len(self._events)

    def __getitem__(self, index):
        """Get event by index"""
        return self._events[index]

# Usage
# Create device normally
light = SmartDevice("Living Room Light", "Light", "Living Room")

# Create from config using classmethod
config = {"name": "Kitchen Thermostat", "type": "Thermostat", "location": "Kitchen"}
thermostat = SmartDevice.from_config(config)

print(light)                    # Light: Living Room Light (OFF) - Living Room
print(thermostat)               # Thermostat: Kitchen Thermostat (OFF) - Kitchen

print(light.turn_on())          # Living Room Light is now on
print(len(light))               # 1 (one event logged)

print(SmartDevice.get_device_count())  # 2
print(SmartDevice.validate_location("Living Room"))  # True
```

---

## 💡 Best Practices

### Magic Methods
- **Only implement what you need**: Don't add magic methods "just because"
- **Follow conventions**: `__str__` for users, `__repr__` for developers
- **Handle edge cases**: Consider what happens with invalid inputs
- **Document behavior**: Explain what your custom operators do

### Properties
- **Validate in setters**: Use property setters for input validation
- **Keep it simple**: Don't make properties too complex
- **Consider performance**: Computed properties are calculated each time
- **Use for encapsulation**: Hide internal representation

### Class vs Static Methods
- **Class methods**: When you need to work with class-level data
- **Static methods**: For utility functions that don't need `self` or `cls`
- **Alternative constructors**: Use classmethods for different creation patterns

### Dataclasses
- **Use for data containers**: When you mainly store data
- **Combine with regular classes**: Use dataclasses where appropriate
- **Consider immutability**: Use `frozen=True` for immutable objects
- **Custom methods**: Add business logic methods as needed

### General Advanced OOP
- **Single Responsibility**: Each class should have one clear purpose
- **Composition over Inheritance**: Favor composition when possible
- **Interface Segregation**: Keep interfaces focused and minimal
- **DRY Principle**: Don't Repeat Yourself

---

## 🏁 Summary

Advanced OOP concepts provide powerful tools for creating sophisticated, maintainable classes:

- **Magic methods** customize object behavior and operator overloading
- **Property decorators** enable controlled attribute access and validation
- **Class and static methods** provide alternative ways to work with classes
- **Dataclasses** simplify data container creation with automatic method generation
- **Advanced combinations** create robust, flexible class designs

These concepts, when used appropriately, enable you to create more Pythonic, maintainable, and powerful object-oriented code. In the workshop, you'll apply these concepts by building a banking system!
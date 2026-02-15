# OOP Cheatsheet - Python Object-Oriented Programming

**Topic 4**: Object-Oriented Programming
**Quick Reference**: Classes, objects, methods, inheritance

---

## 🏗️ Class Definition

### Basic Class Syntax

```python
class ClassName:
    """Docstring describing the class."""
    pass

# Class with constructor
class Person:
    """A person class."""
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

### Class Naming Conventions

```python
# PascalCase for class names
class UserAccount:    # ✓ Correct
class ProductManager: # ✓ Correct

class user_account:   # ✗ Wrong (snake_case)
class productManager: # ✗ Wrong (camelCase)
```

---

## 🏭 Object Creation & Constructor

### Constructor (`__init__`)

```python
class Car:
    def __init__(self, make, model, year=2024):
        self.make = make      # Instance variable
        self.model = model    # Instance variable
        self.year = year      # Instance variable
        self.mileage = 0     # Default value

# Creating objects
car1 = Car("Toyota", "Camry")        # year defaults to 2024
car2 = Car("Honda", "Civic", 2023)   # year explicitly set
```

### Constructor Parameters

```python
class BankAccount:
    def __init__(self, account_number, owner_name, initial_balance=0):
        self.account_number = account_number
        self.owner_name = owner_name
        self.balance = initial_balance
        self.transactions = []  # Empty list

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.transactions.append(f"Deposit: +${amount}")
```

---

## 📊 Instance Variables & Methods

### Instance Variables

```python
class Student:
    def __init__(self, student_id, name):
        self.student_id = student_id  # Public
        self.name = name              # Public
        self._gpa = 0.0              # Protected (convention)
        self.__courses = []          # Private (name mangling)

# Usage
student = Student("S001", "Alice")
print(student.name)           # Alice (public)
print(student._gpa)          # 0.0 (protected, but accessible)
# print(student.__courses)    # AttributeError
print(student._Student__courses)  # [] (name mangled)
```

### Instance Methods

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):           # Instance method
        return self.width * self.height

    def perimeter(self):      # Instance method
        return 2 * (self.width + self.height)

    def is_square(self):      # Instance method
        return self.width == self.height

rect = Rectangle(5, 10)
print(rect.area())        # 50
print(rect.perimeter())   # 30
print(rect.is_square())   # False
```

### The `self` Parameter

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):      # self is required
        self.count += 1

    def get_count(self):      # self is required
        return self.count

counter = Counter()
counter.increment()       # self passed automatically
print(counter.get_count()) # 2
```

---

## 🏢 Class Variables & Methods

### Class Variables

```python
class Employee:
    # Class variables (shared by all instances)
    company_name = "Tech Corp"
    total_employees = 0
    default_salary = 50000

    def __init__(self, name, salary=None):
        self.name = name
        self.salary = salary or Employee.default_salary
        Employee.total_employees += 1  # Increment class variable

# Usage
emp1 = Employee("Alice", 60000)
emp2 = Employee("Bob")  # Uses default salary

print(Employee.total_employees)   # 2
print(emp1.company_name)          # Tech Corp
print(Employee.company_name)      # Tech Corp (same value)

# Changing class variable affects all instances
Employee.company_name = "Super Tech"
print(emp1.company_name)          # Super Tech
```

### Class Methods

```python
class MathUtils:
    PI = 3.14159

    @classmethod
    def circle_area(cls, radius):
        return cls.PI * radius * radius

    @classmethod
    def from_diameter(cls, diameter):
        radius = diameter / 2
        return cls.circle_area(radius)

# Usage
area1 = MathUtils.circle_area(5)      # 78.53975
area2 = MathUtils.from_diameter(10)   # 78.53975

print(MathUtils.PI)  # 3.14159
```

### Static Methods

```python
class StringUtils:
    @staticmethod
    def is_palindrome(text):
        clean_text = ''.join(c.lower() for c in text if c.isalnum())
        return clean_text == clean_text[::-1]

    @staticmethod
    def word_count(text):
        return len(text.split())

# Usage
print(StringUtils.is_palindrome("A man, a plan, a canal: Panama"))  # True
print(StringUtils.word_count("Hello world from Python"))            # 4
```

---

## 🔒 Encapsulation

### Private Attributes (Name Mangling)

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance    # Private attribute
        self.__transactions = []    # Private attribute

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            self.__transactions.append(f"Deposit: ${amount}")

    def get_balance(self):
        return self.__balance      # Public access method

    def get_transactions(self):
        return self.__transactions.copy()  # Return copy to prevent modification

account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())     # 1500

# Direct access (not recommended)
print(account._BankAccount__balance)  # 1500 (name mangled)
```

### Properties (Getters/Setters)

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):
        """Get temperature in Celsius."""
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        """Set temperature in Celsius."""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value

    @property
    def fahrenheit(self):
        """Get temperature in Fahrenheit."""
        return (self._celsius * 9/5) + 32

    @fahrenheit.setter
    def fahrenheit(self, value):
        """Set temperature in Fahrenheit."""
        self._celsius = (value - 32) * 5/9

temp = Temperature(25)
print(temp.celsius)      # 25
print(temp.fahrenheit)   # 77.0

temp.fahrenheit = 100
print(temp.celsius)      # 37.777...
```

---

## 🔄 Object Lifecycle

### Constructor and Destructor

```python
class FileHandler:
    def __init__(self, filename):
        self.filename = filename
        self.file = None
        print(f"FileHandler created for {filename}")

    def __del__(self):
        if self.file:
            self.file.close()
        print(f"FileHandler destroyed for {self.filename}")

# Usage
handler = FileHandler("test.txt")  # Constructor called
del handler                        # Destructor called
```

### String Representation

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def __str__(self):
        return f"'{self.title}' by {self.author}"

    def __repr__(self):
        return f"Book(title='{self.title}', author='{self.author}')"

book = Book("1984", "George Orwell")
print(book)        # '1984' by George Orwell (__str__)
print(repr(book))  # Book(title='1984', author='George Orwell') (__repr__)
```

### Comparison Methods

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __eq__(self, other):
        if isinstance(other, Person):
            return self.name == other.name and self.age == other.age
        return False

    def __lt__(self, other):
        if isinstance(other, Person):
            return self.age < other.age
        return NotImplemented

person1 = Person("Alice", 25)
person2 = Person("Bob", 30)
person3 = Person("Alice", 25)

print(person1 == person3)  # True
print(person1 == person2)  # False
print(person1 < person2)   # True (25 < 30)
```

---

## 📋 Method Types Summary

| Method Type | Decorator | `self` Parameter | Access |
|-------------|-----------|------------------|---------|
| Instance | None | Yes | `obj.method()` |
| Class | `@classmethod` | No (`cls`) | `Class.method()` |
| Static | `@staticmethod` | No | `Class.method()` |

```python
class Example:
    def instance_method(self):      # Instance method
        return f"Instance: {self}"

    @classmethod
    def class_method(cls):          # Class method
        return f"Class: {cls}"

    @staticmethod
    def static_method():           # Static method
        return "Static method"

obj = Example()
print(obj.instance_method())      # Instance: <__main__.Example object...>
print(Example.class_method())     # Class: <class '__main__.Example'>
print(Example.static_method())    # Static method
```

---

## 🐛 Common Mistakes

### Forgetting `self`

```python
class BadExample:
    def __init__(self):
        self.value = 42

    def get_value(self):  # Missing self!
        return self.value

obj = BadExample()
# obj.get_value()  # TypeError: get_value() takes 0 positional arguments
```

### Incorrect Instance Variable Access

```python
class Counter:
    count = 0  # Class variable (shared!)

    def __init__(self):
        self.count = 0  # This shadows the class variable

    def increment(self):
        self.count += 1  # Only affects this instance

# Better approach
class Counter:
    def __init__(self):
        self.count = 0  # Instance variable

    def increment(self):
        self.count += 1
```

### Mutable Default Arguments

```python
class BadList:
    def __init__(self, items=[]):  # Mutable default!
        self.items = items

    def add_item(self, item):
        self.items.append(item)

list1 = BadList()
list2 = BadList()

list1.add_item("A")
print(list2.items)  # ['A'] - Shared list!

# Correct approach
class GoodList:
    def __init__(self, items=None):
        self.items = items or []  # None is immutable

    def add_item(self, item):
        self.items.append(item)
```

---

## 🎯 Quick Examples

### Complete Class Example

```python
class Student:
    """A student in a university system."""

    # Class variables
    total_students = 0
    university_name = "State University"

    def __init__(self, student_id, name, major):
        # Instance variables
        self.student_id = student_id
        self.name = name
        self.major = major
        self.gpa = 0.0
        self.__courses = []  # Private

        # Increment class variable
        Student.total_students += 1

    def enroll(self, course):
        """Enroll in a course."""
        if course not in self.__courses:
            self.__courses.append(course)

    def get_courses(self):
        """Get enrolled courses (return copy)."""
        return self.__courses.copy()

    @property
    def course_count(self):
        """Get number of enrolled courses."""
        return len(self.__courses)

    @classmethod
    def get_total_students(cls):
        """Get total number of students."""
        return cls.total_students

    def __str__(self):
        return f"{self.name} ({self.student_id}) - {self.major}"

# Usage
student = Student("S001", "Alice Johnson", "Computer Science")
student.enroll("CS101")
student.enroll("MATH201")

print(student)                    # Alice Johnson (S001) - Computer Science
print(student.course_count)       # 2
print(Student.get_total_students()) # 1
```

---

## 📚 Key Takeaways

1. **Classes are Blueprints**: Define structure and behavior for objects
2. **Objects are Instances**: Each object has its own data but shares methods
3. **`self` is Essential**: Always include `self` in instance method definitions
4. **Encapsulation**: Use private attributes and public methods to control access
5. **Class vs Instance**: Class variables are shared, instance variables are unique
6. **Properties**: Use `@property` for controlled attribute access
7. **Special Methods**: Implement `__str__`, `__eq__`, etc. for better object behavior

---

**Cheatsheet Version**: 1.0
**Last Updated**: February 2026
**Pages**: 1
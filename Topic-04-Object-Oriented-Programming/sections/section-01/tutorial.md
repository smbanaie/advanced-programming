# Tutorial 1: OOP Fundamentals

**Section**: 1 - OOP Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-3 (Basic Python, Data Structures)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Define Classes**: Create class blueprints with proper syntax
2. **Create Objects**: Instantiate objects and understand their lifecycle
3. **Implement Methods**: Write instance methods with correct `self` usage
4. **Manage Attributes**: Work with instance and class variables
5. **Apply Encapsulation**: Control access to object data
6. **Design Basic OOP Solutions**: Structure simple object-oriented programs

---

## 📚 Table of Contents

1. [What is Object-Oriented Programming?](#what-is-object-oriented-programming)
2. [Classes and Objects](#classes-and-objects)
3. [The Constructor (`__init__`)](#the-constructor-__init__)
4. [Instance Variables and Methods](#instance-variables-and-methods)
5. [The `self` Parameter](#the-self-parameter)
6. [Class Variables](#class-variables)
7. [Basic Encapsulation](#basic-encapsulation)
8. [Object Lifecycle](#object-lifecycle)
9. [Best Practices](#best-practices)

---

## 🤔 What is Object-Oriented Programming?

Object-Oriented Programming (OOP) is a programming paradigm that organizes code around **objects** rather than functions and logic. Objects are instances of **classes**, which serve as blueprints.

### Key OOP Concepts
- **Class**: A blueprint or template for creating objects
- **Object**: An instance of a class (also called an instance)
- **Attributes**: Data associated with objects (variables)
- **Methods**: Functions associated with objects
- **Encapsulation**: Bundling data and methods that operate on that data

### Why OOP?
```python
# Procedural approach (what we've done so far)
def calculate_circle_area(radius):
    return 3.14159 * radius * radius

def calculate_circle_perimeter(radius):
    return 2 * 3.14159 * radius

area = calculate_circle_area(5)
perimeter = calculate_circle_perimeter(5)

# OOP approach (what we'll learn)
class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius * self.radius

    def perimeter(self):
        return 2 * 3.14159 * self.radius

circle = Circle(5)
area = circle.area()
perimeter = circle.perimeter()
```

**Benefits:**
- **Organization**: Related data and functions are grouped together
- **Reusability**: Objects can be reused in different contexts
- **Maintainability**: Changes to object behavior are localized
- **Modeling**: Objects can represent real-world entities

---

## 🏗️ Classes and Objects

### Defining a Class

```python
# Basic class definition
class Person:
    """A simple Person class representing a human being."""
    pass

# Class with docstring
class Car:
    """
    A class representing a car with basic attributes and methods.

    This class demonstrates fundamental OOP concepts including
    attributes, methods, and object instantiation.
    """
    pass
```

### Creating Objects (Instantiation)

```python
# Create objects from classes
person1 = Person()
person2 = Person()

car1 = Car()
car2 = Car()

print(type(person1))  # <class '__main__.Person'>
print(type(car1))     # <class '__main__.Car'>

# Each object is unique
print(person1 is person2)  # False
print(car1 is car2)        # False
```

### Class Naming Conventions

```python
# Good class names (PascalCase)
class UserAccount:
    pass

class ProductCatalog:
    pass

class DatabaseConnection:
    pass

# Avoid these (snake_case should be for variables/functions)
class user_account:    # Wrong!
    pass

class product_catalog: # Wrong!
    pass
```

---

## 🏭 The Constructor (`__init__`)

The `__init__` method is a special method called the **constructor**. It's automatically called when you create a new object.

### Basic Constructor

```python
class Person:
    def __init__(self):
        print("A new Person object is being created!")

# Creating objects calls __init__
person1 = Person()  # Output: A new Person object is being created!
person2 = Person()  # Output: A new Person object is being created!
```

### Constructor with Parameters

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        print(f"Created person: {self.name}, age {self.age}")

# Creating objects with parameters
person1 = Person("Alice", 25)    # Created person: Alice, age 25
person2 = Person("Bob", 30)      # Created person: Bob, age 30
```

### Default Parameters

```python
class Person:
    def __init__(self, name, age=18):
        self.name = name
        self.age = age

# Mix of required and optional parameters
person1 = Person("Charlie")        # age defaults to 18
person2 = Person("Diana", 22)      # age explicitly set to 22
```

---

## 📊 Instance Variables and Methods

### Instance Variables

Instance variables are unique to each object. They store data specific to that particular instance.

```python
class Student:
    def __init__(self, name, student_id, major):
        # Instance variables
        self.name = name
        self.student_id = student_id
        self.major = major
        self.gpa = 0.0  # Default value
        self.courses = []  # Default empty list

# Create student objects
student1 = Student("Alice", "S001", "Computer Science")
student2 = Student("Bob", "S002", "Mathematics")

# Each object has its own instance variables
print(student1.name)        # Alice
print(student2.name)        # Bob

print(student1.courses)     # []
print(student2.courses)     # []

# Modify instance variables independently
student1.gpa = 3.8
student2.gpa = 3.5

student1.courses.append("Python Programming")
student2.courses.append("Calculus I")

print(student1.courses)     # ['Python Programming']
print(student2.courses)     # ['Calculus I']
```

### Instance Methods

Instance methods are functions that operate on the object's data. They always take `self` as the first parameter.

```python
class BankAccount:
    def __init__(self, account_number, owner_name, initial_balance=0):
        self.account_number = account_number
        self.owner_name = owner_name
        self.balance = initial_balance

    def deposit(self, amount):
        """Add money to the account."""
        if amount > 0:
            self.balance += amount
            print(f"Deposited ${amount:.2f}. New balance: ${self.balance:.2f}")
        else:
            print("Deposit amount must be positive.")

    def withdraw(self, amount):
        """Remove money from the account."""
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            print(f"Withdrew ${amount:.2f}. New balance: ${self.balance:.2f}")
        elif amount > self.balance:
            print("Insufficient funds.")
        else:
            print("Withdrawal amount must be positive.")

    def get_balance(self):
        """Return the current balance."""
        return self.balance

    def display_info(self):
        """Display account information."""
        print(f"Account: {self.account_number}")
        print(f"Owner: {self.owner_name}")
        print(f"Balance: ${self.balance:.2f}")

# Using the BankAccount class
account = BankAccount("123456789", "John Doe", 1000)

account.display_info()
# Account: 123456789
# Owner: John Doe
# Balance: $1000.00

account.deposit(500)
# Deposited $500.00. New balance: $1500.00

account.withdraw(200)
# Withdrew $200.00. New balance: $1300.00

print(f"Current balance: ${account.get_balance():.2f}")
# Current balance: $1300.00
```

---

## 🎯 The `self` Parameter

The `self` parameter refers to the current object instance. It's automatically passed when you call a method on an object.

### Why `self` is Required

```python
class Counter:
    def __init__(self):
        self.count = 0

    def increment(self):
        self.count += 1

    def get_count(self):
        return self.count

counter = Counter()
counter.increment()
counter.increment()
print(counter.get_count())  # 2

# Behind the scenes, Python does this:
# Counter.increment(counter)  # self is passed automatically
```

### Common `self` Mistakes

```python
class BadExample:
    def __init__(self):
        self.value = 42

    def get_value(self):  # Missing self!
        return self.value

# This will fail
obj = BadExample()
# obj.get_value()  # TypeError: get_value() takes 0 positional arguments but 1 was given
```

### `self` in Different Contexts

```python
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hello, I'm {self.name}"

    def introduce(self, other_person):
        return f"{self.name} meets {other_person.name}"

person1 = Person("Alice")
person2 = Person("Bob")

print(person1.greet())              # Hello, I'm Alice
print(person2.greet())              # Hello, I'm Bob

print(person1.introduce(person2))   # Alice meets Bob
print(person2.introduce(person1))   # Bob meets Alice
```

---

## 🏢 Class Variables

Class variables are shared among all instances of a class. They belong to the class itself, not individual objects.

### Class Variables vs Instance Variables

```python
class Employee:
    # Class variable - shared by all employees
    company_name = "Tech Corp"
    total_employees = 0

    def __init__(self, name, salary):
        # Instance variables - unique to each employee
        self.name = name
        self.salary = salary

        # Increment class variable when new employee is created
        Employee.total_employees += 1

    def get_info(self):
        return f"{self.name} works at {Employee.company_name} with salary ${self.salary}"

# Create employees
emp1 = Employee("Alice", 75000)
emp2 = Employee("Bob", 80000)

print(Employee.total_employees)      # 2 (class variable)
print(emp1.total_employees)         # 2 (accessed through instance)
print(emp2.total_employees)         # 2 (accessed through instance)

print(Employee.company_name)        # Tech Corp
print(emp1.company_name)            # Tech Corp

# Changing class variable affects all instances
Employee.company_name = "Super Tech Corp"
print(emp1.get_info())              # Alice works at Super Tech Corp with salary $75000
print(emp2.get_info())              # Bob works at Super Tech Corp with salary $80000

# Instance variables are independent
print(emp1.salary)                  # 75000
print(emp2.salary)                  # 80000
```

### Use Cases for Class Variables

```python
class Circle:
    # Class constants
    PI = 3.14159

    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return Circle.PI * self.radius * self.radius

    def circumference(self):
        return 2 * Circle.PI * self.radius

# All circles share the same PI value
circle1 = Circle(5)
circle2 = Circle(10)

print(circle1.area())      # 78.53975
print(circle2.area())      # 314.159

# Class variables can be accessed without creating instances
print(Circle.PI)          # 3.14159
```

---

## 🔒 Basic Encapsulation

Encapsulation is about bundling data and methods that operate on that data, and controlling access to the internal state.

### Public vs Private Attributes

```python
class BankAccount:
    def __init__(self, account_number, initial_balance):
        self.account_number = account_number  # Public attribute
        self.__balance = initial_balance      # Private attribute (name mangling)
        self.__transaction_history = []       # Private attribute

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            self.__transaction_history.append(f"Deposit: +${amount}")
        else:
            raise ValueError("Deposit amount must be positive")

    def withdraw(self, amount):
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
            self.__transaction_history.append(f"Withdrawal: -${amount}")
        else:
            raise ValueError("Invalid withdrawal amount")

    def get_balance(self):
        """Public method to access private balance."""
        return self.__balance

    def get_transaction_history(self):
        """Public method to access private transaction history."""
        return self.__transaction_history.copy()  # Return copy to prevent modification

# Usage
account = BankAccount("12345", 1000)

# Public attributes can be accessed directly
print(account.account_number)    # 12345

# Private attributes are name-mangled (not truly private, but discouraged)
# print(account.__balance)       # AttributeError
print(account._BankAccount__balance)  # 1000 (not recommended!)

# Use public methods instead
print(account.get_balance())     # 1000

account.deposit(500)
account.withdraw(200)

print(account.get_balance())     # 1300
print(account.get_transaction_history())
# ['Deposit: +$500', 'Withdrawal: -$200']
```

### Naming Conventions for Privacy

```python
class Example:
    def __init__(self):
        self.public_attr = "accessible"           # Public
        self._protected_attr = "internal use"     # Protected (convention)
        self.__private_attr = "name mangled"      # Private (name mangling)

obj = Example()

print(obj.public_attr)           # accessible
print(obj._protected_attr)       # internal use (but accessible)
# print(obj.__private_attr)      # AttributeError
print(obj._Example__private_attr) # name mangled (not recommended)
```

---

## 🔄 Object Lifecycle

### Object Creation and Destruction

```python
class LifecycleDemo:
    def __init__(self, name):
        self.name = name
        print(f"Object {self.name} created")

    def __del__(self):
        print(f"Object {self.name} destroyed")

# Object creation
obj1 = LifecycleDemo("A")    # Object A created

# Object goes out of scope
del obj1                       # Object A destroyed

# Multiple objects
obj2 = LifecycleDemo("B")    # Object B created
obj3 = LifecycleDemo("C")    # Object C created

# Objects destroyed when program ends
# Object B destroyed
# Object C destroyed
```

### Reference Counting

```python
class ReferenceDemo:
    def __init__(self, name):
        self.name = name
        print(f"Created {self.name}")

    def __del__(self):
        print(f"Destroyed {self.name}")

# Reference counting
obj = ReferenceDemo("Test")    # Created Test

ref1 = obj                      # obj has 2 references
ref2 = obj                      # obj has 3 references

del ref1                        # obj has 2 references
del ref2                        # obj has 1 reference
del obj                         # obj has 0 references, destroyed
```

---

## 🎯 Best Practices

### Class Design Principles

```python
# Good: Single Responsibility Principle
class EmailSender:
    def __init__(self, smtp_server):
        self.smtp_server = smtp_server

    def send_email(self, to_address, subject, body):
        # Email sending logic
        pass

class UserManager:
    def __init__(self, database):
        self.database = database

    def create_user(self, username, email):
        # User creation logic
        pass

    def authenticate_user(self, username, password):
        # Authentication logic
        pass

# Bad: Multiple responsibilities in one class
class UserService:
    def __init__(self):
        self.smtp_server = "smtp.example.com"
        self.database = "users.db"

    def create_user(self, username, email, password):
        # User creation
        pass

    def send_welcome_email(self, email):
        # Email sending
        pass

    def authenticate_user(self, username, password):
        # Authentication
        pass
```

### Method Naming and Documentation

```python
class Calculator:
    """
    A simple calculator class demonstrating good OOP practices.
    """

    def __init__(self):
        """Initialize calculator with zero value."""
        self._result = 0.0

    def add(self, value):
        """
        Add a value to the current result.

        Args:
            value (float): Value to add

        Returns:
            float: New result after addition
        """
        self._result += value
        return self._result

    def subtract(self, value):
        """
        Subtract a value from the current result.

        Args:
            value (float): Value to subtract

        Returns:
            float: New result after subtraction
        """
        self._result -= value
        return self._result

    def get_result(self):
        """
        Get the current result.

        Returns:
            float: Current calculator result
        """
        return self._result

    def reset(self):
        """Reset the calculator to zero."""
        self._result = 0.0
```

### Error Handling in Classes

```python
class SafeCalculator:
    """Calculator with proper error handling."""

    def __init__(self):
        self._result = 0.0

    def divide(self, dividend, divisor):
        """
        Divide dividend by divisor.

        Args:
            dividend (float): Number to be divided
            divisor (float): Number to divide by

        Returns:
            float: Result of division

        Raises:
            ValueError: If divisor is zero
            TypeError: If inputs are not numbers
        """
        try:
            if divisor == 0:
                raise ValueError("Cannot divide by zero")
            return dividend / divisor
        except TypeError:
            raise TypeError("Both dividend and divisor must be numbers")

    def safe_operation(self, operation, *args):
        """
        Perform operation with error handling.

        Args:
            operation (str): Operation name ('add', 'subtract', etc.)
            *args: Arguments for the operation

        Returns:
            float: Result of operation
        """
        try:
            if operation == 'add':
                return self.add(args[0])
            elif operation == 'divide':
                return self.divide(args[0], args[1])
            else:
                raise ValueError(f"Unknown operation: {operation}")
        except Exception as e:
            print(f"Error in {operation}: {e}")
            return self._result  # Return current result on error
```

---

## 🎯 Key Takeaways

1. **Classes are Blueprints**: Classes define the structure and behavior of objects
2. **Objects are Instances**: Each object created from a class is unique
3. **Self is Essential**: The `self` parameter refers to the current object instance
4. **Encapsulation**: Bundle data and methods, control access through methods
5. **Instance vs Class Variables**: Instance variables are unique per object, class variables are shared
6. **Methods Operate on Data**: Instance methods work with the object's attributes

---

## 🔗 Further Reading

- [Python Classes Documentation](https://docs.python.org/3/tutorial/classes.html)
- [OOP Principles](https://realpython.com/python3-object-oriented-programming/)
- [Class vs Instance Variables](https://docs.python.org/3/tutorial/classes.html#class-and-instance-variables)

---

## 📝 Practice Exercises

1. **Create a Book Class**: Define a Book class with title, author, and ISBN. Add methods to display info and check if two books are the same.

2. **Build a Rectangle Class**: Create a Rectangle class with width and height. Add methods to calculate area, perimeter, and check if it's a square.

3. **Design a Student Class**: Implement a Student class with name, grades, and methods to add grades, calculate GPA, and display student info.

4. **Create a Shopping Cart**: Build a ShoppingCart class that can add items, remove items, calculate total price, and apply discounts.

5. **Implement a Library System**: Create Book and Library classes where Library can manage multiple books, search by title/author, and track borrowed books.

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
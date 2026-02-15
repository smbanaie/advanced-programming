# Tutorial 2: Inheritance & Polymorphism

**Section**: 2 - Inheritance & Polymorphism (90 min)
**Level**: Intermediate
**Prerequisites**: Section 1 (OOP Fundamentals)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Implement Inheritance**: Create child classes that inherit from parent classes
2. **Override Methods**: Customize inherited behavior in child classes
3. **Use `super()`**: Access parent class methods and constructors
4. **Apply Polymorphism**: Design systems that work with multiple object types
5. **Design Class Hierarchies**: Structure related classes effectively
6. **Use Abstract Base Classes**: Define common interfaces for related classes

---

## 📚 Table of Contents

1. [What is Inheritance?](#what-is-inheritance)
2. [Basic Inheritance Syntax](#basic-inheritance-syntax)
3. [Method Overriding](#method-overriding)
4. [The `super()` Function](#the-super-function)
5. [Polymorphism](#polymorphism)
6. [Abstract Base Classes](#abstract-base-classes)
7. [Multiple Inheritance](#multiple-inheritance)
8. [Best Practices](#best-practices)

---

## 🏗️ What is Inheritance?

Inheritance is a fundamental OOP concept that allows one class to inherit properties and methods from another class. The class that inherits is called the **child class** or **subclass**, and the class being inherited from is called the **parent class** or **superclass**.

### Why Inheritance?
- **Code Reuse**: Avoid duplicating code across similar classes
- **Hierarchy**: Create logical relationships between classes
- **Polymorphism**: Treat different objects uniformly
- **Extensibility**: Add features without modifying existing code

### Real-World Analogy
Think of inheritance like family relationships:
- Parent class = "Vehicle" (general concept)
- Child classes = "Car", "Truck", "Motorcycle" (specific types)

---

## 📝 Basic Inheritance Syntax

### Simple Inheritance
```python
# Parent class
class Vehicle:
    def __init__(self, make, model):
        self.make = make
        self.model = model

    def start(self):
        return f"The {self.make} {self.model} is starting..."

    def stop(self):
        return f"The {self.make} {self.model} is stopping..."

# Child class inheriting from Vehicle
class Car(Vehicle):  # Car inherits from Vehicle
    def __init__(self, make, model, num_doors):
        super().__init__(make, model)  # Call parent constructor
        self.num_doors = num_doors

    def honk(self):
        return "Beep beep!"

# Usage
my_car = Car("Toyota", "Camry", 4)
print(my_car.start())  # Inherited method
print(my_car.honk())   # Child-specific method
```

### Key Points:
- Child class automatically gets all parent methods and attributes
- Child can add new methods and attributes
- Child can override inherited methods

---

## 🔄 Method Overriding

Method overriding allows a child class to provide a specific implementation of a method that's already defined in the parent class.

### Basic Overriding
```python
class Animal:
    def speak(self):
        return "Some generic animal sound"

class Dog(Animal):
    def speak(self):  # Override the speak method
        return "Woof!"

class Cat(Animal):
    def speak(self):  # Override the speak method
        return "Meow!"

# Usage
animals = [Dog(), Cat(), Animal()]

for animal in animals:
    print(animal.speak())  # Each calls its own version

# Output:
# Woof!
# Meow!
# Some generic animal sound
```

### Extending Parent Behavior
```python
class Vehicle:
    def __init__(self, make, model):
        self.make = make
        self.model = model

class ElectricCar(Vehicle):
    def __init__(self, make, model, battery_range):
        super().__init__(make, model)  # Call parent constructor
        self.battery_range = battery_range

    def start(self):
        parent_message = super().start()  # Get parent method result
        return f"{parent_message} (Electric mode activated)"

# Usage
tesla = ElectricCar("Tesla", "Model 3", 300)
print(tesla.start())
# Output: "The Tesla Model 3 is starting... (Electric mode activated)"
```

---

## 🔗 The `super()` Function

The `super()` function provides access to methods and properties of a parent class. It's essential for proper inheritance.

### Constructor Inheritance
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Employee(Person):
    def __init__(self, name, age, employee_id, salary):
        super().__init__(name, age)  # Initialize parent attributes
        self.employee_id = employee_id
        self.salary = salary

# Usage
emp = Employee("Alice", 30, "E001", 75000)
print(f"{emp.name} (ID: {emp.employee_id}) earns ${emp.salary}")
```

### Method Extension
```python
class Shape:
    def area(self):
        return 0

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2
```

---

## 🎭 Polymorphism

Polymorphism means "many forms" and allows objects of different classes to be treated as objects of a common parent class. It's a key benefit of inheritance.

### Polymorphism in Action
```python
# Using the Shape classes from above

def print_areas(shapes):
    for shape in shapes:
        print(f"Area: {shape.area()}")

shapes = [
    Rectangle(5, 10),  # Area: 50
    Circle(7),         # Area: ~153.94
    Rectangle(3, 4),   # Area: 12
]

print_areas(shapes)
```

### Duck Typing
Python uses "duck typing" - if it walks like a duck and quacks like a duck, it's a duck!

```python
class Duck:
    def speak(self):
        return "Quack!"

class Dog:
    def speak(self):
        return "Woof!"

def make_sound(animal):
    return animal.speak()  # Works for any object with speak() method

animals = [Duck(), Dog()]

for animal in animals:
    print(make_sound(animal))
```

---

## 🎯 Abstract Base Classes (ABC)

Abstract Base Classes define interfaces that must be implemented by child classes. They ensure that certain methods exist in all subclasses.

```python
from abc import ABC, abstractmethod

class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount):
        """Process a payment of the given amount"""
        pass

    @abstractmethod
    def refund_payment(self, transaction_id):
        """Refund a payment by transaction ID"""
        pass

class CreditCardProcessor(PaymentProcessor):
    def process_payment(self, amount):
        return f"Processing ${amount} via credit card"

    def refund_payment(self, transaction_id):
        return f"Refunding transaction {transaction_id} to credit card"

class PayPalProcessor(PaymentProcessor):
    def process_payment(self, amount):
        return f"Processing ${amount} via PayPal"

    def refund_payment(self, transaction_id):
        return f"Refunding transaction {transaction_id} via PayPal"

# Usage
processors = [CreditCardProcessor(), PayPalProcessor()]

for processor in processors:
    print(processor.process_payment(100))
```

### Key Benefits of ABC:
- **Interface Definition**: Ensures all subclasses implement required methods
- **Type Safety**: Runtime checking of method availability
- **Design Clarity**: Clear contract between classes

---

## 🔀 Multiple Inheritance

Python supports multiple inheritance, where a class can inherit from multiple parent classes.

```python
class Flyable:
    def fly(self):
        return "Flying through the air!"

class Swimmable:
    def swim(self):
        return "Swimming in the water!"

class Duck(Flyable, Swimmable):  # Multiple inheritance
    def quack(self):
        return "Quack!"

# Usage
duck = Duck()
print(duck.fly())   # From Flyable
print(duck.swim())  # From Swimmable
print(duck.quack()) # From Duck
```

### Method Resolution Order (MRO)
```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):  # Multiple inheritance
    pass

d = D()
print(d.method())  # "B" - follows MRO: D -> B -> C -> A
print(D.__mro__)   # (<class '__main__.D'>, <class '__main__.B'>,
                   #  <class '__main__.C'>, <class '__main__.A'>,
                   #  <class 'object'>)
```

---

## 💡 Best Practices

### 1. **Composition over Inheritance**
Sometimes composition is better than inheritance:
```python
# Inheritance (tight coupling)
class Car(Vehicle):
    pass

# Composition (loose coupling)
class Car:
    def __init__(self):
        self.engine = Engine()
        self.wheels = [Wheel() for _ in range(4)]
```

### 2. **Liskov Substitution Principle**
Child classes should be substitutable for parent classes:
```python
# Good: Rectangle can be used anywhere Shape is expected
def calculate_total_area(shapes):
    return sum(shape.area() for shape in shapes)

shapes = [Rectangle(5, 10), Circle(7)]
total = calculate_total_area(shapes)  # Works for all shapes
```

### 3. **Avoid Deep Inheritance Hierarchies**
Keep inheritance chains shallow (max 3-4 levels) to avoid complexity.

### 4. **Use Abstract Base Classes for Interfaces**
```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def connect(self):
        pass

    @abstractmethod
    def query(self, sql):
        pass
```

### 5. **Document Inheritance Relationships**
Use docstrings to explain the purpose of inheritance:
```python
class ElectricCar(Car):
    """ElectricCar extends Car with electric-specific features.

    This class inherits all Car functionality while adding
    battery management and electric motor controls.
    """
```

---

## 🏁 Summary

Inheritance and polymorphism are powerful OOP concepts that enable:

- **Code Reuse**: Child classes inherit parent functionality
- **Extensibility**: Add features without modifying existing code
- **Polymorphism**: Treat different objects through common interfaces
- **Type Safety**: Abstract base classes ensure method availability
- **Flexible Design**: Create hierarchies that model real-world relationships

Key takeaways:
- Use `class Child(Parent):` for inheritance
- Override methods to customize behavior
- Use `super()` to access parent methods
- Leverage polymorphism for flexible code
- Consider ABCs for interface definitions
- Prefer composition when inheritance feels forced

In the next workshop, you'll apply these concepts by building a shape hierarchy system!
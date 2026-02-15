# Workshop 2: Shape Hierarchy System

**Section**: 2 - Inheritance & Polymorphism (90 min)
**Level**: Intermediate
**Prerequisites**: Section 2 Tutorial (Inheritance & Polymorphism)

---

## 📋 Workshop Overview

In this workshop, you'll build a comprehensive shape hierarchy system that demonstrates inheritance, polymorphism, and abstract base classes. You'll create a system where different shapes can be treated uniformly while maintaining their unique characteristics.

**Estimated Time**: 40-50 minutes
**Learning Focus**: Practical application of inheritance and polymorphism

---

## 🎯 Learning Objectives

By completing this workshop, you will:

1. **Implement Inheritance**: Create a shape class hierarchy
2. **Apply Polymorphism**: Build systems that work with multiple shape types
3. **Use Abstract Base Classes**: Define shape interfaces
4. **Override Methods**: Customize behavior for different shapes
5. **Design Flexible Systems**: Create extensible shape collections

---

## 🛠️ Workshop Setup

### Prerequisites
- Python 3.8+ installed
- Text editor or IDE
- Understanding of inheritance concepts from tutorial

### Files to Create
- `shape_system.py` - Main shape hierarchy implementation
- `test_shapes.py` - Testing script for your shapes

### Getting Started
```bash
# Create a new directory for this workshop
mkdir shape_workshop
cd shape_workshop

# Create the main implementation file
touch shape_system.py
touch test_shapes.py
```

---

## 📝 Step-by-Step Implementation

### Step 1: Create the Base Shape Class

Start by creating an abstract base class for all shapes:

```python
# shape_system.py
from abc import ABC, abstractmethod
import math

class Shape(ABC):
    """Abstract base class for all geometric shapes."""

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def area(self):
        """Calculate and return the area of the shape."""
        pass

    @abstractmethod
    def perimeter(self):
        """Calculate and return the perimeter of the shape."""
        pass

    def describe(self):
        """Return a description of the shape."""
        return f"This is a {self.name} with area {self.area():.2f} and perimeter {self.perimeter():.2f}"

    def __str__(self):
        return self.name

    def __repr__(self):
        return f"{self.__class__.__name__}(name='{self.name}')"
```

**Key Points:**
- `Shape` inherits from `ABC` (Abstract Base Class)
- `area()` and `perimeter()` are abstract methods (must be implemented by subclasses)
- `describe()` provides common functionality for all shapes

### Step 2: Implement Rectangle Class

Create a Rectangle class that inherits from Shape:

```python
# Add to shape_system.py
class Rectangle(Shape):
    """A rectangular shape with width and height."""

    def __init__(self, name, width, height):
        super().__init__(name)
        self.width = width
        self.height = height

        # Validation
        if width <= 0 or height <= 0:
            raise ValueError("Width and height must be positive")

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

    def is_square(self):
        """Check if this rectangle is actually a square."""
        return self.width == self.height
```

**Key Points:**
- Inherits from `Shape` using `class Rectangle(Shape):`
- Calls `super().__init__(name)` to initialize parent
- Implements required abstract methods `area()` and `perimeter()`
- Adds rectangle-specific method `is_square()`
- Includes validation in constructor

### Step 3: Implement Circle Class

Create a Circle class that also inherits from Shape:

```python
# Add to shape_system.py
class Circle(Shape):
    """A circular shape with a radius."""

    def __init__(self, name, radius):
        super().__init__(name)
        self.radius = radius

        # Validation
        if radius <= 0:
            raise ValueError("Radius must be positive")

    def area(self):
        return math.pi * self.radius ** 2

    def perimeter(self):
        return 2 * math.pi * self.radius

    def diameter(self):
        """Calculate and return the diameter."""
        return 2 * self.radius
```

**Key Points:**
- Uses `math.pi` for accurate calculations
- Implements the same interface as Rectangle
- Maintains consistent method signatures

### Step 4: Implement Triangle Class

Add a Triangle class with method overriding:

```python
# Add to shape_system.py
class Triangle(Shape):
    """A triangular shape with three sides."""

    def __init__(self, name, side_a, side_b, side_c):
        super().__init__(name)
        self.side_a = side_a
        self.side_b = side_b
        self.side_c = side_c

        # Validation: triangle inequality
        if not (side_a + side_b > side_c and
                side_a + side_c > side_b and
                side_b + side_c > side_a):
            raise ValueError("Invalid triangle sides")

    def area(self):
        # Using Heron's formula
        s = self.perimeter() / 2  # semi-perimeter
        return math.sqrt(s * (s - self.side_a) * (s - self.side_b) * (s - self.side_c))

    def perimeter(self):
        return self.side_a + self.side_b + self.side_c

    def is_equilateral(self):
        """Check if all sides are equal."""
        return (self.side_a == self.side_b == self.side_c)

    def is_isosceles(self):
        """Check if at least two sides are equal."""
        return (self.side_a == self.side_b or
                self.side_a == self.side_c or
                self.side_b == self.side_c)
```

**Key Points:**
- More complex area calculation using Heron's formula
- Additional validation for triangle inequality theorem
- Extra utility methods for triangle classification

### Step 5: Create Shape Collection Class

Build a class to manage collections of shapes:

```python
# Add to shape_system.py
class ShapeCollection:
    """A collection of shapes with utility methods."""

    def __init__(self, name):
        self.name = name
        self.shapes = []

    def add_shape(self, shape):
        """Add a shape to the collection."""
        if isinstance(shape, Shape):
            self.shapes.append(shape)
        else:
            raise TypeError("Only Shape objects can be added")

    def total_area(self):
        """Calculate total area of all shapes."""
        return sum(shape.area() for shape in self.shapes)

    def total_perimeter(self):
        """Calculate total perimeter of all shapes."""
        return sum(shape.perimeter() for shape in self.shapes)

    def get_shapes_by_type(self, shape_type):
        """Get all shapes of a specific type."""
        return [shape for shape in self.shapes
                if isinstance(shape, shape_type)]

    def sort_by_area(self, descending=False):
        """Sort shapes by area."""
        return sorted(self.shapes, key=lambda s: s.area(), reverse=descending)

    def __len__(self):
        return len(self.shapes)

    def __iter__(self):
        return iter(self.shapes)
```

**Key Points:**
- Type checking ensures only Shape objects are added
- Polymorphic methods work with any shape type
- Utility methods demonstrate polymorphism benefits

---

## 🧪 Testing Your Implementation

Create `test_shapes.py` to verify your implementation:

```python
# test_shapes.py
from shape_system import Shape, Rectangle, Circle, Triangle, ShapeCollection

def test_shapes():
    print("🧪 Testing Shape Hierarchy System")
    print("=" * 40)

    # Test individual shapes
    rect = Rectangle("My Rectangle", 5, 10)
    circle = Circle("My Circle", 7)
    triangle = Triangle("My Triangle", 3, 4, 5)

    print(f"Rectangle: {rect.describe()}")
    print(f"Circle: {circle.describe()}")
    print(f"Triangle: {triangle.describe()}")
    print()

    # Test shape collection
    collection = ShapeCollection("My Shape Collection")
    collection.add_shape(rect)
    collection.add_shape(circle)
    collection.add_shape(triangle)

    print(f"Collection has {len(collection)} shapes")
    print(f"Total area: {collection.total_area():.2f}")
    print(f"Total perimeter: {collection.total_perimeter():.2f}")
    print()

    # Test polymorphism - same method, different behavior
    print("🎭 Polymorphism Demo:")
    for shape in collection:
        print(f"{shape.name}: {shape.area():.2f} area units")
    print()

    # Test sorting
    print("📊 Shapes sorted by area:")
    sorted_shapes = collection.sort_by_area(descending=True)
    for shape in sorted_shapes:
        print(f"{shape.name}: {shape.area():.2f}")

if __name__ == "__main__":
    test_shapes()
```

**Expected Output:**
```
🧪 Testing Shape Hierarchy System
========================================
Rectangle: This is a My Rectangle with area 50.00 and perimeter 30.00
Circle: This is a My Circle with area 153.94 and perimeter 43.98
Triangle: This is a My Triangle with area 6.00 and perimeter 12.00

Collection has 3 shapes
Total area: 209.94
Total perimeter: 85.98

🎭 Polymorphism Demo:
My Rectangle: 50.00 area units
My Circle: 153.94 area units
My Triangle: 6.00 area units

📊 Shapes sorted by area:
My Circle: 153.94
My Rectangle: 50.00
My Triangle: 6.00
```

---

## 🚀 Extension Challenges

### Challenge 1: Add More Shape Types
Create additional shape classes:
- `Square` (inherits from Rectangle)
- `Ellipse` (with major/minor axes)
- `RegularPolygon` (with n sides and side length)

### Challenge 2: Shape Statistics
Add methods to `ShapeCollection`:
- `average_area()` - Calculate average shape area
- `largest_shape()` - Find shape with maximum area
- `shape_type_distribution()` - Count shapes by type

### Challenge 3: Shape Serialization
Add methods to save/load shapes:
- `to_dict()` method for each shape
- `save_to_file()` and `load_from_file()` for collections
- Support JSON serialization

---

## 🐛 Troubleshooting

### Common Issues:

1. **Abstract Method Error**
   ```
   TypeError: Can't instantiate abstract class Shape with abstract methods area, perimeter
   ```
   **Solution**: Make sure all abstract methods are implemented in concrete classes.

2. **Method Resolution Order Issues**
   ```
   TypeError: super() takes at least 1 argument
   ```
   **Solution**: Always call `super().__init__()` in child constructors.

3. **Type Checking Problems**
   ```
   TypeError: Only Shape objects can be added
   ```
   **Solution**: Ensure all shape classes inherit from the `Shape` base class.

4. **Validation Errors**
   ```
   ValueError: Width and height must be positive
   ```
   **Solution**: Check that you're passing valid dimensions to shape constructors.

### Debug Tips:
- Use `isinstance(obj, Shape)` to check inheritance
- Call `shape.__class__.__mro__` to see method resolution order
- Add print statements in methods to trace execution

---

## ✅ Success Criteria

Your implementation is complete when:

- ✅ All shape classes inherit from `Shape`
- ✅ Abstract methods are implemented in all concrete classes
- ✅ Polymorphism works (same method calls on different shapes)
- ✅ ShapeCollection manages different shape types uniformly
- ✅ Validation prevents invalid shapes from being created
- ✅ All tests pass with expected output

---

## 📚 Key Takeaways

1. **Inheritance Structure**: Parent classes define common interfaces, child classes provide specific implementations

2. **Polymorphism Benefits**: Collections and operations work with any shape type through common interfaces

3. **Abstract Base Classes**: Ensure all subclasses implement required methods, providing type safety

4. **Method Overriding**: Child classes can customize inherited behavior while maintaining interface compatibility

5. **Composition vs Inheritance**: Use inheritance for "is-a" relationships, composition for "has-a" relationships

---

## 🔗 Next Steps

In the homework assignment, you'll extend this system by creating a vehicle management system that demonstrates more advanced inheritance and polymorphism concepts. The shape system you've built here provides a solid foundation for understanding these OOP principles!
# Homework 2: Vehicle Management System

**Section**: 2 - Inheritance & Polymorphism
**Estimated Time**: 4-6 hours
**Difficulty**: Intermediate
**Prerequisites**: Section 2 Tutorial & Workshop

---

## 📋 Assignment Overview

Create a comprehensive vehicle management system that demonstrates advanced inheritance and polymorphism concepts. You'll build a class hierarchy for different vehicle types, implement polymorphic operations, and create a system that can manage various vehicles through a common interface.

**Learning Focus**: Real-world application of inheritance, polymorphism, and abstract base classes

---

## 🎯 Learning Objectives

By completing this assignment, you will demonstrate the ability to:

1. **Design Class Hierarchies**: Create logical inheritance relationships
2. **Implement Polymorphism**: Build systems that work with multiple object types
3. **Use Abstract Base Classes**: Define interfaces for related classes
4. **Apply Method Overriding**: Customize behavior in child classes
5. **Manage Object Collections**: Handle polymorphic collections effectively

---

## 📋 Requirements

### Core Requirements

#### 1. Abstract Vehicle Base Class
Create an abstract `Vehicle` class with the following abstract methods:
- `start_engine()` - Start the vehicle's engine
- `stop_engine()` - Stop the vehicle's engine
- `accelerate(speed_increase)` - Increase vehicle speed
- `brake(speed_decrease)` - Decrease vehicle speed
- `get_info()` - Return vehicle information as a dictionary

#### 2. Concrete Vehicle Classes
Implement at least **4 different vehicle types**:

**Land Vehicles:**
- `Car` - Standard passenger car
- `Truck` - Commercial vehicle with cargo capacity
- `Motorcycle` - Two-wheeled vehicle

**Other Vehicles:**
- `Boat` - Water vehicle (extends beyond land vehicles)

Each vehicle class must:
- Inherit from the `Vehicle` base class
- Implement all abstract methods
- Have appropriate attributes (make, model, year, etc.)
- Include vehicle-specific attributes and methods

#### 3. Vehicle Collection System
Create a `Fleet` class that can manage multiple vehicles:

**Required Methods:**
- `add_vehicle(vehicle)` - Add a vehicle to the fleet
- `remove_vehicle(vehicle_id)` - Remove a vehicle by ID
- `get_vehicle(vehicle_id)` - Retrieve a vehicle by ID
- `list_vehicles()` - Return all vehicles in the fleet
- `start_all_engines()` - Start engines for all vehicles
- `stop_all_engines()` - Stop engines for all vehicles
- `get_fleet_status()` - Return status summary of all vehicles

**Optional Advanced Features:**
- `filter_by_type(vehicle_type)` - Get vehicles of specific type
- `calculate_fleet_value()` - Calculate total fleet value
- `find_vehicle_by_make(make)` - Search vehicles by manufacturer

### Implementation Requirements

#### Code Quality
- Use proper inheritance hierarchies
- Implement method overriding where appropriate
- Apply polymorphism throughout the system
- Include comprehensive error handling
- Add input validation for all methods
- Use meaningful variable and method names

#### Documentation
- Include docstrings for all classes and methods
- Add inline comments for complex logic
- Document class relationships and inheritance structure

#### Testing
- Create a comprehensive test script (`test_vehicles.py`)
- Demonstrate all vehicle types working correctly
- Show polymorphic behavior in collections
- Test error conditions and edge cases

---

## 🏗️ Implementation Guide

### Step 1: Design Your Class Hierarchy

```
Vehicle (Abstract Base Class)
├── Car
│   ├── Sedan
│   └── SUV
├── Truck
│   ├── PickupTruck
│   └── SemiTruck
├── Motorcycle
│   ├── SportBike
│   └── Cruiser
└── Boat
    ├── SpeedBoat
    └── Sailboat
```

### Step 2: Implement the Base Vehicle Class

```python
from abc import ABC, abstractmethod
from datetime import datetime

class Vehicle(ABC):
    """Abstract base class for all vehicles."""

    # Class variable for unique ID generation
    _next_id = 1

    def __init__(self, make, model, year, color="Unknown"):
        self.id = Vehicle._next_id
        Vehicle._next_id += 1

        self.make = make
        self.model = model
        self.year = year
        self.color = color
        self._current_speed = 0
        self._engine_running = False
        self._created_at = datetime.now()

    @abstractmethod
    def start_engine(self):
        """Start the vehicle's engine."""
        pass

    @abstractmethod
    def stop_engine(self):
        """Stop the vehicle's engine."""
        pass

    @abstractmethod
    def accelerate(self, speed_increase):
        """Increase vehicle speed by the given amount."""
        pass

    @abstractmethod
    def brake(self, speed_decrease):
        """Decrease vehicle speed by the given amount."""
        pass

    @abstractmethod
    def get_info(self):
        """Return vehicle information as a dictionary."""
        pass

    # Common methods
    def get_current_speed(self):
        """Get the current speed of the vehicle."""
        return self._current_speed

    def is_engine_running(self):
        """Check if the engine is currently running."""
        return self._engine_running

    def get_age(self):
        """Calculate the age of the vehicle in years."""
        return datetime.now().year - self.year
```

### Step 3: Implement Specific Vehicle Classes

#### Car Class Example:
```python
class Car(Vehicle):
    """A standard passenger car."""

    def __init__(self, make, model, year, color="Unknown", num_doors=4, fuel_type="Gasoline"):
        super().__init__(make, model, year, color)
        self.num_doors = num_doors
        self.fuel_type = fuel_type
        self._max_speed = 120  # mph

    def start_engine(self):
        if not self._engine_running:
            self._engine_running = True
            return f"{self.make} {self.model} engine started. Vroom!"
        return f"{self.make} {self.model} engine is already running."

    def stop_engine(self):
        if self._engine_running:
            self._current_speed = 0  # Car stops when engine is turned off
            self._engine_running = False
            return f"{self.make} {self.model} engine stopped."
        return f"{self.make} {self.model} engine is already off."

    def accelerate(self, speed_increase):
        if not self._engine_running:
            raise RuntimeError("Cannot accelerate: engine is not running")

        old_speed = self._current_speed
        self._current_speed = min(self._current_speed + speed_increase, self._max_speed)

        actual_increase = self._current_speed - old_speed
        return f"{self.make} {self.model} accelerated from {old_speed} to {self._current_speed} mph"

    def brake(self, speed_decrease):
        if self._current_speed == 0:
            return f"{self.make} {self.model} is already stopped."

        old_speed = self._current_speed
        self._current_speed = max(0, self._current_speed - speed_decrease)

        return f"{self.make} {self.model} slowed from {old_speed} to {self._current_speed} mph"

    def get_info(self):
        return {
            "id": self.id,
            "type": "Car",
            "make": self.make,
            "model": self.model,
            "year": self.year,
            "color": self.color,
            "doors": self.num_doors,
            "fuel_type": self.fuel_type,
            "current_speed": self._current_speed,
            "engine_running": self._engine_running,
            "age": self.get_age()
        }
```

### Step 4: Implement the Fleet Management System

```python
class Fleet:
    """A collection of vehicles with management capabilities."""

    def __init__(self, name):
        self.name = name
        self.vehicles = {}

    def add_vehicle(self, vehicle):
        """Add a vehicle to the fleet."""
        if not isinstance(vehicle, Vehicle):
            raise TypeError("Only Vehicle objects can be added to the fleet")

        self.vehicles[vehicle.id] = vehicle
        return f"Added {vehicle.make} {vehicle.model} (ID: {vehicle.id}) to {self.name}"

    def remove_vehicle(self, vehicle_id):
        """Remove a vehicle from the fleet by ID."""
        if vehicle_id in self.vehicles:
            vehicle = self.vehicles.pop(vehicle_id)
            return f"Removed {vehicle.make} {vehicle.model} (ID: {vehicle_id}) from {self.name}"
        raise ValueError(f"Vehicle with ID {vehicle_id} not found in fleet")

    def get_vehicle(self, vehicle_id):
        """Get a vehicle by ID."""
        if vehicle_id not in self.vehicles:
            raise ValueError(f"Vehicle with ID {vehicle_id} not found")
        return self.vehicles[vehicle_id]

    def list_vehicles(self):
        """Return all vehicles in the fleet."""
        return list(self.vehicles.values())

    def start_all_engines(self):
        """Start engines for all vehicles in the fleet."""
        results = []
        for vehicle in self.vehicles.values():
            try:
                result = vehicle.start_engine()
                results.append(f"✓ {vehicle.id}: {result}")
            except Exception as e:
                results.append(f"✗ {vehicle.id}: Failed to start - {e}")

        return "\n".join(results)

    def stop_all_engines(self):
        """Stop engines for all vehicles in the fleet."""
        results = []
        for vehicle in self.vehicles.values():
            try:
                result = vehicle.stop_engine()
                results.append(f"✓ {vehicle.id}: {result}")
            except Exception as e:
                results.append(f"✗ {vehicle.id}: Failed to stop - {e}")

        return "\n".join(results)

    def get_fleet_status(self):
        """Get a summary of the fleet status."""
        total_vehicles = len(self.vehicles)
        running_engines = sum(1 for v in self.vehicles.values() if v.is_engine_running())
        total_value = sum(getattr(v, 'value', 0) for v in self.vehicles.values())

        return {
            "fleet_name": self.name,
            "total_vehicles": total_vehicles,
            "engines_running": running_engines,
            "engines_stopped": total_vehicles - running_engines,
            "total_value": total_value
        }

    def __len__(self):
        return len(self.vehicles)
```

---

## 🧪 Testing Requirements

Create a comprehensive test script that demonstrates:

```python
# test_vehicles.py
from vehicle_system import Vehicle, Car, Truck, Motorcycle, Boat, Fleet

def test_vehicle_system():
    print("🚗 Testing Vehicle Management System")
    print("=" * 50)

    # Create vehicles
    car = Car("Toyota", "Camry", 2020, "Blue", 4, "Gasoline")
    truck = Truck("Ford", "F-150", 2019, "Red", 2000)
    motorcycle = Motorcycle("Honda", "CBR", 2021, "Black", "Sport")
    boat = Boat("Bayliner", "Element", 2018, "White", "Powerboat")

    # Create fleet
    fleet = Fleet("City Transportation Fleet")

    # Add vehicles to fleet
    for vehicle in [car, truck, motorcycle, boat]:
        print(fleet.add_vehicle(vehicle))

    print(f"\nFleet now has {len(fleet)} vehicles")
    print("\n🚀 Testing Polymorphic Operations:")

    # Demonstrate polymorphism
    print("\nStarting all engines:")
    print(fleet.start_all_engines())

    print("\nAccelerating vehicles:")
    for vehicle in fleet.list_vehicles():
        try:
            result = vehicle.accelerate(20)
            print(f"  {result}")
        except Exception as e:
            print(f"  ✗ {vehicle.make} {vehicle.model}: {e}")

    print("\nStopping all engines:")
    print(fleet.stop_all_engines())

    print("\n📊 Fleet Status:")
    status = fleet.get_fleet_status()
    for key, value in status.items():
        print(f"  {key}: {value}")

    print("\n📋 Vehicle Details:")
    for vehicle in fleet.list_vehicles():
        info = vehicle.get_info()
        print(f"  {info['make']} {info['model']} ({info['year']}) - {info['color']}")

if __name__ == "__main__":
    test_vehicle_system()
```

---

## 📋 Submission Requirements

### Code Files
- `vehicle_system.py` - Main implementation with all classes
- `test_vehicles.py` - Comprehensive test script
- `README.md` - Documentation explaining your design decisions

### Documentation Requirements
- Class hierarchy diagram (ASCII art or simple diagram)
- Explanation of inheritance decisions
- Description of polymorphic features
- Error handling strategies

### Test Results
Your test script should demonstrate:
- All vehicle types working correctly
- Polymorphic operations on the fleet
- Proper error handling
- Realistic vehicle behavior

---

## 🎯 Evaluation Criteria

### Code Quality (40%)
- Proper inheritance implementation
- Method overriding and polymorphism
- Error handling and validation
- Code organization and readability

### Functionality (40%)
- All required classes and methods implemented
- Abstract base class properly used
- Fleet management system working
- Polymorphic behavior demonstrated

### Testing (20%)
- Comprehensive test coverage
- Edge cases handled
- Clear test output and documentation

### Documentation (10%)
- Clear code comments and docstrings
- README with design explanations
- Class relationship documentation

---

## 🚀 Extension Challenges

### Advanced Features
1. **Vehicle Persistence** - Add save/load functionality using JSON
2. **Fuel Management** - Implement fuel consumption and refueling
3. **Maintenance System** - Track service schedules and repairs
4. **GPS Integration** - Add location tracking for vehicles
5. **Rental System** - Implement vehicle checkout/checkin functionality

### Performance Optimizations
1. **Lazy Loading** - Load vehicle data on demand
2. **Caching** - Cache expensive calculations
3. **Indexing** - Optimize vehicle lookups

---

## 🆘 Getting Help

### Common Issues
1. **Abstract Method Errors** - Ensure all abstract methods are implemented
2. **Type Errors** - Check inheritance relationships
3. **Attribute Errors** - Verify method calls and attribute access

### Resources
- Review Section 2 tutorial for inheritance patterns
- Check workshop examples for polymorphism implementation
- Python ABC documentation for abstract base classes

### Debug Tips
- Use `isinstance()` to check object types
- Print method resolution order with `Class.__mro__`
- Test individual components before integration

---

## ✅ Success Criteria

Your vehicle management system is complete when:

- ✅ All vehicle classes inherit properly from Vehicle
- ✅ Abstract methods are implemented in concrete classes
- ✅ Polymorphism works across different vehicle types
- ✅ Fleet management handles all vehicle operations
- ✅ Error handling prevents invalid operations
- ✅ Test script demonstrates all functionality
- ✅ Code is well-documented and readable

---

**Estimated Completion Time**: 4-6 hours
**Difficulty Level**: Intermediate
**Submission Deadline**: [Set by instructor]

Good luck building your vehicle management system! This assignment will demonstrate your mastery of inheritance and polymorphism concepts.
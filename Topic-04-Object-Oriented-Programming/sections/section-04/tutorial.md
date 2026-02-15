# Tutorial 4: Design Patterns

**Section**: 4 - Design Patterns (90 min)
**Level**: Advanced
**Prerequisites**: Sections 1-3 (OOP Fundamentals through Advanced Concepts)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Implement Singleton Pattern**: Create classes that ensure only one instance exists
2. **Apply Factory Pattern**: Build flexible object creation mechanisms
3. **Use Observer Pattern**: Implement event-driven systems with loose coupling
4. **Utilize Strategy Pattern**: Create interchangeable algorithms and behaviors
5. **Design Pattern Combinations**: Combine multiple patterns for complex solutions
6. **Pattern Selection**: Choose appropriate patterns for different design problems

---

## 📚 Table of Contents

1. [What are Design Patterns?](#what-are-design-patterns)
2. [Singleton Pattern](#singleton-pattern)
3. [Factory Pattern](#factory-pattern)
4. [Observer Pattern](#observer-pattern)
5. [Strategy Pattern](#strategy-pattern)
6. [Pattern Combinations](#pattern-combinations)
7. [When to Use Each Pattern](#when-to-use-each-pattern)

---

## 🤔 What are Design Patterns?

Design patterns are **proven solutions to common software design problems**. They provide templates for solving recurring design issues in object-oriented programming.

### Why Design Patterns Matter
- **Reusability**: Solutions that work across different contexts
- **Maintainability**: Well-understood structures that are easy to modify
- **Communication**: Common vocabulary for discussing design solutions
- **Best Practices**: Time-tested approaches to common problems

### Pattern Categories
- **Creational**: Object creation patterns (Singleton, Factory, Builder)
- **Structural**: Object composition patterns (Adapter, Decorator, Facade)
- **Behavioral**: Object interaction patterns (Observer, Strategy, Command)

---

## 🏆 Singleton Pattern

The Singleton pattern ensures that a class has only one instance and provides a global point of access to it.

### Basic Singleton Implementation

```python
class Singleton:
    """Basic singleton implementation using class variable"""

    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        # Only initialize once
        if not hasattr(self, '_initialized'):
            self.data = {}
            self._initialized = True

# Usage
singleton1 = Singleton()
singleton1.data['key'] = 'value'

singleton2 = Singleton()
print(singleton2.data['key'])  # 'value' - same instance
print(singleton1 is singleton2)  # True
```

### Thread-Safe Singleton (Advanced)

```python
import threading

class ThreadSafeSingleton:
    """Thread-safe singleton using double-checked locking"""

    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:
                # Double-check locking
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self.counter = 0
            self._initialized = True

    def increment(self):
        with self._lock:
            self.counter += 1
            return self.counter
```

### Metaclass Singleton (Most Pythonic)

```python
class SingletonMeta(type):
    """Metaclass for creating singleton classes"""

    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class DatabaseConnection(metaclass=SingletonMeta):
    """Database connection singleton"""

    def __init__(self, host="localhost", port=5432):
        if not hasattr(self, '_initialized'):
            self.host = host
            self.port = port
            self.connection_pool = []
            self._initialized = True
            print(f"Connected to database at {host}:{port}")

    def get_connection(self):
        return f"Connection from pool (host: {self.host})"

# Usage
db1 = DatabaseConnection("prod-db", 5432)
db2 = DatabaseConnection("dev-db", 5432)  # Ignored - same instance

print(db1.get_connection())
print(db2.get_connection())
print(db1 is db2)  # True
```

### Singleton Registry

```python
class SingletonRegistry:
    """Registry for managing multiple singletons"""

    _instances = {}

    @staticmethod
    def get_instance(cls, *args, **kwargs):
        """Get or create singleton instance"""
        if cls not in SingletonRegistry._instances:
            SingletonRegistry._instances[cls] = cls(*args, **kwargs)
        return SingletonRegistry._instances[cls]

    @staticmethod
    def clear_instance(cls):
        """Remove singleton instance (for testing)"""
        if cls in SingletonRegistry._instances:
            del SingletonRegistry._instances[cls]

class Logger(metaclass=SingletonMeta):
    def __init__(self):
        if not hasattr(self, '_initialized'):
            self.logs = []
            self._initialized = True

    def log(self, message, level="INFO"):
        self.logs.append(f"[{level}] {message}")

# Usage
logger1 = SingletonRegistry.get_instance(Logger)
logger1.log("Application started")

logger2 = SingletonRegistry.get_instance(Logger)
logger2.log("User logged in")

print(f"All logs: {logger1.logs}")
print(logger1 is logger2)  # True
```

---

## 🏭 Factory Pattern

Factory patterns provide interfaces for creating objects without specifying their concrete classes.

### Simple Factory

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class AnimalFactory:
    """Simple factory for creating animals"""

    @staticmethod
    def create_animal(animal_type):
        if animal_type == "dog":
            return Dog()
        elif animal_type == "cat":
            return Cat()
        else:
            raise ValueError(f"Unknown animal type: {animal_type}")

# Usage
dog = AnimalFactory.create_animal("dog")
cat = AnimalFactory.create_animal("cat")

print(dog.speak())  # Woof!
print(cat.speak())  # Meow!
```

### Factory Method Pattern

```python
from abc import ABC, abstractmethod

class Document(ABC):
    @abstractmethod
    def open(self):
        pass

    @abstractmethod
    def save(self):
        pass

class WordDocument(Document):
    def open(self):
        return "Opening Word document"

    def save(self):
        return "Saving Word document"

class PDFDocument(Document):
    def open(self):
        return "Opening PDF document"

    def save(self):
        return "Saving PDF document"

class DocumentCreator(ABC):
    """Abstract creator with factory method"""

    @abstractmethod
    def create_document(self):
        pass

    def open_document(self):
        doc = self.create_document()
        return doc.open()

class WordCreator(DocumentCreator):
    def create_document(self):
        return WordDocument()

class PDFCreator(DocumentCreator):
    def create_document(self):
        return PDFDocument()

# Usage
word_creator = WordCreator()
pdf_creator = PDFCreator()

print(word_creator.open_document())  # Opening Word document
print(pdf_creator.open_document())   # Opening PDF document
```

### Abstract Factory Pattern

```python
from abc import ABC, abstractmethod

class Button(ABC):
    @abstractmethod
    def render(self):
        pass

class Checkbox(ABC):
    @abstractmethod
    def render(self):
        pass

# Concrete products for Windows
class WindowsButton(Button):
    def render(self):
        return "Rendering Windows button"

class WindowsCheckbox(Checkbox):
    def render(self):
        return "Rendering Windows checkbox"

# Concrete products for macOS
class MacButton(Button):
    def render(self):
        return "Rendering macOS button"

class MacCheckbox(Checkbox):
    def render(self):
        return "Rendering macOS checkbox"

class GUIFactory(ABC):
    """Abstract factory for GUI components"""

    @abstractmethod
    def create_button(self):
        pass

    @abstractmethod
    def create_checkbox(self):
        pass

class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()

    def create_checkbox(self):
        return WindowsCheckbox()

class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()

    def create_checkbox(self):
        return MacCheckbox()

def create_ui(factory):
    """Create UI using abstract factory"""
    button = factory.create_button()
    checkbox = factory.create_checkbox()
    return button, checkbox

# Usage
windows_factory = WindowsFactory()
mac_factory = MacFactory()

# Create Windows UI
win_button, win_checkbox = create_ui(windows_factory)
print(win_button.render())
print(win_checkbox.render())

# Create macOS UI
mac_button, mac_checkbox = create_ui(mac_factory)
print(mac_button.render())
print(mac_checkbox.render())
```

---

## 👁️ Observer Pattern

The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

### Basic Observer Implementation

```python
from abc import ABC, abstractmethod

class Observer(ABC):
    """Abstract observer"""

    @abstractmethod
    def update(self, subject, data=None):
        pass

class Subject:
    """Observable subject"""

    def __init__(self):
        self._observers = []

    def attach(self, observer):
        """Add observer"""
        if observer not in self._observers:
            self._observers.append(observer)

    def detach(self, observer):
        """Remove observer"""
        self._observers.remove(observer)

    def notify(self, data=None):
        """Notify all observers"""
        for observer in self._observers:
            observer.update(self, data)

class NewsPublisher(Subject):
    """News publisher that notifies subscribers"""

    def __init__(self):
        super().__init__()
        self._latest_news = None

    def publish_news(self, news):
        self._latest_news = news
        print(f"Publishing news: {news}")
        self.notify(news)

class EmailSubscriber(Observer):
    def __init__(self, email):
        self.email = email

    def update(self, subject, data=None):
        print(f"Email to {self.email}: {data}")

class SMSSubscriber(Observer):
    def __init__(self, phone):
        self.phone = phone

    def update(self, subject, data=None):
        print(f"SMS to {self.phone}: {data}")

# Usage
publisher = NewsPublisher()

email_sub = EmailSubscriber("user@example.com")
sms_sub = SMSSubscriber("+1234567890")

publisher.attach(email_sub)
publisher.attach(sms_sub)

publisher.publish_news("Breaking: Python 4.0 Released!")
# Output:
# Publishing news: Breaking: Python 4.0 Released!
# Email to user@example.com: Breaking: Python 4.0 Released!
# SMS to +1234567890: Breaking: Python 4.0 Released!
```

### Event System with Observer

```python
from dataclasses import dataclass
from typing import Any, Dict
from datetime import datetime

@dataclass
class Event:
    name: str
    data: Dict[str, Any]
    timestamp: datetime = None

    def __post_init__(self):
        if self.timestamp is None:
            self.timestamp = datetime.now()

class EventManager(Subject):
    """Event manager using observer pattern"""

    def __init__(self):
        super().__init__()
        self._event_history = []

    def emit_event(self, event_name, **data):
        """Emit an event to all observers"""
        event = Event(event_name, data)
        self._event_history.append(event)
        self.notify(event)

    def get_event_history(self, event_name=None):
        """Get event history, optionally filtered by event name"""
        if event_name:
            return [e for e in self._event_history if e.name == event_name]
        return self._event_history

class UserActivityLogger(Observer):
    def update(self, subject, data=None):
        if isinstance(data, Event):
            print(f"[LOG] {data.timestamp}: {data.name} - {data.data}")

class NotificationService(Observer):
    def update(self, subject, data=None):
        if isinstance(data, Event) and data.name == "user_login":
            user = data.data.get('username')
            print(f"🔔 Welcome back, {user}!")

class AnalyticsTracker(Observer):
    def __init__(self):
        self.stats = {}

    def update(self, subject, data=None):
        if isinstance(data, Event):
            event_name = data.name
            self.stats[event_name] = self.stats.get(event_name, 0) + 1

    def get_stats(self):
        return self.stats

# Usage
event_manager = EventManager()

logger = UserActivityLogger()
notifier = NotificationService()
analytics = AnalyticsTracker()

event_manager.attach(logger)
event_manager.attach(notifier)
event_manager.attach(analytics)

# Simulate user activities
event_manager.emit_event("user_login", username="alice", ip="192.168.1.1")
event_manager.emit_event("page_view", page="/dashboard", user="alice")
event_manager.emit_event("user_logout", username="alice")

print(f"\nAnalytics: {analytics.get_stats()}")
```

---

## 🎯 Strategy Pattern

The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable.

### Basic Strategy Implementation

```python
from abc import ABC, abstractmethod

class PaymentStrategy(ABC):
    """Abstract strategy for payment processing"""

    @abstractmethod
    def pay(self, amount):
        pass

class CreditCardPayment(PaymentStrategy):
    def __init__(self, card_number, expiry_date, cvv):
        self.card_number = card_number
        self.expiry_date = expiry_date
        self.cvv = cvv

    def pay(self, amount):
        return f"Paid ${amount:.2f} using credit card ****{self.card_number[-4:]}"

class PayPalPayment(PaymentStrategy):
    def __init__(self, email, password):
        self.email = email
        self.password = password

    def pay(self, amount):
        return f"Paid ${amount:.2f} using PayPal account {self.email}"

class BitcoinPayment(PaymentStrategy):
    def __init__(self, wallet_address):
        self.wallet_address = wallet_address

    def pay(self, amount):
        return f"Paid ${amount:.2f} in Bitcoin to {self.wallet_address[:8]}..."

class ShoppingCart:
    """Context class that uses different strategies"""

    def __init__(self):
        self.items = []
        self._payment_strategy = None

    def add_item(self, item, price):
        self.items.append((item, price))

    def set_payment_strategy(self, strategy):
        """Set the payment strategy at runtime"""
        self._payment_strategy = strategy

    def checkout(self):
        total = sum(price for _, price in self.items)
        if self._payment_strategy is None:
            raise ValueError("Payment strategy not set")

        receipt = self._payment_strategy.pay(total)
        return receipt

# Usage
cart = ShoppingCart()
cart.add_item("Laptop", 999.99)
cart.add_item("Mouse", 29.99)

# Pay with different strategies
cart.set_payment_strategy(CreditCardPayment("1234567890123456", "12/25", "123"))
print(cart.checkout())

cart.set_payment_strategy(PayPalPayment("user@example.com", "password"))
print(cart.checkout())

cart.set_payment_strategy(BitcoinPayment("1A2B3C4D5E6F"))
print(cart.checkout())
```

### Sorting Strategy Example

```python
from abc import ABC, abstractmethod

class SortStrategy(ABC):
    @abstractmethod
    def sort(self, data):
        pass

class BubbleSort(SortStrategy):
    def sort(self, data):
        arr = data.copy()
        n = len(arr)
        for i in range(n):
            for j in range(0, n-i-1):
                if arr[j] > arr[j+1]:
                    arr[j], arr[j+1] = arr[j+1], arr[j]
        return arr

class QuickSort(SortStrategy):
    def sort(self, data):
        arr = data.copy()
        if len(arr) <= 1:
            return arr

        pivot = arr[len(arr) // 2]
        left = [x for x in arr if x < pivot]
        middle = [x for x in arr if x == pivot]
        right = [x for x in arr if x > pivot]

        return QuickSort().sort(left) + middle + QuickSort().sort(right)

class MergeSort(SortStrategy):
    def sort(self, data):
        arr = data.copy()
        if len(arr) <= 1:
            return arr

        mid = len(arr) // 2
        left = MergeSort().sort(arr[:mid])
        right = MergeSort().sort(arr[mid:])

        return self._merge(left, right)

    def _merge(self, left, right):
        result = []
        i = j = 0

        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1

        result.extend(left[i:])
        result.extend(right[j:])
        return result

class Sorter:
    """Context class for sorting"""

    def __init__(self, strategy=None):
        self._strategy = strategy or BubbleSort()

    def set_strategy(self, strategy):
        self._strategy = strategy

    def sort(self, data):
        return self._strategy.sort(data)

# Usage
data = [64, 34, 25, 12, 22, 11, 90]

sorter = Sorter()

# Use different sorting strategies
sorter.set_strategy(BubbleSort())
print("Bubble Sort:", sorter.sort(data))

sorter.set_strategy(QuickSort())
print("Quick Sort:", sorter.sort(data))

sorter.set_strategy(MergeSort())
print("Merge Sort:", sorter.sort(data))
```

---

## 🔗 Pattern Combinations

Combining multiple patterns creates powerful, flexible architectures.

### Factory + Singleton

```python
class ObjectPool(metaclass=SingletonMeta):
    """Singleton object pool using factory pattern"""

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._pool = {}
            self._factories = {}
            self._initialized = True

    def register_factory(self, object_type, factory_func):
        """Register a factory function for an object type"""
        self._factories[object_type] = factory_func

    def get_object(self, object_type, *args, **kwargs):
        """Get object from pool or create new one"""
        if object_type not in self._pool:
            self._pool[object_type] = []

        # Try to find available object
        for obj in self._pool[object_type]:
            if not getattr(obj, '_in_use', False):
                obj._in_use = True
                return obj

        # Create new object using factory
        if object_type in self._factories:
            obj = self._factories[object_type](*args, **kwargs)
            obj._in_use = True
            self._pool[object_type].append(obj)
            return obj

        raise ValueError(f"No factory registered for {object_type}")

    def release_object(self, obj):
        """Release object back to pool"""
        obj._in_use = False

# Usage
pool = ObjectPool()

# Register factories
pool.register_factory('database_connection',
    lambda host: f"DBConnection(host={host})")

pool.register_factory('cache_client',
    lambda: "RedisClient()")

# Get objects from pool
db1 = pool.get_object('database_connection', 'localhost')
db2 = pool.get_object('database_connection', 'localhost')  # Reuses connection

print(f"DB1: {db1}")
print(f"DB2: {db2}")

pool.release_object(db1)  # Return to pool
```

### Observer + Strategy

```python
class EventProcessor(Subject):
    """Event processor combining observer and strategy patterns"""

    def __init__(self):
        super().__init__()
        self._processing_strategy = None

    def set_processing_strategy(self, strategy):
        """Set how events are processed"""
        self._processing_strategy = strategy

    def process_event(self, event_data):
        """Process event using current strategy and notify observers"""
        if self._processing_strategy:
            result = self._processing_strategy.process(event_data)
            self.notify(result)
            return result
        return event_data

class LoggingStrategy:
    def process(self, event_data):
        print(f"[LOG] Processing: {event_data}")
        return {**event_data, 'logged': True}

class ValidationStrategy:
    def process(self, event_data):
        # Validate required fields
        required_fields = ['user_id', 'action']
        for field in required_fields:
            if field not in event_data:
                raise ValueError(f"Missing required field: {field}")

        return {**event_data, 'validated': True}

class AuditLogger(Observer):
    def update(self, subject, data=None):
        print(f"[AUDIT] Event processed: {data}")

class MetricsCollector(Observer):
    def __init__(self):
        self.metrics = {}

    def update(self, subject, data=None):
        event_type = data.get('action', 'unknown')
        self.metrics[event_type] = self.metrics.get(event_type, 0) + 1

# Usage
processor = EventProcessor()
processor.attach(AuditLogger())
processor.attach(MetricsCollector())

# Process with logging strategy
processor.set_processing_strategy(LoggingStrategy())
result1 = processor.process_event({'user_id': 123, 'action': 'login'})

# Switch to validation strategy
processor.set_processing_strategy(ValidationStrategy())
result2 = processor.process_event({'user_id': 456, 'action': 'logout'})

print(f"Results: {result1}, {result2}")
```

---

## 🎯 When to Use Each Pattern

### Singleton Pattern
- **Use when**: Need exactly one instance of a class
- **Examples**: Database connections, configuration managers, logging systems
- **Avoid when**: Multiple instances might be needed, testing requires isolation

### Factory Pattern
- **Use when**: Object creation is complex or should be abstracted
- **Examples**: GUI component creation, database connection factories
- **Avoid when**: Simple object creation with `ClassName()` is sufficient

### Observer Pattern
- **Use when**: Need loose coupling between objects that communicate
- **Examples**: Event systems, model-view separation, pub/sub architectures
- **Avoid when**: Objects are tightly coupled or direct method calls suffice

### Strategy Pattern
- **Use when**: Need interchangeable algorithms or behaviors
- **Examples**: Sorting algorithms, payment processing, compression methods
- **Avoid when**: Only one algorithm is needed or inheritance can solve it

### General Guidelines
- **Start simple**: Don't use patterns where direct solutions work
- **Consider maintenance**: Patterns should make code easier to maintain
- **Document usage**: Explain why you chose a particular pattern
- **Test thoroughly**: Patterns can introduce complexity that needs testing

---

## 🏁 Summary

Design patterns provide proven solutions to common software design problems:

- **Singleton** ensures unique instances and global access points
- **Factory** abstracts object creation and provides flexibility
- **Observer** enables loose coupling through event-driven systems
- **Strategy** allows interchangeable algorithms and behaviors
- **Combinations** create powerful, flexible architectures

Key takeaways:
- Patterns are tools, not goals - use them when they solve real problems
- Understand the problem before applying a pattern
- Patterns work best when combined thoughtfully
- Always consider the trade-offs and maintenance implications

In the workshop, you'll apply these patterns by building an e-commerce system!
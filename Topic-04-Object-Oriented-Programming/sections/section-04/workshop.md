# Workshop 4: E-commerce System with Design Patterns

**Section**: 4 - Design Patterns (90 min)
**Level**: Advanced
**Prerequisites**: Section 4 Tutorial (Design Patterns)

---

## 📋 Workshop Overview

In this workshop, you'll build a comprehensive e-commerce system that demonstrates multiple design patterns working together. You'll implement shopping carts with observer notifications, payment processing with strategy patterns, product catalogs with factory patterns, and user session management with singletons.

**Estimated Time**: 40-50 minutes
**Learning Focus**: Practical application of design patterns in real systems

---

## 🎯 Learning Objectives

By completing this workshop, you will:

1. **Implement Singleton Pattern**: Create session management and configuration systems
2. **Apply Factory Pattern**: Build flexible product and payment creation
3. **Use Observer Pattern**: Implement event-driven cart notifications and logging
4. **Utilize Strategy Pattern**: Create interchangeable payment and shipping methods
5. **Combine Multiple Patterns**: Build a cohesive e-commerce architecture
6. **Design Pattern Integration**: Connect patterns for complex functionality

---

## 🛠️ Workshop Setup

### Prerequisites
- Python 3.8+ installed
- Text editor or IDE
- Understanding of design patterns from tutorial

### Files to Create
- `ecommerce_system.py` - Main e-commerce system implementation
- `test_ecommerce.py` - Testing script for your e-commerce system

### Getting Started
```bash
# Create a new directory for this workshop
mkdir ecommerce_workshop
cd ecommerce_workshop

# Create the main implementation file
touch ecommerce_system.py
touch test_ecommerce.py
```

---

## 📝 Step-by-Step Implementation

### Step 1: Create Base Classes and Singletons

Start with the core infrastructure using singleton patterns:

```python
# ecommerce_system.py
from abc import ABC, abstractmethod
from dataclasses import dataclass, field
from datetime import datetime
from typing import List, Dict, Optional, Any
import threading

class SingletonMeta(type):
    """Thread-safe singleton metaclass"""
    _instances = {}
    _lock = threading.Lock()

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            with cls._lock:
                if cls not in cls._instances:
                    cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

@dataclass
class Product:
    """Product data class"""
    id: int
    name: str
    price: float
    category: str
    description: str = ""
    in_stock: bool = True

@dataclass
class CartItem:
    """Shopping cart item"""
    product: Product
    quantity: int
    added_at: datetime = field(default_factory=datetime.now)

    @property
    def total_price(self):
        return self.product.price * self.quantity

class SessionManager(metaclass=SingletonMeta):
    """Singleton session manager"""

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._sessions = {}
            self._initialized = True

    def create_session(self, user_id):
        """Create new user session"""
        session_id = f"session_{user_id}_{len(self._sessions)}"
        self._sessions[session_id] = {
            'user_id': user_id,
            'created_at': datetime.now(),
            'data': {}
        }
        return session_id

    def get_session(self, session_id):
        """Get session data"""
        return self._sessions.get(session_id)

    def update_session(self, session_id, key, value):
        """Update session data"""
        if session_id in self._sessions:
            self._sessions[session_id]['data'][key] = value

    def clear_session(self, session_id):
        """Clear session data"""
        if session_id in self._sessions:
            del self._sessions[session_id]

class ConfigurationManager(metaclass=SingletonMeta):
    """Singleton configuration manager"""

    def __init__(self):
        if not hasattr(self, '_initialized'):
            self._config = {
                'tax_rate': 0.08,
                'shipping_free_threshold': 50.0,
                'currency': 'USD',
                'supported_payment_methods': ['credit_card', 'paypal', 'bank_transfer']
            }
            self._initialized = True

    def get(self, key, default=None):
        """Get configuration value"""
        return self._config.get(key, default)

    def set(self, key, value):
        """Set configuration value"""
        self._config[key] = value
```

**Key Points:**
- Thread-safe singleton metaclass for concurrent access
- Session manager for user state management
- Configuration manager for system settings

### Step 2: Implement Observer Pattern for Cart Events

Create the shopping cart with observer notifications:

```python
# Add to ecommerce_system.py
class Observer(ABC):
    """Abstract observer for cart events"""

    @abstractmethod
    def update(self, event_type, data):
        pass

class Subject(ABC):
    """Abstract subject that maintains observers"""

    def __init__(self):
        self._observers = []

    def attach(self, observer):
        """Add observer"""
        if observer not in self._observers:
            self._observers.append(observer)

    def detach(self, observer):
        """Remove observer"""
        self._observers.remove(observer)

    def notify(self, event_type, data=None):
        """Notify all observers"""
        for observer in self._observers:
            observer.update(event_type, data)

class ShoppingCart(Subject):
    """Shopping cart with observer notifications"""

    def __init__(self, user_id):
        super().__init__()
        self.user_id = user_id
        self.items = []
        self.created_at = datetime.now()

    def add_item(self, product, quantity=1):
        """Add item to cart"""
        # Check if item already exists
        for item in self.items:
            if item.product.id == product.id:
                item.quantity += quantity
                self.notify('item_updated', {'product': product, 'new_quantity': item.quantity})
                return

        # Add new item
        cart_item = CartItem(product, quantity)
        self.items.append(cart_item)
        self.notify('item_added', {'item': cart_item})

    def remove_item(self, product_id):
        """Remove item from cart"""
        for i, item in enumerate(self.items):
            if item.product.id == product_id:
                removed_item = self.items.pop(i)
                self.notify('item_removed', {'item': removed_item})
                return
        raise ValueError(f"Product {product_id} not in cart")

    def update_quantity(self, product_id, new_quantity):
        """Update item quantity"""
        if new_quantity <= 0:
            self.remove_item(product_id)
            return

        for item in self.items:
            if item.product.id == product_id:
                old_quantity = item.quantity
                item.quantity = new_quantity
                self.notify('quantity_updated', {
                    'product': item.product,
                    'old_quantity': old_quantity,
                    'new_quantity': new_quantity
                })
                return
        raise ValueError(f"Product {product_id} not in cart")

    def clear_cart(self):
        """Clear all items"""
        cleared_items = self.items.copy()
        self.items.clear()
        self.notify('cart_cleared', {'cleared_items': cleared_items})

    @property
    def total_items(self):
        """Total number of items"""
        return sum(item.quantity for item in self.items)

    @property
    def subtotal(self):
        """Cart subtotal before tax/shipping"""
        return sum(item.total_price for item in self.items)

    def get_summary(self):
        """Get cart summary"""
        config = ConfigurationManager()
        tax_rate = config.get('tax_rate', 0.08)
        tax = self.subtotal * tax_rate
        total = self.subtotal + tax

        return {
            'items': len(self.items),
            'total_quantity': self.total_items,
            'subtotal': round(self.subtotal, 2),
            'tax': round(tax, 2),
            'total': round(total, 2)
        }
```

**Key Points:**
- Inherits from Subject for observer functionality
- Notifies observers of all cart changes
- Provides comprehensive cart operations

### Step 3: Implement Factory Pattern for Products and Payments

Create factory classes for flexible object creation:

```python
# Add to ecommerce_system.py
class ProductFactory:
    """Factory for creating products"""

    _product_counter = 1

    @staticmethod
    def create_electronics(name, price, description=""):
        """Create electronics product"""
        product = Product(
            id=ProductFactory._product_counter,
            name=name,
            price=price,
            category="Electronics",
            description=description
        )
        ProductFactory._product_counter += 1
        return product

    @staticmethod
    def create_book(title, author, price, genre="General"):
        """Create book product"""
        product = Product(
            id=ProductFactory._product_counter,
            name=f"{title} by {author}",
            price=price,
            category="Books",
            description=f"Genre: {genre}"
        )
        ProductFactory._product_counter += 1
        return product

    @staticmethod
    def create_clothing(item_type, size, color, price):
        """Create clothing product"""
        product = Product(
            id=ProductFactory._product_counter,
            name=f"{color} {size} {item_type}",
            price=price,
            category="Clothing",
            description=f"Size: {size}, Color: {color}"
        )
        ProductFactory._product_counter += 1
        return product

    @classmethod
    def create_from_dict(cls, product_dict):
        """Create product from dictionary"""
        category = product_dict.get('category', 'General')

        if category == 'Electronics':
            return cls.create_electronics(
                product_dict['name'],
                product_dict['price'],
                product_dict.get('description', '')
            )
        elif category == 'Books':
            return cls.create_book(
                product_dict['name'],
                product_dict.get('author', 'Unknown'),
                product_dict['price'],
                product_dict.get('genre', 'General')
            )
        else:
            # Generic product creation
            product = Product(
                id=cls._product_counter,
                name=product_dict['name'],
                price=product_dict['price'],
                category=category,
                description=product_dict.get('description', '')
            )
            cls._product_counter += 1
            return product

class PaymentMethod(ABC):
    """Abstract payment method"""

    @abstractmethod
    def process_payment(self, amount):
        pass

class CreditCardPayment(PaymentMethod):
    def __init__(self, card_number, expiry_date, cvv):
        self.card_number = card_number
        self.expiry_date = expiry_date
        self.cvv = cvv

    def process_payment(self, amount):
        return f"Processed ${amount:.2f} via credit card ****{self.card_number[-4:]}"

class PayPalPayment(PaymentMethod):
    def __init__(self, email):
        self.email = email

    def process_payment(self, amount):
        return f"Processed ${amount:.2f} via PayPal ({self.email})"

class BankTransferPayment(PaymentMethod):
    def __init__(self, account_number, routing_number):
        self.account_number = account_number
        self.routing_number = routing_number

    def process_payment(self, amount):
        return f"Processed ${amount:.2f} via bank transfer (****{self.account_number[-4:]})"

class PaymentFactory:
    """Factory for creating payment methods"""

    @staticmethod
    def create_credit_card(card_number, expiry_date, cvv):
        """Create credit card payment method"""
        return CreditCardPayment(card_number, expiry_date, cvv)

    @staticmethod
    def create_paypal(email):
        """Create PayPal payment method"""
        return PayPalPayment(email)

    @staticmethod
    def create_bank_transfer(account_number, routing_number):
        """Create bank transfer payment method"""
        return BankTransferPayment(account_number, routing_number)

    @classmethod
    def create_from_type(cls, payment_type, **kwargs):
        """Create payment method from type string"""
        if payment_type == 'credit_card':
            return cls.create_credit_card(**kwargs)
        elif payment_type == 'paypal':
            return cls.create_paypal(**kwargs)
        elif payment_type == 'bank_transfer':
            return cls.create_bank_transfer(**kwargs)
        else:
            raise ValueError(f"Unknown payment type: {payment_type}")
```

**Key Points:**
- ProductFactory creates different types of products
- PaymentFactory creates different payment methods
- Both support creation from dictionaries/types

### Step 4: Implement Strategy Pattern for Shipping

Create interchangeable shipping strategies:

```python
# Add to ecommerce_system.py
class ShippingStrategy(ABC):
    """Abstract shipping strategy"""

    @abstractmethod
    def calculate_cost(self, cart):
        pass

    @abstractmethod
    def get_delivery_time(self):
        pass

class StandardShipping(ShippingStrategy):
    """Standard shipping strategy"""

    def calculate_cost(self, cart):
        base_cost = 5.99
        item_cost = cart.total_items * 0.50
        return base_cost + item_cost

    def get_delivery_time(self):
        return "5-7 business days"

class ExpressShipping(ShippingStrategy):
    """Express shipping strategy"""

    def calculate_cost(self, cart):
        base_cost = 12.99
        item_cost = cart.total_items * 1.00
        return base_cost + item_cost

    def get_delivery_time(self):
        return "2-3 business days"

class OvernightShipping(ShippingStrategy):
    """Overnight shipping strategy"""

    def calculate_cost(self, cart):
        base_cost = 24.99
        item_cost = cart.total_items * 2.00
        return base_cost + item_cost

    def get_delivery_time(self):
        return "Next business day"

class FreeShipping(ShippingStrategy):
    """Free shipping for orders over threshold"""

    def __init__(self):
        self.config = ConfigurationManager()

    def calculate_cost(self, cart):
        threshold = self.config.get('shipping_free_threshold', 50.0)
        if cart.subtotal >= threshold:
            return 0.0
        return 999.99  # Effectively disable if threshold not met

    def get_delivery_time(self):
        return "7-10 business days"

class ShippingCalculator:
    """Context for shipping strategy"""

    def __init__(self, strategy=None):
        self._strategy = strategy or StandardShipping()

    def set_strategy(self, strategy):
        """Change shipping strategy"""
        self._strategy = strategy

    def calculate_shipping(self, cart):
        """Calculate shipping cost"""
        return self._strategy.calculate_cost(cart)

    def get_delivery_estimate(self):
        """Get delivery time estimate"""
        return self._strategy.get_delivery_time()
```

**Key Points:**
- Abstract base class for shipping strategies
- Multiple concrete strategies with different cost/time trade-offs
- Context class allows runtime strategy changes

### Step 5: Create Observer Implementations

Implement concrete observers for cart events:

```python
# Add to ecommerce_system.py
class CartLogger(Observer):
    """Observer that logs cart events"""

    def update(self, event_type, data=None):
        timestamp = datetime.now().strftime('%H:%M:%S')
        if event_type == 'item_added':
            item = data['item']
            print(f"[{timestamp}] LOG: Added {item.quantity}x {item.product.name}")
        elif event_type == 'item_removed':
            item = data['item']
            print(f"[{timestamp}] LOG: Removed {item.product.name}")
        elif event_type == 'quantity_updated':
            product = data['product']
            print(f"[{timestamp}] LOG: Updated {product.name} quantity: {data['old_quantity']} -> {data['new_quantity']}")
        elif event_type == 'cart_cleared':
            items = data['cleared_items']
            print(f"[{timestamp}] LOG: Cleared cart ({len(items)} items)")

class InventoryManager(Observer):
    """Observer that manages inventory"""

    def __init__(self):
        self.inventory_alerts = []

    def update(self, event_type, data=None):
        if event_type == 'item_added':
            item = data['item']
            # In real system, would check inventory levels
            if item.quantity > 10:  # Arbitrary threshold
                alert = f"High quantity alert: {item.quantity}x {item.product.name}"
                self.inventory_alerts.append(alert)
                print(f"⚠️  INVENTORY: {alert}")

class EmailNotifier(Observer):
    """Observer that sends email notifications"""

    def update(self, event_type, data=None):
        if event_type == 'cart_cleared':
            print("📧 EMAIL: Cart cleared notification sent to user")
        elif event_type == 'item_added' and data['item'].quantity >= 5:
            item = data['item']
            print(f"📧 EMAIL: Bulk purchase alert for {item.product.name}")
```

**Key Points:**
- CartLogger provides audit trail of cart changes
- InventoryManager monitors stock levels
- EmailNotifier sends notifications for important events

### Step 6: Create the Main E-commerce System

Integrate all components into a complete system:

```python
# Add to ecommerce_system.py
class EcommerceSystem:
    """Main e-commerce system integrating all patterns"""

    def __init__(self):
        self.session_manager = SessionManager()
        self.config_manager = ConfigurationManager()
        self.product_factory = ProductFactory()
        self.payment_factory = PaymentFactory()
        self._carts = {}  # session_id -> cart

    def create_session(self, user_id):
        """Create user session"""
        return self.session_manager.create_session(user_id)

    def get_cart(self, session_id):
        """Get or create cart for session"""
        if session_id not in self._carts:
            session = self.session_manager.get_session(session_id)
            if not session:
                raise ValueError("Invalid session")

            user_id = session['user_id']
            cart = ShoppingCart(user_id)

            # Attach observers
            cart.attach(CartLogger())
            cart.attach(InventoryManager())
            cart.attach(EmailNotifier())

            self._carts[session_id] = cart

        return self._carts[session_id]

    def add_to_cart(self, session_id, product, quantity=1):
        """Add product to user's cart"""
        cart = self.get_cart(session_id)
        cart.add_item(product, quantity)

    def checkout(self, session_id, payment_type, shipping_strategy=None, **payment_kwargs):
        """Complete purchase"""
        cart = self.get_cart(session_id)
        if not cart.items:
            raise ValueError("Cart is empty")

        # Calculate costs
        summary = cart.get_summary()

        # Apply shipping
        shipping_calc = ShippingCalculator(shipping_strategy)
        shipping_cost = shipping_calc.calculate_shipping(cart)
        delivery_time = shipping_calc.get_delivery_estimate()

        total_cost = summary['total'] + shipping_cost

        # Process payment
        payment_method = self.payment_factory.create_from_type(payment_type, **payment_kwargs)
        payment_result = payment_method.process_payment(total_cost)

        # Clear cart after successful payment
        cart.clear_cart()

        return {
            'order_total': round(total_cost, 2),
            'shipping_cost': round(shipping_cost, 2),
            'delivery_time': delivery_time,
            'payment_result': payment_result,
            'items_purchased': summary['total_quantity']
        }
```

**Key Points:**
- Integrates all design patterns into cohesive system
- Manages user sessions and shopping carts
- Provides complete checkout process

---

## 🧪 Testing Your Implementation

Create `test_ecommerce.py` to verify your implementation:

```python
# test_ecommerce.py
from ecommerce_system import *

def test_ecommerce_system():
    print("🛒 Testing E-commerce System")
    print("=" * 50)

    # Initialize system
    system = EcommerceSystem()

    # Create user session
    session_id = system.create_session("user123")
    print(f"Created session: {session_id}")

    # Create products using factory
    laptop = ProductFactory.create_electronics("Gaming Laptop", 1299.99, "High-performance gaming laptop")
    book = ProductFactory.create_book("Design Patterns", "Gang of Four", 49.99, "Computer Science")
    shirt = ProductFactory.create_clothing("T-Shirt", "L", "Blue", 19.99)

    print(f"\nCreated products:")
    for product in [laptop, book, shirt]:
        print(f"  {product.name} (${product.price}) - {product.category}")

    # Add items to cart (triggers observer notifications)
    print(f"\n🛒 Adding items to cart:")
    system.add_to_cart(session_id, laptop, 1)
    system.add_to_cart(session_id, book, 2)
    system.add_to_cart(session_id, shirt, 3)

    # Get cart and show summary
    cart = system.get_cart(session_id)
    summary = cart.get_summary()
    print(f"\nCart Summary: {summary}")

    # Test different shipping strategies
    print(f"\n🚚 Testing Shipping Strategies:")
    shipping_calc = ShippingCalculator()

    for strategy_name, strategy in [
        ("Standard", StandardShipping()),
        ("Express", ExpressShipping()),
        ("Free", FreeShipping())
    ]:
        shipping_calc.set_strategy(strategy)
        cost = shipping_calc.calculate_shipping(cart)
        time = shipping_calc.get_delivery_estimate()
        print(f"  {strategy_name}: ${cost:.2f} - {time}")

    # Perform checkout with different payment methods
    print(f"\n💳 Testing Checkout:")

    # Test credit card payment
    try:
        result = system.checkout(
            session_id,
            'credit_card',
            shipping_strategy=ExpressShipping(),
            card_number='1234567890123456',
            expiry_date='12/25',
            cvv='123'
        )
        print(f"Credit Card Checkout: {result}")
    except Exception as e:
        print(f"Checkout failed: {e}")

    # Add items again and test PayPal
    system.add_to_cart(session_id, book, 1)
    try:
        result = system.checkout(
            session_id,
            'paypal',
            shipping_strategy=StandardShipping(),
            email='user@example.com'
        )
        print(f"PayPal Checkout: {result}")
    except Exception as e:
        print(f"Checkout failed: {e}")

    # Test configuration manager
    config = ConfigurationManager()
    print(f"\n⚙️ Configuration:")
    print(f"  Tax Rate: {config.get('tax_rate')}")
    print(f"  Currency: {config.get('currency')}")
    print(f"  Free Shipping Threshold: ${config.get('shipping_free_threshold')}")

    # Test session management
    session_data = system.session_manager.get_session(session_id)
    print(f"\n📊 Session Data: {session_data}")

if __name__ == "__main__":
    test_ecommerce_system()
```

**Expected Output:**
```
🛒 Testing E-commerce System
==================================================
Created session: session_user123_0

Created products:
  Gaming Laptop ($1299.99) - Electronics
  Design Patterns by Gang of Four ($49.99) - Books
  Blue L T-Shirt ($19.99) - Clothing

🛒 Adding items to cart:
[14:30:15] LOG: Added 1x Gaming Laptop
[14:30:15] LOG: Added 2x Design Patterns by Gang of Four
[14:30:15] LOG: Added 3x Blue L T-Shirt

Cart Summary: {'items': 3, 'total_quantity': 6, 'subtotal': 1429.95, 'tax': 114.4, 'total': 1544.35}

🚚 Testing Shipping Strategies:
  Standard: $8.99 - 5-7 business days
  Express: $18.99 - 2-3 business days
  Free: $0.00 - 7-10 business days

💳 Testing Checkout:
[14:30:15] LOG: Removed Design Patterns by Gang of Four
[14:30:15] LOG: Removed Blue L T-Shirt
[14:30:15] LOG: Removed Gaming Laptop
📧 EMAIL: Cart cleared notification sent to user
Credit Card Checkout: {'order_total': 1563.34, 'shipping_cost': 18.99, 'delivery_time': '2-3 business days', 'payment_result': 'Processed $1563.34 via credit card ****3456', 'items_purchased': 6}

🛒 Adding items to cart:
[14:30:15] LOG: Added 1x Design Patterns by Gang of Four

💳 Testing Checkout:
[14:30:15] LOG: Removed Design Patterns by Gang of Four
📧 EMAIL: Cart cleared notification sent to user
PayPal Checkout: {'order_total': 58.49, 'shipping_cost': 8.99, 'delivery_time': '5-7 business days', 'payment_result': 'Processed $58.49 via PayPal (user@example.com)', 'items_purchased': 1}

⚙️ Configuration:
  Tax Rate: 0.08
  Currency: USD
  Free Shipping Threshold: $50.0

📊 Session Data: {'user_id': 'user123', 'created_at': datetime.datetime(2026, 2, 15, 14, 30, 15), 'data': {}}
```

---

## 🚀 Extension Challenges

### Challenge 1: Product Categories
Extend the ProductFactory to support more categories:
- Furniture products with dimensions
- Food products with expiration dates
- Digital products (no shipping needed)

### Challenge 2: Advanced Payment Strategies
Add more payment methods:
- Cryptocurrency payments
- Buy now, pay later options
- Gift card payments
- Multi-step payment flows

### Challenge 3: Order Management
Implement order tracking and management:
- Order history for users
- Order status updates (pending, shipped, delivered)
- Return and refund processing
- Order search and filtering

---

## 🐛 Troubleshooting

### Common Issues:

1. **Observer Not Notifying**
   ```
   # Problem: Forgot to call super().__init__() in Subject
   class ShoppingCart(Subject):
       def __init__(self, user_id):
           self.user_id = user_id  # Missing super().__init__()
   ```
   **Solution**: Always call `super().__init__()` in Subject subclasses.

2. **Singleton Thread Safety**
   ```
   # Problem: Race condition in singleton creation
   ```
   **Solution**: Use the thread-safe SingletonMeta metaclass provided.

3. **Factory Method Errors**
   ```
   TypeError: create_from_type() missing required arguments
   ```
   **Solution**: Ensure all required arguments are passed to factory methods.

4. **Strategy Pattern Issues**
   ```
   AttributeError: 'ShippingCalculator' object has no attribute '_strategy'
   ```
   **Solution**: Initialize the strategy in the constructor.

### Debug Tips:
- Add print statements in observer `update()` methods to trace notifications
- Test each pattern individually before combining them
- Use the session manager to inspect cart state during debugging

---

## ✅ Success Criteria

Your e-commerce system implementation is complete when:

- ✅ Singleton pattern works for session and configuration management
- ✅ Factory pattern creates products and payment methods flexibly
- ✅ Observer pattern notifies subscribers of cart changes
- ✅ Strategy pattern allows interchangeable shipping and payment methods
- ✅ All patterns work together in the complete checkout flow
- ✅ Test script demonstrates all functionality with expected output

---

## 📚 Key Takeaways

1. **Singleton Pattern**: Ensures unique instances for shared resources like sessions and configuration
2. **Factory Pattern**: Provides flexible object creation and hides complexity
3. **Observer Pattern**: Enables loose coupling through event-driven notifications
4. **Strategy Pattern**: Allows runtime algorithm selection for shipping and payments
5. **Pattern Integration**: Real systems combine multiple patterns for robust architectures

---

## 🔗 Next Steps

In the homework assignment, you'll create a game development framework that uses design patterns to manage characters, events, and game state. This will demonstrate how patterns work together in complex, interactive systems!
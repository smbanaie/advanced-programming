# Workshop 3: Banking System Implementation

**Section**: 3 - Advanced OOP Concepts (90 min)
**Level**: Intermediate
**Prerequisites**: Section 3 Tutorial (Advanced OOP Concepts)

---

## 📋 Workshop Overview

In this workshop, you'll build a comprehensive banking system that demonstrates advanced Object-Oriented Programming concepts. You'll implement magic methods for account operations, property decorators for data validation, class methods for account creation patterns, and dataclasses for transaction management.

**Estimated Time**: 40-50 minutes
**Learning Focus**: Practical application of advanced OOP features

---

## 🎯 Learning Objectives

By completing this workshop, you will:

1. **Implement Magic Methods**: Use special methods for account comparisons and operations
2. **Apply Property Decorators**: Control access to account balance with validation
3. **Use Class Methods**: Create alternative account constructors
4. **Work with Dataclasses**: Model transactions and account data
5. **Combine Advanced Concepts**: Build a sophisticated banking system

---

## 🛠️ Workshop Setup

### Prerequisites
- Python 3.8+ installed
- Text editor or IDE
- Understanding of advanced OOP concepts from tutorial

### Files to Create
- `banking_system.py` - Main banking system implementation
- `test_banking.py` - Testing script for your banking system

### Getting Started
```bash
# Create a new directory for this workshop
mkdir banking_workshop
cd banking_workshop

# Create the main implementation file
touch banking_system.py
touch test_banking.py
```

---

## 📝 Step-by-Step Implementation

### Step 1: Create the Transaction Dataclass

Start with a dataclass for bank transactions:

```python
# banking_system.py
from dataclasses import dataclass, field
from datetime import datetime
from typing import List, Optional

@dataclass
class Transaction:
    """Represents a bank transaction"""
    amount: float
    description: str
    timestamp: datetime = field(default_factory=datetime.now)
    transaction_type: str = "debit"  # "debit" or "credit"
    transaction_id: Optional[int] = None

    def __post_init__(self):
        """Validate transaction data"""
        if self.amount <= 0:
            raise ValueError("Transaction amount must be positive")
        if self.transaction_type not in ["debit", "credit"]:
            raise ValueError("Transaction type must be 'debit' or 'credit'")

    def __str__(self):
        symbol = "-" if self.transaction_type == "debit" else "+"
        return f"{self.timestamp.strftime('%Y-%m-%d %H:%M')} {symbol}${self.amount:.2f} {self.description}"

# Transaction ID counter
_next_transaction_id = 1
```

**Key Points:**
- Uses `@dataclass` for automatic method generation
- `__post_init__` validates data after initialization
- Provides readable string representation

### Step 2: Implement the BankAccount Class with Magic Methods

Create the main BankAccount class with advanced OOP features:

```python
# Add to banking_system.py
class BankAccount:
    """Bank account with advanced OOP features"""

    # Class variable for account number generation
    _next_account_number = 1000
    _accounts = {}  # Store all accounts

    def __init__(self, owner_name, initial_balance=0.0):
        self._owner_name = owner_name
        self._account_number = BankAccount._next_account_number
        BankAccount._next_account_number += 1
        self._balance = float(initial_balance)
        self._transactions = []
        self._is_frozen = False

        # Register account
        BankAccount._accounts[self._account_number] = self

    @property
    def owner_name(self):
        """Get account owner name"""
        return self._owner_name

    @owner_name.setter
    def owner_name(self, value):
        """Set owner name with validation"""
        if not value.strip():
            raise ValueError("Owner name cannot be empty")
        self._owner_name = value.strip()

    @property
    def account_number(self):
        """Get account number (read-only)"""
        return self._account_number

    @property
    def balance(self):
        """Get current balance"""
        return self._balance

    @balance.setter
    def balance(self, value):
        """Set balance with validation (admin use only)"""
        if value < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = value

    @property
    def is_frozen(self):
        """Check if account is frozen"""
        return self._is_frozen

    def freeze_account(self):
        """Freeze the account"""
        self._is_frozen = True

    def unfreeze_account(self):
        """Unfreeze the account"""
        self._is_frozen = False

    def deposit(self, amount, description="Deposit"):
        """Deposit money into the account"""
        if self._is_frozen:
            raise RuntimeError("Cannot deposit into frozen account")

        if amount <= 0:
            raise ValueError("Deposit amount must be positive")

        self._balance += amount

        # Create transaction record
        transaction = Transaction(amount, description, transaction_type="credit")
        transaction.transaction_id = len(self._transactions) + 1
        self._transactions.append(transaction)

        return f"Deposited ${amount:.2f}. New balance: ${self._balance:.2f}"

    def withdraw(self, amount, description="Withdrawal"):
        """Withdraw money from the account"""
        if self._is_frozen:
            raise RuntimeError("Cannot withdraw from frozen account")

        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")

        if amount > self._balance:
            raise ValueError(f"Insufficient funds. Balance: ${self._balance:.2f}")

        self._balance -= amount

        # Create transaction record
        transaction = Transaction(amount, description, transaction_type="debit")
        transaction.transaction_id = len(self._transactions) + 1
        self._transactions.append(transaction)

        return f"Withdrew ${amount:.2f}. New balance: ${self._balance:.2f}"

    def transfer_to(self, other_account, amount, description="Transfer"):
        """Transfer money to another account"""
        if not isinstance(other_account, BankAccount):
            raise TypeError("Can only transfer to BankAccount objects")

        # Withdraw from this account
        self.withdraw(amount, f"Transfer to {other_account.owner_name}")

        # Deposit to other account
        other_account.deposit(amount, f"Transfer from {self.owner_name}")

        return f"Transferred ${amount:.2f} to {other_account.owner_name}"
```

**Key Points:**
- Property decorators control access to sensitive attributes
- Validation in setters prevents invalid data
- Methods include comprehensive error handling

### Step 3: Add Magic Methods to BankAccount

Implement special methods for enhanced functionality:

```python
# Add to BankAccount class in banking_system.py
def __str__(self):
    """String representation for users"""
    status = "FROZEN" if self._is_frozen else "ACTIVE"
    return f"Account {self._account_number}: {self._owner_name} (${self._balance:.2f}) [{status}]"

def __repr__(self):
    """Detailed representation for developers"""
    return f"BankAccount(owner='{self._owner_name}', number={self._account_number}, balance={self._balance:.2f})"

def __eq__(self, other):
    """Check equality by account number"""
    if not isinstance(other, BankAccount):
        return False
    return self._account_number == other._account_number

def __lt__(self, other):
    """Compare accounts by balance (for sorting)"""
    if not isinstance(other, BankAccount):
        return NotImplemented
    return self._balance < other._balance

def __add__(self, other):
    """Combine two accounts (create joint account)"""
    if not isinstance(other, BankAccount):
        return NotImplemented

    # Create new joint account
    joint_name = f"{self._owner_name} & {other._owner_name}"
    joint_balance = self._balance + other._balance

    joint_account = BankAccount(joint_name, joint_balance)

    # Transfer transaction history
    for transaction in self._transactions + other._transactions:
        joint_account._transactions.append(transaction)

    return joint_account

def __len__(self):
    """Return number of transactions"""
    return len(self._transactions)

def __getitem__(self, index):
    """Get transaction by index"""
    return self._transactions[index]

def __iter__(self):
    """Make account iterable (iterate through transactions)"""
    return iter(self._transactions)

def __bool__(self):
    """Account is truthy if it has a positive balance"""
    return self._balance > 0
```

**Key Points:**
- `__str__` for user-friendly display
- `__eq__` for account comparison
- `__add__` for combining accounts
- Container methods for transaction access

### Step 4: Add Class and Static Methods

Implement alternative constructors and utilities:

```python
# Add to BankAccount class
@classmethod
def from_dict(cls, account_dict):
    """Create account from dictionary"""
    return cls(
        owner_name=account_dict['owner_name'],
        initial_balance=account_dict.get('balance', 0.0)
    )

@classmethod
def get_account_by_number(cls, account_number):
    """Get account by account number"""
    return cls._accounts.get(account_number)

@classmethod
def get_all_accounts(cls):
    """Get all existing accounts"""
    return list(cls._accounts.values())

@classmethod
def get_total_bank_balance(cls):
    """Get total balance across all accounts"""
    return sum(account.balance for account in cls._accounts.values())

@staticmethod
def validate_amount(amount):
    """Validate transaction amount"""
    if not isinstance(amount, (int, float)):
        raise TypeError("Amount must be a number")
    if amount <= 0:
        raise ValueError("Amount must be positive")
    if amount > 1000000:  # Arbitrary large limit
        raise ValueError("Amount too large")
    return True

@staticmethod
def format_currency(amount):
    """Format amount as currency string"""
    return f"${amount:,.2f}"
```

**Key Points:**
- Class methods for account management
- Static methods for utility functions
- Alternative constructors using classmethods

### Step 5: Create the Bank Class

Add a Bank class to manage multiple accounts:

```python
# Add to banking_system.py
class Bank:
    """Bank that manages multiple accounts"""

    def __init__(self, name):
        self.name = name
        self.accounts = {}

    def open_account(self, owner_name, initial_balance=0.0):
        """Open a new account"""
        account = BankAccount(owner_name, initial_balance)
        self.accounts[account.account_number] = account
        return account

    def close_account(self, account_number):
        """Close an account (if balance is zero)"""
        if account_number not in self.accounts:
            raise ValueError("Account not found")

        account = self.accounts[account_number]
        if account.balance > 0:
            raise ValueError("Cannot close account with positive balance")

        del self.accounts[account_number]
        del BankAccount._accounts[account_number]  # Remove from global registry
        return f"Account {account_number} closed"

    def get_account(self, account_number):
        """Get account by number"""
        return self.accounts.get(account_number)

    def transfer_between_accounts(self, from_account_num, to_account_num, amount):
        """Transfer money between accounts in this bank"""
        from_account = self.get_account(from_account_num)
        to_account = self.get_account(to_account_num)

        if not from_account:
            raise ValueError(f"Source account {from_account_num} not found")
        if not to_account:
            raise ValueError(f"Destination account {to_account_num} not found")

        return from_account.transfer_to(to_account, amount)

    def get_bank_summary(self):
        """Get summary of all accounts in the bank"""
        total_accounts = len(self.accounts)
        total_balance = sum(account.balance for account in self.accounts.values())
        active_accounts = sum(1 for account in self.accounts.values() if not account.is_frozen)

        return {
            "bank_name": self.name,
            "total_accounts": total_accounts,
            "active_accounts": active_accounts,
            "frozen_accounts": total_accounts - active_accounts,
            "total_balance": total_balance
        }

    def __len__(self):
        return len(self.accounts)

    def __iter__(self):
        return iter(self.accounts.values())
```

**Key Points:**
- Manages multiple accounts
- Provides bank-level operations
- Implements container protocol

---

## 🧪 Testing Your Implementation

Create `test_banking.py` to verify your implementation:

```python
# test_banking.py
from banking_system import BankAccount, Bank, Transaction

def test_banking_system():
    print("🏦 Testing Banking System")
    print("=" * 50)

    # Test basic account operations
    account1 = BankAccount("Alice Johnson", 1000.0)
    account2 = BankAccount("Bob Smith", 500.0)

    print(f"Created: {account1}")
    print(f"Created: {account2}")
    print()

    # Test property decorators
    print("🛡️ Testing Property Validation:")
    try:
        account1.owner_name = ""  # Should fail
    except ValueError as e:
        print(f"✓ Name validation: {e}")

    try:
        account1.balance = -100  # Should fail
    except ValueError as e:
        print(f"✓ Balance validation: {e}")
    print()

    # Test magic methods
    print("🎩 Testing Magic Methods:")
    print(f"account1 == account2: {account1 == account2}")  # False
    print(f"account1 < account2: {account1 < account2}")    # False (1000 > 500)
    print(f"bool(account1): {bool(account1)}")             # True (positive balance)
    print()

    # Test transactions
    print("💸 Testing Transactions:")
    print(account1.deposit(250.50, "Salary deposit"))
    print(account1.withdraw(50.00, "Grocery shopping"))
    print(f"Account now has {len(account1)} transactions")
    print("Recent transactions:")
    for i, transaction in enumerate(account1, 1):
        print(f"  {i}. {transaction}")
    print()

    # Test transfer
    print("🔄 Testing Transfers:")
    print(account1.transfer_to(account2, 100.00, "Gift"))
    print(f"Alice's balance: ${account1.balance:.2f}")
    print(f"Bob's balance: ${account2.balance:.2f}")
    print()

    # Test class methods
    print("🏭 Testing Class Methods:")
    account_dict = {"owner_name": "Charlie Brown", "balance": 750.00}
    account3 = BankAccount.from_dict(account_dict)
    print(f"Created from dict: {account3}")

    print(f"Total accounts created: {len(BankAccount.get_all_accounts())}")
    print(f"Total bank balance: ${BankAccount.get_total_bank_balance():.2f}")
    print()

    # Test static methods
    print("🔧 Testing Static Methods:")
    try:
        BankAccount.validate_amount(100)
        print("✓ Amount validation passed")
    except ValueError as e:
        print(f"✗ Amount validation failed: {e}")

    print(f"Formatted currency: {BankAccount.format_currency(1234.56)}")
    print()

    # Test bank operations
    print("🏦 Testing Bank Operations:")
    my_bank = Bank("PyBank")

    # Open accounts through bank
    bank_acc1 = my_bank.open_account("Diana Prince", 2000.00)
    bank_acc2 = my_bank.open_account("Bruce Wayne", 50000.00)

    print(f"Bank has {len(my_bank)} accounts")

    # Bank transfer
    my_bank.transfer_between_accounts(bank_acc1.account_number,
                                     bank_acc2.account_number, 500.00)

    summary = my_bank.get_bank_summary()
    print(f"Bank Summary: {summary}")
    print()

    # Test account combination
    print("➕ Testing Account Combination:")
    joint_account = account1 + account2
    print(f"Joint account: {joint_account}")
    print(f"Joint balance: ${joint_account.balance:.2f}")

if __name__ == "__main__":
    test_banking_system()
```

**Expected Output:**
```
🏦 Testing Banking System
==================================================
Created: Account 1000: Alice Johnson ($1000.00) [ACTIVE]
Created: Account 1001: Bob Smith ($500.00) [ACTIVE]

🛡️ Testing Property Validation:
✓ Name validation: Owner name cannot be empty
✓ Balance validation: Balance cannot be negative

🎩 Testing Magic Methods:
account1 == account2: False
account1 < account2: False
bool(account1): True

💸 Testing Transactions:
Deposited $250.50. New balance: $1250.50
Withdrew $50.00. New balance: $1200.50
Account now has 2 transactions
Recent transactions:
  1. 2026-02-15 10:30:00 +$250.50 Salary deposit
  2. 2026-02-15 10:30:00 -$50.00 Grocery shopping

🔄 Testing Transfers:
Transferred $100.00 to Bob Smith
Alice's balance: $1100.50
Bob's balance: $600.00

🏭 Testing Class Methods:
Created from dict: Account 1002: Charlie Brown ($750.00) [ACTIVE]
Total accounts created: 3
Total bank balance: $2450.50

🔧 Testing Static Methods:
✓ Amount validation passed
Formatted currency: $1,234.56

🏦 Testing Bank Operations:
Bank has 2 accounts
Bank Summary: {'bank_name': 'PyBank', 'total_accounts': 2, 'active_accounts': 2, 'frozen_accounts': 0, 'total_balance': 52500.0}

➕ Testing Account Combination:
Joint account: Account 1003: Alice Johnson & Bob Smith ($1700.50) [ACTIVE]
Joint balance: $1700.50
```

---

## 🚀 Extension Challenges

### Challenge 1: Account Types
Create different account types using inheritance:
- `SavingsAccount` - earns interest, limited withdrawals
- `CheckingAccount` - no interest, unlimited transactions
- `CreditAccount` - allows negative balance up to limit

### Challenge 2: Transaction History
Enhance transaction management:
- Add transaction search by date range
- Implement transaction categories
- Add monthly statement generation

### Challenge 3: Advanced Banking Features
Implement additional features:
- Account freezing/unfreezing with reasons
- Multi-account transfers
- Scheduled transactions
- Account backup/restore functionality

---

## 🐛 Troubleshooting

### Common Issues:

1. **Property Setter Errors**
   ```
   AttributeError: can't set attribute
   ```
   **Solution**: Make sure properties have both getter and setter defined.

2. **Magic Method Conflicts**
   ```
   TypeError: unsupported operand type(s)
   ```
   **Solution**: Return `NotImplemented` for unsupported operations.

3. **Class Method Issues**
   ```
   TypeError: from_dict() missing 1 required positional argument: 'account_dict'
   ```
   **Solution**: Class methods receive `cls` as first argument, not `self`.

4. **Dataclass Validation**
   ```
   ValueError: Transaction amount must be positive
   ```
   **Solution**: Check `__post_init__` method for validation logic.

### Debug Tips:
- Use `print(type(obj))` to check object types
- Test individual methods before integration
- Use `hasattr(obj, 'method_name')` to check method availability

---

## ✅ Success Criteria

Your banking system implementation is complete when:

- ✅ Magic methods work for account comparisons and operations
- ✅ Property decorators validate input and control access
- ✅ Class methods create accounts from different sources
- ✅ Static methods provide utility functions
- ✅ Dataclasses manage transaction data effectively
- ✅ Bank class manages multiple accounts
- ✅ All tests pass with expected output

---

## 📚 Key Takeaways

1. **Magic Methods**: Enable objects to work with built-in Python operations
2. **Property Decorators**: Provide controlled access to object attributes
3. **Class Methods**: Work with class-level data and provide alternative constructors
4. **Static Methods**: Utility functions that don't need instance or class context
5. **Dataclasses**: Simplify data container creation with automatic method generation

---

## 🔗 Next Steps

In the homework assignment, you'll create a custom container class that implements sequence-like behavior using the advanced OOP concepts you've mastered. This will include implementing magic methods for indexing, iteration, and length operations!
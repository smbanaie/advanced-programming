# Workshop 1: Library Management System

**Section**: 1 - OOP Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 1 (OOP Fundamentals)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Apply OOP Concepts**: Use classes, objects, and methods in practice
2. **Design Class Relationships**: Create multiple classes that work together
3. **Implement CRUD Operations**: Build Create, Read, Update, Delete functionality
4. **Manage Object Collections**: Handle lists of objects and relationships
5. **Practice Encapsulation**: Control access to object data through methods

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Book Class Implementation](#exercise-1-book-class-implementation)
3. [Exercise 2: Member Class Design](#exercise-2-member-class-design)
4. [Exercise 3: Library System Integration](#exercise-3-library-system-integration)
5. [Exercise 4: Borrowing System](#exercise-4-borrowing-system)
6. [Exercise 5: Advanced Features](#exercise-5-advanced-features)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-library-system
cd workshop-library-system

# Create Python files
touch book.py member.py library.py main.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate
```

### Project Structure

```
workshop-library-system/
├── book.py          # Book class implementation
├── member.py        # Library member class
├── library.py       # Main library management system
├── main.py          # Demo and testing script
└── README.md        # Documentation (optional)
```

---

## 📖 Exercise 1: Book Class Implementation

**Goal**: Create a Book class with proper encapsulation and methods

### Task 1.1: Basic Book Class

Create `book.py` with a Book class that has the following features:

```python
class Book:
    """Represents a book in the library system."""

    def __init__(self, title, author, isbn, publication_year):
        """Initialize a book with its basic information."""
        # TODO: Initialize instance variables
        # - title, author, isbn, publication_year
        # - available (boolean, starts as True)
        # - borrower_id (None initially)

    def get_info(self):
        """Return formatted book information."""
        # TODO: Return a string with all book details

    def is_available(self):
        """Check if the book is available for borrowing."""
        # TODO: Return availability status

    def borrow_book(self, member_id):
        """Mark book as borrowed by a member."""
        # TODO: Set available to False and store member_id
        # TODO: Handle case where book is already borrowed

    def return_book(self):
        """Mark book as returned."""
        # TODO: Set available to True and clear borrower_id
        # TODO: Handle case where book is not borrowed
```

**Test your Book class:**
```python
# Create some books
book1 = Book("1984", "George Orwell", "978-0451524935", 1949)
book2 = Book("To Kill a Mockingbird", "Harper Lee", "978-0061120084", 1960)

print(book1.get_info())
print(book2.is_available())  # Should be True

# Test borrowing
book1.borrow_book("M001")
print(book1.is_available())  # Should be False

book1.return_book()
print(book1.is_available())  # Should be True
```

### Task 1.2: Book Collection Management

Add methods to manage collections of books:

```python
class Book:
    # ... (previous methods)

    @classmethod
    def create_sample_books(cls):
        """Create and return a list of sample books."""
        # TODO: Create 5-6 sample books
        # TODO: Return list of Book objects

    @staticmethod
    def validate_isbn(isbn):
        """Validate ISBN format (basic check)."""
        # TODO: Check if ISBN is a valid string format
        # TODO: Return True/False
```

**Test your collection methods:**
```python
# Test sample books creation
books = Book.create_sample_books()
print(f"Created {len(books)} sample books")

for book in books[:2]:  # Show first 2 books
    print(book.get_info())

# Test ISBN validation
print(Book.validate_isbn("978-0451524935"))  # Should be True
print(Book.validate_isbn("invalid"))         # Should be False
```

---

## 👥 Exercise 2: Member Class Design

**Goal**: Create a Member class to represent library patrons

### Task 2.1: Basic Member Class

Create `member.py` with a Member class:

```python
class Member:
    """Represents a library member."""

    # Class variable to track total members
    total_members = 0

    def __init__(self, member_id, name, email):
        """Initialize a library member."""
        # TODO: Initialize instance variables
        # - member_id, name, email
        # - borrowed_books (empty list)
        # - join_date (current date/time)
        # Increment total_members class variable

    def get_info(self):
        """Return formatted member information."""
        # TODO: Return string with member details and borrowed book count

    def borrow_book(self, book):
        """Add a book to member's borrowed list."""
        # TODO: Add book to borrowed_books list
        # TODO: Update book's borrower_id
        # TODO: Handle max books limit (e.g., 3 books)

    def return_book(self, book):
        """Remove a book from member's borrowed list."""
        # TODO: Remove book from borrowed_books list
        # TODO: Clear book's borrower_id

    def get_borrowed_books(self):
        """Return list of currently borrowed books."""
        # TODO: Return copy of borrowed_books list

    def can_borrow_more(self, max_books=3):
        """Check if member can borrow more books."""
        # TODO: Check current borrowed count vs max_books
```

**Test your Member class:**
```python
# Create members
member1 = Member("M001", "Alice Johnson", "alice@email.com")
member2 = Member("M002", "Bob Smith", "bob@email.com")

print(f"Total members: {Member.total_members}")  # Should be 2

print(member1.get_info())
print(member2.can_borrow_more())  # Should be True

# Test borrowing limit
books = Book.create_sample_books()
member1.borrow_book(books[0])
member1.borrow_book(books[1])
member1.borrow_book(books[2])
print(member1.can_borrow_more())  # Should be False (at limit)

member1.borrow_book(books[3])  # Should not work
```

### Task 2.2: Member Search and Filtering

Add search capabilities to the Member class:

```python
class Member:
    # ... (previous methods)

    @classmethod
    def find_by_id(cls, members_list, member_id):
        """Find member by ID in a list of members."""
        # TODO: Search through members_list
        # TODO: Return member or None

    @classmethod
    def find_by_name(cls, members_list, name):
        """Find members by name (partial match)."""
        # TODO: Return list of matching members

    def to_dict(self):
        """Convert member to dictionary for serialization."""
        # TODO: Return dictionary representation
```

**Test your search methods:**
```python
# Create member list
members = [
    Member("M001", "Alice Johnson", "alice@email.com"),
    Member("M002", "Bob Smith", "bob@email.com"),
    Member("M003", "Alice Brown", "alice.brown@email.com")
]

# Test search
found_member = Member.find_by_id(members, "M002")
print(found_member.get_info() if found_member else "Not found")

alice_members = Member.find_by_name(members, "Alice")
print(f"Found {len(alice_members)} Alice members")

# Test serialization
member_dict = members[0].to_dict()
print(member_dict)
```

---

## 🏛️ Exercise 3: Library System Integration

**Goal**: Create a Library class that manages books and members

### Task 3.1: Basic Library Class

Create `library.py` with a Library class:

```python
from book import Book
from member import Member

class Library:
    """Main library management system."""

    def __init__(self, name):
        """Initialize library with name."""
        # TODO: Initialize instance variables
        # - name, books (empty list), members (empty list)

    def add_book(self, book):
        """Add a book to the library collection."""
        # TODO: Add book to books list
        # TODO: Validate book is Book instance

    def add_member(self, member):
        """Add a member to the library."""
        # TODO: Add member to members list
        # TODO: Validate member is Member instance

    def get_available_books(self):
        """Return list of available books."""
        # TODO: Filter books where is_available() is True

    def get_borrowed_books(self):
        """Return list of borrowed books."""
        # TODO: Filter books where is_available() is False

    def find_book_by_title(self, title):
        """Find books by title (partial match)."""
        # TODO: Return list of matching books

    def find_member_by_id(self, member_id):
        """Find member by ID."""
        # TODO: Use Member.find_by_id method
```

**Test your Library class:**
```python
# Create library
library = Library("City Central Library")

# Add sample data
books = Book.create_sample_books()
members = [
    Member("M001", "Alice Johnson", "alice@email.com"),
    Member("M002", "Bob Smith", "bob@email.com")
]

for book in books:
    library.add_book(book)

for member in members:
    library.add_member(member)

print(f"Library: {library.name}")
print(f"Total books: {len(library.books)}")
print(f"Available books: {len(library.get_available_books())}")
print(f"Total members: {len(library.members)}")
```

### Task 3.2: Library Statistics

Add statistical methods to the Library class:

```python
class Library:
    # ... (previous methods)

    def get_library_stats(self):
        """Return comprehensive library statistics."""
        # TODO: Return dictionary with stats:
        # - total_books, available_books, borrowed_books
        # - total_members, active_members (with borrowed books)
        # - popular_books (most borrowed - for future extension)

    def generate_report(self):
        """Generate a text report of library status."""
        # TODO: Create formatted text report
        # TODO: Include stats and current status
```

**Test your statistics methods:**
```python
# Get and display statistics
stats = library.get_library_stats()
print("Library Statistics:")
for key, value in stats.items():
    print(f"  {key}: {value}")

# Generate report
report = library.generate_report()
print("\nLibrary Report:")
print(report)
```

---

## 📚 Exercise 4: Borrowing System

**Goal**: Implement the complete book borrowing workflow

### Task 4.1: Borrowing Operations

Add borrowing methods to the Library class:

```python
class Library:
    # ... (previous methods)

    def borrow_book(self, member_id, book_title):
        """
        Allow a member to borrow a book.

        Args:
            member_id (str): Member's ID
            book_title (str): Book title to borrow

        Returns:
            bool: True if successful, False otherwise
        """
        # TODO: Find member by ID
        # TODO: Find available book by title
        # TODO: Check if member can borrow more books
        # TODO: Process the borrowing (update book and member)
        # TODO: Return success status

    def return_book(self, member_id, book_title):
        """
        Allow a member to return a book.

        Args:
            member_id (str): Member's ID
            book_title (str): Book title to return

        Returns:
            bool: True if successful, False otherwise
        """
        # TODO: Find member by ID
        # TODO: Find book by title in member's borrowed books
        # TODO: Process the return (update book and member)
        # TODO: Return success status
```

**Test your borrowing system:**
```python
# Test borrowing
success = library.borrow_book("M001", "1984")
print(f"Borrowing successful: {success}")

success = library.borrow_book("M001", "To Kill a Mockingbird")
print(f"Second borrowing successful: {success}")

# Check status
stats = library.get_library_stats()
print(f"Available books: {stats['available_books']}")

# Test return
success = library.return_book("M001", "1984")
print(f"Return successful: {success}")

stats = library.get_library_stats()
print(f"Available books after return: {stats['available_books']}")
```

### Task 4.2: Error Handling and Validation

Improve the borrowing system with better error handling:

```python
class LibraryError(Exception):
    """Custom exception for library operations."""
    pass

class Library:
    # ... (previous methods)

    def borrow_book_safe(self, member_id, book_title):
        """
        Safe book borrowing with comprehensive error handling.

        Args:
            member_id (str): Member's ID
            book_title (str): Book title to borrow

        Returns:
            str: Success message

        Raises:
            LibraryError: If borrowing fails
        """
        # TODO: Implement with try/catch and detailed error messages
        # TODO: Check member exists
        # TODO: Check book exists and is available
        # TODO: Check borrowing limits
        # TODO: Process borrowing
        # TODO: Return success message

    def return_book_safe(self, member_id, book_title):
        """
        Safe book return with error handling.

        Args:
            member_id (str): Member's ID
            book_title (str): Book title to return

        Returns:
            str: Success message

        Raises:
            LibraryError: If return fails
        """
        # TODO: Implement safe return with error handling
```

**Test your error handling:**
```python
# Test various error conditions
try:
    library.borrow_book_safe("M001", "Non-existent Book")
except LibraryError as e:
    print(f"Borrowing failed: {e}")

try:
    library.return_book_safe("M999", "1984")  # Non-existent member
except LibraryError as e:
    print(f"Return failed: {e}")

# Test successful operations
try:
    message = library.borrow_book_safe("M001", "Pride and Prejudice")
    print(f"Success: {message}")
except LibraryError as e:
    print(f"Unexpected error: {e}")
```

---

## 🚀 Exercise 5: Advanced Features

**Goal**: Add advanced features to make the system more complete

### Task 5.1: Search and Filter System

Add search capabilities to the Library class:

```python
class Library:
    # ... (previous methods)

    def search_books(self, query, search_by="title"):
        """
        Search books by various criteria.

        Args:
            query (str): Search query
            search_by (str): Search field ("title", "author", "isbn")

        Returns:
            list: Matching books
        """
        # TODO: Implement search by different fields
        # TODO: Support partial matches

    def get_overdue_books(self, max_borrow_days=14):
        """
        Get list of overdue books (for future enhancement).

        Args:
            max_borrow_days (int): Maximum borrowing period

        Returns:
            list: Overdue books (empty for now)
        """
        # TODO: This would require adding borrow dates to track time
        # TODO: Return empty list for now
        return []

    def generate_member_report(self, member_id):
        """
        Generate detailed report for a member.

        Args:
            member_id (str): Member's ID

        Returns:
            str: Formatted member report
        """
        # TODO: Find member
        # TODO: Create detailed report with borrowing history
```

**Test your advanced features:**
```python
# Test search
python_books = library.search_books("Python", "title")
print(f"Found {len(python_books)} Python books")

orwell_books = library.search_books("Orwell", "author")
print(f"Found {len(orwell_books)} Orwell books")

# Test member report
try:
    report = library.generate_member_report("M001")
    print("Member Report:")
    print(report)
except Exception as e:
    print(f"Report generation failed: {e}")
```

### Task 5.2: Data Persistence

Add basic data saving/loading capabilities:

```python
import json
import os

class Library:
    # ... (previous methods)

    def save_data(self, filename="library_data.json"):
        """Save library data to JSON file."""
        # TODO: Convert books and members to dictionaries
        # TODO: Save to JSON file

    def load_data(self, filename="library_data.json"):
        """Load library data from JSON file."""
        # TODO: Load from JSON file
        # TODO: Recreate Book and Member objects
        # TODO: Handle file not found gracefully
```

**Test your data persistence:**
```python
# Save library state
library.save_data("test_library.json")

# Create new library and load data
new_library = Library("Test Library")
new_library.load_data("test_library.json")

print(f"Loaded library has {len(new_library.books)} books")
print(f"Loaded library has {len(new_library.members)} members")
```

---

## 🏆 Challenge Exercises

### Challenge 1: Book Categories

Add book categorization system:

```python
class Book:
    # Add category support
    def __init__(self, title, author, isbn, publication_year, category="General"):
        # TODO: Add category parameter

class Library:
    def get_books_by_category(self, category):
        """Return books in a specific category."""
        # TODO: Implement category filtering

    def get_category_stats(self):
        """Return statistics by category."""
        # TODO: Count books per category
```

### Challenge 2: Borrowing Limits and Due Dates

Implement borrowing limits and due date tracking:

```python
from datetime import datetime, timedelta

class Book:
    # Add borrowing date tracking
    def borrow_book(self, member_id, borrow_date=None):
        # TODO: Store borrow date

class Member:
    # Add due date checking
    def get_overdue_books(self):
        # TODO: Check which books are overdue

class Library:
    def get_overdue_books(self, current_date=None):
        # TODO: Find all overdue books
```

---

## ✅ Solution Code

### Book Class Solution

```python
class Book:
    """Represents a book in the library system."""

    def __init__(self, title, author, isbn, publication_year):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.publication_year = publication_year
        self.available = True
        self.borrower_id = None

    def get_info(self):
        status = "Available" if self.available else f"Borrowed by {self.borrower_id}"
        return f"'{self.title}' by {self.author} ({self.publication_year}) - {status}"

    def is_available(self):
        return self.available

    def borrow_book(self, member_id):
        if self.available:
            self.available = False
            self.borrower_id = member_id
            return True
        return False

    def return_book(self):
        if not self.available:
            self.available = True
            self.borrower_id = None
            return True
        return False

    @classmethod
    def create_sample_books(cls):
        books_data = [
            ("1984", "George Orwell", "978-0451524935", 1949),
            ("To Kill a Mockingbird", "Harper Lee", "978-0061120084", 1960),
            ("Pride and Prejudice", "Jane Austen", "978-0486284736", 1813),
            ("The Great Gatsby", "F. Scott Fitzgerald", "978-0743273565", 1925),
            ("Harry Potter", "J.K. Rowling", "978-0439708180", 1997)
        ]
        return [cls(*data) for data in books_data]

    @staticmethod
    def validate_isbn(isbn):
        return isinstance(isbn, str) and len(isbn.strip()) > 0
```

### Member Class Solution

```python
from datetime import datetime

class Member:
    """Represents a library member."""

    total_members = 0

    def __init__(self, member_id, name, email):
        self.member_id = member_id
        self.name = name
        self.email = email
        self.borrowed_books = []
        self.join_date = datetime.now()
        Member.total_members += 1

    def get_info(self):
        return f"{self.name} ({self.email}) - {len(self.borrowed_books)} books borrowed"

    def borrow_book(self, book):
        if len(self.borrowed_books) < 3 and book.is_available():
            if book.borrow_book(self.member_id):
                self.borrowed_books.append(book)
                return True
        return False

    def return_book(self, book):
        if book in self.borrowed_books:
            if book.return_book():
                self.borrowed_books.remove(book)
                return True
        return False

    def get_borrowed_books(self):
        return self.borrowed_books.copy()

    def can_borrow_more(self, max_books=3):
        return len(self.borrowed_books) < max_books

    @classmethod
    def find_by_id(cls, members_list, member_id):
        return next((member for member in members_list if member.member_id == member_id), None)

    @classmethod
    def find_by_name(cls, members_list, name):
        return [member for member in members_list if name.lower() in member.name.lower()]

    def to_dict(self):
        return {
            "member_id": self.member_id,
            "name": self.name,
            "email": self.email,
            "borrowed_books_count": len(self.borrowed_books),
            "join_date": self.join_date.isoformat()
        }
```

### Library Class Solution

```python
from book import Book
from member import Member

class LibraryError(Exception):
    """Custom exception for library operations."""
    pass

class Library:
    """Main library management system."""

    def __init__(self, name):
        self.name = name
        self.books = []
        self.members = []

    def add_book(self, book):
        if isinstance(book, Book):
            self.books.append(book)
        else:
            raise ValueError("Only Book objects can be added")

    def add_member(self, member):
        if isinstance(member, Member):
            self.members.append(member)
        else:
            raise ValueError("Only Member objects can be added")

    def get_available_books(self):
        return [book for book in self.books if book.is_available()]

    def get_borrowed_books(self):
        return [book for book in self.books if not book.is_available()]

    def find_book_by_title(self, title):
        return [book for book in self.books if title.lower() in book.title.lower()]

    def find_member_by_id(self, member_id):
        return Member.find_by_id(self.members, member_id)

    def borrow_book(self, member_id, book_title):
        member = self.find_member_by_id(member_id)
        if not member:
            return False

        available_books = self.get_available_books()
        matching_books = [book for book in available_books if book_title.lower() in book.title.lower()]

        if not matching_books:
            return False

        book = matching_books[0]  # Take first match
        return member.borrow_book(book)

    def return_book(self, member_id, book_title):
        member = self.find_member_by_id(member_id)
        if not member:
            return False

        for book in member.get_borrowed_books():
            if book_title.lower() in book.title.lower():
                return member.return_book(book)
        return False

    def borrow_book_safe(self, member_id, book_title):
        member = self.find_member_by_id(member_id)
        if not member:
            raise LibraryError(f"Member {member_id} not found")

        available_books = self.get_available_books()
        matching_books = [book for book in available_books if book_title.lower() in book.title.lower()]

        if not matching_books:
            raise LibraryError(f"Book '{book_title}' not available")

        if not member.can_borrow_more():
            raise LibraryError(f"Member {member_id} has reached borrowing limit")

        book = matching_books[0]
        if member.borrow_book(book):
            return f"Successfully borrowed '{book.title}'"
        else:
            raise LibraryError("Borrowing failed")

    def return_book_safe(self, member_id, book_title):
        member = self.find_member_by_id(member_id)
        if not member:
            raise LibraryError(f"Member {member_id} not found")

        for book in member.get_borrowed_books():
            if book_title.lower() in book.title.lower():
                if member.return_book(book):
                    return f"Successfully returned '{book.title}'"
                else:
                    raise LibraryError("Return failed")

        raise LibraryError(f"Book '{book_title}' not found in member's borrowed books")

    def get_library_stats(self):
        return {
            "total_books": len(self.books),
            "available_books": len(self.get_available_books()),
            "borrowed_books": len(self.get_borrowed_books()),
            "total_members": len(self.members),
            "active_members": len([m for m in self.members if len(m.get_borrowed_books()) > 0])
        }

    def generate_report(self):
        stats = self.get_library_stats()
        report = f"""
Library Report: {self.name}
{'='*50}
Books:
  Total: {stats['total_books']}
  Available: {stats['available_books']}
  Borrowed: {stats['borrowed_books']}

Members:
  Total: {stats['total_members']}
  Active: {stats['active_members']}

Available Books:
"""
        for book in self.get_available_books()[:5]:  # Show first 5
            report += f"  - {book.title} by {book.author}\n"

        if len(self.get_available_books()) > 5:
            report += f"  ... and {len(self.get_available_books()) - 5} more\n"

        return report.strip()
```

---

## 🧪 Testing Your Solutions

Create a comprehensive test script:

```python
# test_library_system.py
from book import Book
from member import Member
from library import Library, LibraryError

def test_basic_functionality():
    """Test basic library operations."""
    # Create library
    library = Library("Test Library")

    # Add books and members
    books = Book.create_sample_books()
    for book in books:
        library.add_book(book)

    members = [
        Member("M001", "Alice", "alice@test.com"),
        Member("M002", "Bob", "bob@test.com")
    ]
    for member in members:
        library.add_member(member)

    # Test borrowing
    success = library.borrow_book_safe("M001", "1984")
    print(f"Borrowing test: {success}")

    # Test return
    success = library.return_book_safe("M001", "1984")
    print(f"Return test: {success}")

    # Test error handling
    try:
        library.borrow_book_safe("M999", "1984")  # Invalid member
    except LibraryError as e:
        print(f"Error handling test: {e}")

    # Show stats
    stats = library.get_library_stats()
    print(f"Library stats: {stats}")

    print("✓ Basic functionality tests passed")

if __name__ == "__main__":
    test_basic_functionality()
    print("🎉 All tests passed!")
```

---

## 📝 Key Takeaways

1. **Class Design**: Well-designed classes encapsulate related data and behavior
2. **Object Relationships**: Objects can reference and interact with each other
3. **Error Handling**: Proper validation and exception handling prevent system failures
4. **Collection Management**: Classes can manage collections of other objects
5. **Method Design**: Methods should have clear responsibilities and return meaningful values
6. **Encapsulation**: Control access to internal data through public methods

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
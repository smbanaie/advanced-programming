# Workshop 1: Adding Logging to Existing Apps

**Section**: 1 - Python Logging Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 1 (Python Logging Fundamentals)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Apply Logging Configuration**: Set up logging systems in existing code
2. **Replace Print Statements**: Convert debugging prints to proper logging
3. **Implement Multiple Handlers**: Configure console and file logging
4. **Use Appropriate Log Levels**: Apply correct levels for different message types
5. **Debug with Logging**: Use logging to identify and fix application issues
6. **Create Modular Logging**: Implement logging across multiple modules

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Basic Logging Setup](#exercise-1-basic-logging-setup)
3. [Exercise 2: Converting Print to Logging](#exercise-2-converting-print-to-logging)
4. [Exercise 3: Multi-Handler Configuration](#exercise-3-multi-handler-configuration)
5. [Exercise 4: Exception Logging](#exercise-4-exception-logging)
6. [Exercise 5: Modular Logging System](#exercise-5-modular-logging-system)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-logging-basics
cd workshop-logging-basics

# Create Python files
touch user_manager.py test_user_manager.py
touch calculator.py test_calculator.py
touch web_scraper.py test_web_scraper.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate
```

### Project Structure

```
workshop-logging-basics/
├── user_manager.py         # User management with print statements
├── test_user_manager.py    # Logging implementation
├── calculator.py           # Calculator with debug prints
├── test_calculator.py      # Calculator logging
├── web_scraper.py          # Web scraper with error prints
├── test_web_scraper.py     # Scraper logging
└── README.md              # Documentation (optional)
```

---

## 🧮 Exercise 1: Basic Logging Setup

**Goal**: Set up basic logging configuration and understand log levels

### Code to Enhance (user_manager.py)

```python
"""User management system with basic functionality."""

class UserManager:
    """Manages user accounts and authentication."""

    def __init__(self):
        self.users = {}  # username -> user_data

    def create_user(self, username, email, password):
        """Create a new user account."""
        if username in self.users:
            print(f"ERROR: User {username} already exists")
            return False

        if not self._validate_email(email):
            print(f"ERROR: Invalid email format: {email}")
            return False

        # Create user
        user_data = {
            'email': email,
            'password': password,  # In real app, hash this!
            'created_at': '2023-12-07'
        }
        self.users[username] = user_data
        print(f"SUCCESS: User {username} created")
        return True

    def authenticate_user(self, username, password):
        """Authenticate a user."""
        if username not in self.users:
            print(f"ERROR: User {username} not found")
            return False

        user_data = self.users[username]
        if user_data['password'] != password:
            print(f"ERROR: Invalid password for user {username}")
            return False

        print(f"SUCCESS: User {username} authenticated")
        return True

    def get_user_info(self, username):
        """Get user information."""
        if username not in self.users:
            print(f"ERROR: User {username} not found")
            return None

        user_data = self.users[username].copy()
        del user_data['password']  # Don't expose password
        print(f"INFO: Retrieved info for user {username}")
        return user_data

    def _validate_email(self, email):
        """Validate email format."""
        return '@' in email and '.' in email

# Test the class
if __name__ == '__main__':
    manager = UserManager()

    # Create users
    manager.create_user("alice", "alice@example.com", "pass123")
    manager.create_user("bob", "bob@example.com", "pass456")
    manager.create_user("alice", "duplicate@example.com", "pass789")  # Should fail

    # Authenticate
    manager.authenticate_user("alice", "pass123")
    manager.authenticate_user("alice", "wrongpass")

    # Get info
    info = manager.get_user_info("alice")
    print(f"User info: {info}")
```

### Adding Basic Logging (test_user_manager.py)

Create comprehensive logging setup to replace print statements:

```python
import logging
import logging.config
from user_manager import UserManager

# TODO: Configure logging with:
# - Console handler for INFO and above
# - File handler for DEBUG and above
# - Proper formatting
# - Logger named after the module

# TODO: Replace all print statements in UserManager with appropriate logging calls:
# - ERROR messages -> logging.error()
# - SUCCESS messages -> logging.info()
# - INFO messages -> logging.info()

# TODO: Add debug logging for method entry/exit and important variable values

# TODO: Test the logging by running the user manager operations

if __name__ == '__main__':
    # Test the enhanced user manager
    manager = UserManager()

    # These operations should now generate proper log messages
    manager.create_user("alice", "alice@example.com", "pass123")
    manager.create_user("bob", "bob@example.com", "pass456")
    manager.create_user("alice", "duplicate@example.com", "pass789")

    manager.authenticate_user("alice", "pass123")
    manager.authenticate_user("alice", "wrongpass")

    info = manager.get_user_info("alice")
    info = manager.get_user_info("nonexistent")
```

**Expected Log Output:**
```
2023-12-07 10:30:15 - INFO - User alice created successfully
2023-12-07 10:30:15 - INFO - User bob created successfully
2023-12-07 10:30:15 - ERROR - User alice already exists
2023-12-07 10:30:15 - INFO - User alice authenticated
2023-12-07 10:30:15 - ERROR - Invalid password for alice
2023-12-07 10:30:15 - INFO - Retrieved info for alice
2023-12-07 10:30:15 - ERROR - User nonexistent not found
```

---

## 🧪 Exercise 2: Converting Print to Logging

**Goal**: Replace print statements with proper logging in a calculator application

### Code to Enhance (calculator.py)

```python
"""Calculator class with various operations."""

class Calculator:
    """A calculator with basic and advanced operations."""

    def __init__(self):
        self.history = []
        print("Calculator initialized")

    def add(self, a, b):
        print(f"Adding {a} + {b}")
        result = a + b
        self.history.append(f"add({a}, {b}) = {result}")
        print(f"Result: {result}")
        return result

    def subtract(self, a, b):
        print(f"Subtracting {a} - {b}")
        result = a - b
        self.history.append(f"subtract({a}, {b}) = {result}")
        print(f"Result: {result}")
        return result

    def multiply(self, a, b):
        print(f"Multiplying {a} * {b}")
        result = a * b
        self.history.append(f"multiply({a}, {b}) = {result}")
        print(f"Result: {result}")
        return result

    def divide(self, a, b):
        print(f"Dividing {a} / {b}")
        if b == 0:
            print("ERROR: Division by zero!")
            return None
        result = a / b
        self.history.append(f"divide({a}, {b}) = {result}")
        print(f"Result: {result}")
        return result

    def power(self, base, exponent):
        print(f"Calculating {base} ^ {exponent}")
        result = base ** exponent
        self.history.append(f"power({base}, {exponent}) = {result}")
        print(f"Result: {result}")
        return result

    def get_history(self):
        print(f"Returning {len(self.history)} history entries")
        return self.history.copy()

    def clear_history(self):
        print("Clearing calculation history")
        self.history.clear()

# Test the calculator
if __name__ == '__main__':
    calc = Calculator()

    calc.add(5, 3)
    calc.subtract(10, 4)
    calc.multiply(6, 7)
    calc.divide(15, 3)
    calc.divide(10, 0)  # Error case
    calc.power(2, 3)

    history = calc.get_history()
    print(f"History: {history}")

    calc.clear_history()
    history = calc.get_history()
    print(f"History after clear: {history}")
```

### Converting to Logging (test_calculator.py)

Replace all print statements with appropriate logging calls:

```python
import logging
import logging.config
from calculator import Calculator

# TODO: Set up logging configuration with:
# - Console handler for WARNING and above (errors and important messages)
# - File handler for DEBUG and above (detailed operation logs)
# - Appropriate formatters for each handler

# TODO: Replace print statements in Calculator class:
# - Initialization message -> logging.info()
# - Operation details -> logging.debug()
# - Results -> logging.debug()
# - History operations -> logging.info()
# - Errors -> logging.error()

# TODO: Add additional logging for:
# - Method entry/exit points
# - Parameter validation
# - Performance information (if applicable)

# TODO: Test the calculator with various operations to verify logging

if __name__ == '__main__':
    # Test with different log levels to see different outputs
    calc = Calculator()

    # These operations should generate appropriate log messages
    calc.add(5, 3)
    calc.subtract(10, 4)
    calc.multiply(6, 7)
    calc.divide(15, 3)
    calc.divide(10, 0)  # Should log error
    calc.power(2, 3)

    history = calc.get_history()
    calc.clear_history()
```

**Expected Behavior:**
- Console shows warnings/errors and important operations
- File shows all debug information and operation details
- No print statements remain in the code

---

## 🎯 Exercise 3: Multi-Handler Configuration

**Goal**: Set up logging with multiple handlers and formatters

### Code to Enhance (web_scraper.py)

```python
"""Simple web scraper with error handling."""

import requests
import time

class WebScraper:
    """A simple web scraper for demonstration."""

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'WebScraper/1.0'
        })
        print("Web scraper initialized")

    def fetch_page(self, url):
        """Fetch a web page."""
        print(f"Fetching URL: {url}")
        try:
            response = self.session.get(url, timeout=10)
            print(f"Response status: {response.status_code}")
            if response.status_code == 200:
                print(f"Successfully fetched {len(response.text)} characters")
                return response.text
            else:
                print(f"ERROR: HTTP {response.status_code}")
                return None
        except requests.exceptions.Timeout:
            print(f"ERROR: Timeout fetching {url}")
            return None
        except requests.exceptions.ConnectionError:
            print(f"ERROR: Connection error for {url}")
            return None
        except Exception as e:
            print(f"ERROR: Unexpected error: {e}")
            return None

    def extract_title(self, html):
        """Extract title from HTML."""
        print("Extracting title from HTML")
        try:
            # Simple title extraction
            start = html.find('<title>')
            end = html.find('</title>')
            if start != -1 and end != -1:
                title = html[start+7:end].strip()
                print(f"Extracted title: '{title}'")
                return title
            else:
                print("No title found")
                return None
        except Exception as e:
            print(f"ERROR extracting title: {e}")
            return None

    def scrape_multiple_pages(self, urls):
        """Scrape multiple pages with delay."""
        print(f"Starting batch scrape of {len(urls)} URLs")
        results = {}

        for i, url in enumerate(urls, 1):
            print(f"Processing {i}/{len(urls)}: {url}")
            content = self.fetch_page(url)
            if content:
                title = self.extract_title(content)
                results[url] = title
            else:
                results[url] = None

            # Be nice to servers
            time.sleep(1)

        print(f"Batch scrape completed. {len(results)} results.")
        return results

# Test the scraper
if __name__ == '__main__':
    scraper = WebScraper()

    # Test single page
    content = scraper.fetch_page("https://httpbin.org/html")
    if content:
        title = scraper.extract_title(content)
        print(f"Page title: {title}")

    # Test multiple pages
    urls = [
        "https://httpbin.org/html",
        "https://httpbin.org/status/404",  # Should fail
        "https://httpbin.org/delay/5",     # Should timeout
    ]
    results = scraper.scrape_multiple_pages(urls)
    print(f"Results: {results}")
```

### Implementing Multi-Handler Logging (test_web_scraper.py)

Create a comprehensive logging setup with multiple handlers:

```python
import logging
import logging.config
from web_scraper import WebScraper

# TODO: Create logging configuration with:
# - Console handler: INFO and above, simple format
# - File handler: DEBUG and above, detailed format with timestamps
# - Error file handler: ERROR and above only, for error tracking
# - Rotating file handler for the main log file

LOGGING_CONFIG = {
    # TODO: Complete the logging configuration dictionary
    # Include formatters, handlers, and loggers
    # Set up appropriate levels and destinations
}

# TODO: Apply the logging configuration

# TODO: Replace print statements in WebScraper with logging:
# - Initialization -> logging.info()
# - URL fetching details -> logging.debug()
# - Success messages -> logging.info()
# - HTTP errors -> logging.warning()
# - Network errors -> logging.error()
# - Unexpected errors -> logging.error()

# TODO: Add additional logging for:
# - Method entry/exit with parameters
# - Response times
# - Data sizes
# - Error details

# TODO: Test the scraper with various scenarios

if __name__ == '__main__':
    # Configure logging
    logging.config.dictConfig(LOGGING_CONFIG)

    scraper = WebScraper()

    # Test single page fetch
    content = scraper.fetch_page("https://httpbin.org/html")
    if content:
        title = scraper.extract_title(content)

    # Test error scenarios
    scraper.fetch_page("https://httpbin.org/status/404")
    scraper.fetch_page("https://httpbin.org/delay/15")  # Timeout

    # Test batch processing
    urls = [
        "https://httpbin.org/html",
        "https://httpbin.org/json"
    ]
    results = scraper.scrape_multiple_pages(urls)
```

**Expected Log Output Structure:**
```
# Console (INFO+):
INFO: Web scraper initialized
INFO: Successfully fetched 1234 characters
WARNING: HTTP 404 for URL: https://httpbin.org/status/404

# Main log file (DEBUG+):
2023-12-07 10:30:15 - web_scraper - DEBUG - fetch_page:15 - Fetching URL: https://httpbin.org/html
2023-12-07 10:30:15 - web_scraper - DEBUG - fetch_page:18 - Response status: 200

# Error log file (ERROR+):
2023-12-07 10:30:20 - web_scraper - ERROR - fetch_page:25 - Connection timeout for https://httpbin.org/delay/15
```

---

## ⚠️ Exercise 4: Exception Logging

**Goal**: Implement proper exception logging and error handling

### Enhanced Code with Exceptions

Create a new version of a function that raises exceptions and logs them properly:

```python
"""Data processor with exception handling."""

class DataProcessor:
    """Processes various types of data."""

    def __init__(self):
        self.processed_items = 0
        print("Data processor initialized")

    def process_number(self, value):
        """Process a numeric value."""
        print(f"Processing number: {value}")
        try:
            # Convert to float and validate
            num = float(value)
            if num < 0:
                raise ValueError("Negative numbers not allowed")
            if num > 1000:
                raise ValueError("Number too large")

            result = num * 2
            print(f"Processed number: {result}")
            self.processed_items += 1
            return result
        except ValueError as e:
            print(f"ERROR: Invalid number {value} - {e}")
            raise
        except Exception as e:
            print(f"ERROR: Unexpected error processing {value} - {e}")
            raise

    def process_list(self, data):
        """Process a list of values."""
        print(f"Processing list with {len(data)} items")
        results = []

        for i, item in enumerate(data):
            print(f"Processing item {i}: {item}")
            try:
                result = self.process_number(item)
                results.append(result)
            except ValueError as e:
                print(f"Skipping invalid item {item}: {e}")
                continue
            except Exception as e:
                print(f"ERROR processing item {item}: {e}")
                raise

        print(f"Successfully processed {len(results)} items")
        return results

    def process_file(self, filename):
        """Process data from a file."""
        print(f"Processing file: {filename}")
        try:
            with open(filename, 'r') as f:
                data = f.read().strip().split(',')
                print(f"Read {len(data)} values from file")
                return self.process_list(data)
        except FileNotFoundError:
            print(f"ERROR: File {filename} not found")
            raise
        except PermissionError:
            print(f"ERROR: Permission denied reading {filename}")
            raise
        except Exception as e:
            print(f"ERROR: Unexpected error reading {filename}: {e}")
            raise

# Test the processor
if __name__ == '__main__':
    processor = DataProcessor()

    # Test number processing
    try:
        processor.process_number("10")
        processor.process_number("-5")  # Should fail
    except ValueError:
        pass

    # Test list processing
    data = ["5", "15", "abc", "25"]
    try:
        results = processor.process_list(data)
        print(f"Final results: {results}")
    except Exception as e:
        print(f"List processing failed: {e}")

    # Test file processing
    try:
        results = processor.process_file("numbers.txt")
        print(f"File results: {results}")
    except Exception as e:
        print(f"File processing failed: {e}")
```

### Implementing Exception Logging

Create proper exception logging to replace print statements:

```python
import logging
import traceback
from data_processor import DataProcessor

# TODO: Set up logging with:
# - Console handler for INFO and above
# - File handler for DEBUG and above
# - Separate error log file for ERROR and above
# - Include exception information in error logs

# TODO: Replace print statements with logging:
# - Initialization -> logging.info()
# - Processing details -> logging.debug()
# - Success messages -> logging.info()
# - Errors -> logging.error() with exception info
# - Warnings -> logging.warning()

# TODO: Use logging.exception() for automatic exception logging
# TODO: Include traceback information for debugging

# TODO: Add structured logging for processing statistics

if __name__ == '__main__':
    # Test the enhanced processor
    processor = DataProcessor()

    # Test successful operations
    try:
        result = processor.process_number("10")
        print(f"Number result: {result}")
    except Exception as e:
        print(f"Number processing failed: {e}")

    # Test error handling
    try:
        processor.process_number("-5")
    except Exception:
        pass  # Should be logged, not printed

    # Test list processing with mixed data
    data = ["5", "15", "abc", "25"]
    try:
        results = processor.process_list(data)
        print(f"List results: {results}")
    except Exception as e:
        print(f"List processing failed: {e}")

    # Test file processing (create a test file first)
    with open("test_numbers.txt", "w") as f:
        f.write("1,2,3,invalid,5")

    try:
        results = processor.process_file("test_numbers.txt")
        print(f"File results: {results}")
    except Exception as e:
        print(f"File processing failed: {e}")

    # Test with non-existent file
    try:
        processor.process_file("nonexistent.txt")
    except Exception:
        pass  # Should be logged
```

---

## 🏗️ Exercise 5: Modular Logging System

**Goal**: Create a reusable logging system for multiple modules

### Modular Application Structure

Create multiple modules that use a shared logging configuration:

```python
# config/logging_config.py
"""Centralized logging configuration."""

import logging.config

LOGGING_CONFIG = {
    # TODO: Create comprehensive logging configuration
    # - Multiple loggers for different modules
    # - Console and file handlers
    # - Appropriate formatters
    # - Log rotation for file handlers
}

def setup_logging(level='INFO'):
    """Set up logging for the entire application."""
    # TODO: Apply configuration with specified level
    pass

def get_logger(name):
    """Get a logger with the specified name."""
    # TODO: Return configured logger
    pass
```

```python
# services/user_service.py
"""User management service."""

from config.logging_config import get_logger

class UserService:
    """Service for user management operations."""

    def __init__(self):
        self.logger = get_logger(__name__)
        # TODO: Add initialization logging

    def create_user(self, username, email):
        """Create a new user."""
        # TODO: Add debug logging for method entry
        # TODO: Add info logging for success
        # TODO: Add error logging for failures
        pass

    def find_user(self, username):
        """Find a user by username."""
        # TODO: Add appropriate logging
        pass
```

```python
# services/email_service.py
"""Email sending service."""

from config.logging_config import get_logger

class EmailService:
    """Service for sending emails."""

    def __init__(self):
        self.logger = get_logger(__name__)
        # TODO: Add initialization logging

    def send_welcome_email(self, email):
        """Send welcome email to new user."""
        # TODO: Add debug logging for email details
        # TODO: Add info logging for success
        # TODO: Add error logging for SMTP failures
        pass

    def send_password_reset(self, email):
        """Send password reset email."""
        # TODO: Add appropriate logging
        pass
```

### Implementing Modular Logging

Create the main application that uses the modular logging:

```python
# main.py
"""Main application with modular logging."""

from config.logging_config import setup_logging
from services.user_service import UserService
from services.email_service import EmailService

def main():
    """Main application function."""
    # TODO: Set up logging at the start of the application
    # TODO: Log application startup

    # Initialize services
    user_service = UserService()
    email_service = EmailService()

    # TODO: Test various operations with comprehensive logging
    # - User creation (success and failure cases)
    # - User lookup operations
    # - Email sending operations
    # - Error scenarios

    # TODO: Log application shutdown

if __name__ == '__main__':
    main()
```

**Expected Modular Logging:**
```
2023-12-07 10:30:15 - services.user_service - INFO - UserService initialized
2023-12-07 10:30:15 - services.email_service - INFO - EmailService initialized
2023-12-07 10:30:15 - services.user_service - DEBUG - Creating user: alice
2023-12-07 10:30:15 - services.user_service - INFO - User alice created successfully
2023-12-07 10:30:15 - services.email_service - DEBUG - Sending welcome email to alice@example.com
2023-12-07 10:30:15 - services.email_service - INFO - Welcome email sent to alice@example.com
```

---

## 🏆 Challenge Exercises

### Challenge 1: Custom Logging Handler

Create a custom logging handler that sends logs to a database or external service:

```python
class DatabaseHandler(logging.Handler):
    """Custom handler that stores logs in a database."""

    def __init__(self, db_connection):
        super().__init__()
        self.db = db_connection

    def emit(self, record):
        # TODO: Store log record in database
        # TODO: Handle database errors gracefully
        pass

# TODO: Integrate the custom handler into the logging configuration
```

### Challenge 2: Log Analysis and Monitoring

Create a log analysis tool that reads and analyzes log files:

```python
class LogAnalyzer:
    """Tool for analyzing log files."""

    def parse_log_file(self, filename):
        """Parse a log file and extract statistics."""
        # TODO: Read log file
        # TODO: Parse log entries
        # TODO: Extract statistics (error counts, response times, etc.)
        pass

    def generate_report(self, stats):
        """Generate a summary report."""
        # TODO: Create human-readable report
        pass

# TODO: Implement log analysis functionality
```

### Challenge 3: Structured Logging with Context

Implement structured logging with request IDs and user context:

```python
class StructuredLogger:
    """Logger with structured data and context."""

    def __init__(self, name):
        self.logger = logging.getLogger(name)

    def log_with_context(self, level, message, **context):
        """Log with additional context information."""
        # TODO: Add context to log record
        # TODO: Include request_id, user_id, etc.
        pass

# TODO: Implement structured logging with context
```

---

## ✅ Solution Code

### Exercise 1: Basic Logging Setup

```python
import logging
import logging.config
from user_manager import UserManager

# Logging configuration
LOGGING_CONFIG = {
    'version': 1,
    'disable_existing_loggers': False,

    'formatters': {
        'simple': {
            'format': '%(levelname)s: %(message)s'
        },
        'detailed': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        }
    },

    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'INFO',
            'formatter': 'simple'
        },
        'file': {
            'class': 'logging.FileHandler',
            'level': 'DEBUG',
            'formatter': 'detailed',
            'filename': 'user_manager.log'
        }
    },

    'loggers': {
        '': {
            'level': 'DEBUG',
            'handlers': ['console', 'file']
        }
    }
}

# Apply configuration
logging.config.dictConfig(LOGGING_CONFIG)
logger = logging.getLogger(__name__)

# Enhanced UserManager with logging
class UserManager:
    def __init__(self):
        self.users = {}
        logger.info("UserManager initialized")

    def create_user(self, username, email, password):
        logger.debug(f"Creating user: {username}")
        if username in self.users:
            logger.error(f"User {username} already exists")
            return False

        if not self._validate_email(email):
            logger.error(f"Invalid email format: {email}")
            return False

        user_data = {
            'email': email,
            'password': password,
            'created_at': '2023-12-07'
        }
        self.users[username] = user_data
        logger.info(f"User {username} created successfully")
        return True

    def authenticate_user(self, username, password):
        logger.debug(f"Authenticating user: {username}")
        if username not in self.users:
            logger.warning(f"Authentication failed: user {username} not found")
            return False

        user_data = self.users[username]
        if user_data['password'] != password:
            logger.warning(f"Authentication failed: invalid password for {username}")
            return False

        logger.info(f"User {username} authenticated successfully")
        return True

    def get_user_info(self, username):
        logger.debug(f"Retrieving info for user: {username}")
        if username not in self.users:
            logger.warning(f"User {username} not found")
            return None

        user_data = self.users[username].copy()
        del user_data['password']
        logger.info(f"Retrieved info for user {username}")
        return user_data

    def _validate_email(self, email):
        return '@' in email and '.' in email

if __name__ == '__main__':
    manager = UserManager()

    manager.create_user("alice", "alice@example.com", "pass123")
    manager.create_user("bob", "bob@example.com", "pass456")
    manager.create_user("alice", "duplicate@example.com", "pass789")

    manager.authenticate_user("alice", "pass123")
    manager.authenticate_user("alice", "wrongpass")

    info = manager.get_user_info("alice")
    info = manager.get_user_info("nonexistent")
```

### Exercise 2: Converting Print to Logging

```python
import logging
import logging.config
from calculator import Calculator

LOGGING_CONFIG = {
    'version': 1,
    'formatters': {
        'console': {'format': '%(levelname)s: %(message)s'},
        'file': {'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s'}
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'WARNING',
            'formatter': 'console'
        },
        'file': {
            'class': 'logging.FileHandler',
            'level': 'DEBUG',
            'formatter': 'file',
            'filename': 'calculator.log'
        }
    },
    'loggers': {
        '': {'level': 'DEBUG', 'handlers': ['console', 'file']}
    }
}

logging.config.dictConfig(LOGGING_CONFIG)
logger = logging.getLogger(__name__)

class Calculator:
    def __init__(self):
        self.history = []
        logger.info("Calculator initialized")

    def add(self, a, b):
        logger.debug(f"Adding {a} + {b}")
        result = a + b
        self.history.append(f"add({a}, {b}) = {result}")
        logger.debug(f"Result: {result}")
        return result

    def subtract(self, a, b):
        logger.debug(f"Subtracting {a} - {b}")
        result = a - b
        self.history.append(f"subtract({a}, {b}) = {result}")
        logger.debug(f"Result: {result}")
        return result

    def multiply(self, a, b):
        logger.debug(f"Multiplying {a} * {b}")
        result = a * b
        self.history.append(f"multiply({a}, {b}) = {result}")
        logger.debug(f"Result: {result}")
        return result

    def divide(self, a, b):
        logger.debug(f"Dividing {a} / {b}")
        if b == 0:
            logger.error("Division by zero attempted")
            return None
        result = a / b
        self.history.append(f"divide({a}, {b}) = {result}")
        logger.debug(f"Result: {result}")
        return result

    def power(self, base, exponent):
        logger.debug(f"Calculating {base} ^ {exponent}")
        result = base ** exponent
        self.history.append(f"power({base}, {exponent}) = {result}")
        logger.debug(f"Result: {result}")
        return result

    def get_history(self):
        logger.info(f"Returning {len(self.history)} history entries")
        return self.history.copy()

    def clear_history(self):
        logger.info("Clearing calculation history")
        self.history.clear()

if __name__ == '__main__':
    calc = Calculator()

    calc.add(5, 3)
    calc.subtract(10, 4)
    calc.multiply(6, 7)
    calc.divide(15, 3)
    calc.divide(10, 0)
    calc.power(2, 3)

    history = calc.get_history()
    calc.clear_history()
```

### Exercise 3: Multi-Handler Configuration

```python
import logging
import logging.config
from web_scraper import WebScraper

LOGGING_CONFIG = {
    'version': 1,
    'formatters': {
        'simple': {'format': '%(levelname)s: %(message)s'},
        'detailed': {'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s'},
        'error_only': {'format': '%(asctime)s - %(name)s - ERROR - %(message)s'}
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'INFO',
            'formatter': 'simple'
        },
        'file': {
            'class': 'logging.handlers.RotatingFileHandler',
            'level': 'DEBUG',
            'formatter': 'detailed',
            'filename': 'scraper.log',
            'maxBytes': 1024*1024,
            'backupCount': 3
        },
        'error_file': {
            'class': 'logging.FileHandler',
            'level': 'ERROR',
            'formatter': 'error_only',
            'filename': 'scraper_errors.log'
        }
    },
    'loggers': {
        '': {
            'level': 'DEBUG',
            'handlers': ['console', 'file', 'error_file']
        }
    }
}

logging.config.dictConfig(LOGGING_CONFIG)
logger = logging.getLogger(__name__)

class WebScraper:
    def __init__(self):
        import requests
        self.session = requests.Session()
        self.session.headers.update({'User-Agent': 'WebScraper/1.0'})
        logger.info("Web scraper initialized")

    def fetch_page(self, url):
        logger.debug(f"Fetching URL: {url}")
        try:
            response = self.session.get(url, timeout=10)
            logger.debug(f"Response status: {response.status_code}")

            if response.status_code == 200:
                logger.info(f"Successfully fetched {len(response.text)} characters from {url}")
                return response.text
            else:
                logger.warning(f"HTTP {response.status_code} for URL: {url}")
                return None
        except requests.exceptions.Timeout:
            logger.error(f"Timeout fetching {url}")
            return None
        except requests.exceptions.ConnectionError:
            logger.error(f"Connection error for {url}")
            return None
        except Exception as e:
            logger.error(f"Unexpected error fetching {url}: {e}")
            return None

    def extract_title(self, html):
        logger.debug("Extracting title from HTML")
        try:
            start = html.find('<title>')
            end = html.find('</title>')
            if start != -1 and end != -1:
                title = html[start+7:end].strip()
                logger.debug(f"Extracted title: '{title}'")
                return title
            else:
                logger.warning("No title found in HTML")
                return None
        except Exception as e:
            logger.error(f"Error extracting title: {e}")
            return None

    def scrape_multiple_pages(self, urls):
        logger.info(f"Starting batch scrape of {len(urls)} URLs")
        results = {}

        for i, url in enumerate(urls, 1):
            logger.info(f"Processing {i}/{len(urls)}: {url}")
            content = self.fetch_page(url)
            if content:
                title = self.extract_title(content)
                results[url] = title
            else:
                results[url] = None

            import time
            time.sleep(1)  # Be nice to servers

        successful = sum(1 for v in results.values() if v is not None)
        logger.info(f"Batch scrape completed. {successful}/{len(urls)} successful.")
        return results

if __name__ == '__main__':
    scraper = WebScraper()

    content = scraper.fetch_page("https://httpbin.org/html")
    if content:
        title = scraper.extract_title(content)

    urls = [
        "https://httpbin.org/html",
        "https://httpbin.org/status/404",
        "https://httpbin.org/delay/5"
    ]
    results = scraper.scrape_multiple_pages(urls)
```

---

## 📝 Key Takeaways

1. **Logging Configuration**: Use dictConfig for complex logging setups with multiple handlers
2. **Appropriate Log Levels**: Choose DEBUG for development, INFO for normal operations, WARNING/ERROR for issues
3. **Multiple Handlers**: Use different handlers for console (human-readable) and files (detailed)
4. **Exception Logging**: Use logging.exception() or include traceback in error messages
5. **Logger Naming**: Use __name__ for hierarchical, organized logging
6. **Performance**: Logging can be disabled and is more efficient than print statements
7. **Structured Information**: Include relevant context in log messages

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
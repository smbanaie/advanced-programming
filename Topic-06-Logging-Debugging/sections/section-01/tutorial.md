# Tutorial 1: Python Logging Fundamentals

**Section**: 1 - Python Logging Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-5 (Basic Python, OOP, Testing)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Configure Logging**: Set up Python's logging module with appropriate configuration
2. **Use Log Levels**: Apply DEBUG, INFO, WARNING, ERROR, and CRITICAL levels correctly
3. **Implement Handlers**: Configure console, file, and custom log destinations
4. **Format Log Messages**: Create readable, informative log message formats
5. **Create Loggers**: Use logger hierarchy and proper naming conventions
6. **Debug with Logging**: Use logging to identify and resolve application issues

---

## 📚 Table of Contents

1. [Why Logging Matters](#why-logging-matters)
2. [Logging Module Overview](#logging-module-overview)
3. [Log Levels](#log-levels)
4. [Basic Logging Setup](#basic-logging-setup)
5. [Loggers](#loggers)
6. [Handlers](#handlers)
7. [Formatters](#formatters)
8. [Configuration Methods](#configuration-methods)
9. [Best Practices](#best-practices)
10. [Debugging with Logging](#debugging-with-logging)

---

## 🤔 Why Logging Matters

Logging is the process of recording application events, errors, and diagnostic information. Unlike print statements, logging provides structured, configurable, and production-ready monitoring capabilities.

### Logging vs Print Statements

```python
# Problematic: Using print statements
def process_user_data(user_data):
    print("Processing user data...")
    if not user_data:
        print("ERROR: No user data provided!")
        return None
    print(f"Processing data for user: {user_data.get('name', 'Unknown')}")
    # ... processing logic ...
    print("User data processed successfully")

# Better: Using logging
import logging

def process_user_data(user_data):
    logging.info("Processing user data...")
    if not user_data:
        logging.error("No user data provided!")
        return None
    logging.debug(f"Processing data for user: {user_data.get('name', 'Unknown')}")
    # ... processing logic ...
    logging.info("User data processed successfully")
```

**Benefits of Logging:**
- **Configurable**: Enable/disable different log levels
- **Structured**: Consistent format and metadata
- **Flexible**: Multiple output destinations (console, files, network)
- **Performance**: Can be disabled in production
- **Thread-safe**: Safe for concurrent applications
- **Standardized**: Industry-standard logging practices

---

## 📦 Logging Module Overview

Python's `logging` module is part of the standard library and provides comprehensive logging capabilities.

### Importing Logging

```python
import logging
```

### Basic Components

The logging system consists of four main components:

1. **Loggers**: Objects that generate log messages
2. **Handlers**: Objects that determine where log messages go
3. **Formatters**: Objects that determine how log messages look
4. **Filters**: Objects that determine which log records to process (optional)

### Logging Flow

```
Logger → Handler(s) → Formatter → Output Destination
```

---

## 📊 Log Levels

Log levels indicate the severity or importance of log messages. Python's logging module defines five standard levels:

### Standard Log Levels

```python
# Log level constants (numeric values)
logging.DEBUG     # 10 - Detailed diagnostic information
logging.INFO      # 20 - General information
logging.WARNING   # 30 - Warning messages (default level)
logging.ERROR     # 40 - Error conditions
logging.CRITICAL  # 50 - Critical errors

# Usage examples
logging.debug("Detailed diagnostic information for developers")
logging.info("General information about application operation")
logging.warning("Something unexpected happened, but application continues")
logging.error("An error occurred, but application can continue")
logging.critical("Critical error that may cause application to stop")
```

### When to Use Each Level

```python
def authenticate_user(username, password):
    logging.debug(f"Authenticating user: {username}")

    user = find_user(username)
    if not user:
        logging.warning(f"Authentication failed: user {username} not found")
        return False

    if not verify_password(password, user.password_hash):
        logging.warning(f"Authentication failed: invalid password for {username}")
        return False

    logging.info(f"User {username} authenticated successfully")
    return True

def process_payment(amount, user_id):
    try:
        # Process payment logic
        result = payment_gateway.charge(amount, user_id)
        logging.info(f"Payment processed successfully: ${amount} for user {user_id}")
        return result
    except ConnectionError as e:
        logging.error(f"Payment gateway connection failed: {e}")
        raise
    except PaymentError as e:
        logging.error(f"Payment processing failed: {e}")
        raise
    except Exception as e:
        logging.critical(f"Unexpected error in payment processing: {e}")
        raise
```

### Setting Log Levels

```python
# Set the root logger level
logging.basicConfig(level=logging.DEBUG)

# All messages will be shown
logging.debug("This will appear")    # ✓
logging.info("This will appear")     # ✓
logging.warning("This will appear")  # ✓

# Only WARNING and above
logging.basicConfig(level=logging.WARNING)

logging.debug("This won't appear")   # ✗
logging.info("This won't appear")    # ✗
logging.warning("This will appear")  # ✓
```

---

## 🏗️ Basic Logging Setup

### basicConfig() - Simple Setup

The `basicConfig()` function provides a simple way to configure logging for basic use cases.

```python
import logging

# Basic console logging
logging.basicConfig(level=logging.INFO)
logging.info("This is an info message")

# Output: INFO:root:This is an info message
```

### Basic Configuration Options

```python
# Configure log level and format
logging.basicConfig(
    level=logging.DEBUG,
    format='%(levelname)s:%(name)s:%(message)s'
)

logging.debug("Debug message")
logging.info("Info message")

# Output:
# DEBUG:root:Debug message
# INFO:root:Info message
```

### File Logging

```python
# Log to a file
logging.basicConfig(
    filename='app.log',
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

logging.info("Application started")
logging.warning("This is a warning")
logging.error("This is an error")

# Check the app.log file
# 2023-12-07 10:30:15,123 - INFO - Application started
# 2023-12-07 10:30:15,124 - WARNING - This is a warning
# 2023-12-07 10:30:15,125 - ERROR - This is an error
```

### Console and File Logging

```python
import logging

# Create logger
logger = logging.getLogger(__name__)

# Create handlers
console_handler = logging.StreamHandler()  # Console output
file_handler = logging.FileHandler('app.log')  # File output

# Set levels for handlers
console_handler.setLevel(logging.WARNING)  # Only warnings and above
file_handler.setLevel(logging.DEBUG)       # All messages

# Create formatters
console_format = logging.Formatter('%(levelname)s: %(message)s')
file_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# Set formatters
console_handler.setFormatter(console_format)
file_handler.setFormatter(file_format)

# Add handlers to logger
logger.addHandler(console_handler)
logger.addHandler(file_handler)

# Set logger level
logger.setLevel(logging.DEBUG)

# Test logging
logger.debug("This goes to file only")     # File only
logger.info("This goes to file only")      # File only
logger.warning("This goes to both")        # Console and file
logger.error("This goes to both")          # Console and file
```

---

## 📝 Loggers

Loggers are objects that generate log messages. They form a hierarchy based on their names.

### Getting Loggers

```python
# Root logger (avoid using directly)
root_logger = logging.getLogger()

# Named logger (recommended)
app_logger = logging.getLogger('my_app')
db_logger = logging.getLogger('my_app.database')
api_logger = logging.getLogger('my_app.api')
```

### Logger Hierarchy

```python
import logging

# Set up hierarchy
root_logger = logging.getLogger()  # Root logger
app_logger = logging.getLogger('app')
db_logger = logging.getLogger('app.database')
api_logger = logging.getLogger('app.api')

# Configure root logger
logging.basicConfig(level=logging.DEBUG)

# Child loggers inherit configuration from parents
# If no specific configuration, they use parent's level

print(f"Root level: {root_logger.level}")      # 30 (WARNING default)
print(f"App level: {app_logger.level}")        # 0 (NOTSET - inherits)
print(f"DB level: {db_logger.level}")          # 0 (NOTSET - inherits)

# Effective levels (what they actually use)
print(f"Root effective: {root_logger.getEffectiveLevel()}")      # 30
print(f"App effective: {app_logger.getEffectiveLevel()}")        # 30
print(f"DB effective: {db_logger.getEffectiveLevel()}")          # 30
```

### Logger Naming Best Practices

```python
# Good naming patterns
logger = logging.getLogger(__name__)  # Use module name

# For classes
class UserService:
    def __init__(self):
        self.logger = logging.getLogger(f"{__name__}.{self.__class__.__name__}")

# For functions
def process_data():
    logger = logging.getLogger(f"{__name__}.process_data")
    logger.info("Processing started")
```

---

## 🎯 Handlers

Handlers determine where log messages are sent (console, files, network, etc.).

### Common Handlers

```python
import logging
import logging.handlers

# 1. StreamHandler - Console output
console_handler = logging.StreamHandler()  # Defaults to stderr
console_handler.setLevel(logging.INFO)

# 2. FileHandler - File output
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.DEBUG)

# 3. RotatingFileHandler - Automatic rotation
rotating_handler = logging.handlers.RotatingFileHandler(
    'app.log',
    maxBytes=1024*1024,  # 1MB
    backupCount=5        # Keep 5 backup files
)

# 4. TimedRotatingFileHandler - Time-based rotation
time_handler = logging.handlers.TimedRotatingFileHandler(
    'app.log',
    when='midnight',     # Rotate at midnight
    interval=1,          # Every 1 interval
    backupCount=30       # Keep 30 days
)
```

### Handler Configuration

```python
import logging

# Create logger
logger = logging.getLogger('my_app')

# Create handlers
console = logging.StreamHandler()
file_handler = logging.FileHandler('app.log')

# Set levels
console.setLevel(logging.WARNING)    # Console: warnings and above
file_handler.setLevel(logging.DEBUG) # File: all messages

# Create formatters
simple_format = logging.Formatter('%(levelname)s: %(message)s')
detailed_format = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s:%(lineno)d - %(message)s'
)

# Set formatters
console.setFormatter(simple_format)
file_handler.setFormatter(detailed_format)

# Add handlers to logger
logger.addHandler(console)
logger.addHandler(file_handler)

# Set logger level (handlers can have stricter levels)
logger.setLevel(logging.DEBUG)

# Test
logger.debug("Debug message (file only)")
logger.info("Info message (file only)")
logger.warning("Warning message (both)")
logger.error("Error message (both)")
```

---

## 🎨 Formatters

Formatters control how log messages are displayed by defining the layout and content.

### Basic Formatting

```python
import logging

# Simple format
formatter1 = logging.Formatter('%(levelname)s: %(message)s')
# Output: INFO: User logged in

# With timestamp
formatter2 = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
# Output: 2023-12-07 10:30:15,123 - INFO - User logged in

# Detailed format
formatter3 = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s:%(lineno)d - %(message)s'
)
# Output: 2023-12-07 10:30:15,123 - my_app - INFO - login:25 - User logged in
```

### Format String Attributes

| Attribute | Description | Example |
|-----------|-------------|---------|
| `%(asctime)s` | Timestamp | `2023-12-07 10:30:15,123` |
| `%(name)s` | Logger name | `my_app.database` |
| `%(levelname)s` | Log level | `INFO`, `ERROR` |
| `%(message)s` | Log message | `User logged in` |
| `%(funcName)s` | Function name | `login` |
| `%(lineno)d` | Line number | `25` |
| `%(pathname)s` | Full file path | `/path/to/file.py` |
| `%(filename)s` | File name | `file.py` |
| `%(process)d` | Process ID | `12345` |
| `%(thread)d` | Thread ID | `12345` |

### Custom Formatting

```python
import logging

class CustomFormatter(logging.Formatter):
    """Custom formatter with colored output."""

    def format(self, record):
        # Add colors based on level
        if record.levelno >= logging.ERROR:
            color = '\033[91m'  # Red
        elif record.levelno >= logging.WARNING:
            color = '\033[93m'  # Yellow
        elif record.levelno >= logging.INFO:
            color = '\033[92m'  # Green
        else:
            color = '\033[94m'  # Blue

        reset = '\033[0m'  # Reset color

        # Format the message
        formatted = super().format(record)
        return f"{color}{formatted}{reset}"

# Usage
logger = logging.getLogger('color_app')
handler = logging.StreamHandler()
handler.setFormatter(CustomFormatter('%(levelname)s: %(message)s'))
logger.addHandler(handler)
logger.setLevel(logging.DEBUG)

logger.info("This is green")
logger.warning("This is yellow")
logger.error("This is red")
```

### JSON Formatting for Structured Logging

```python
import json
import logging

class JSONFormatter(logging.Formatter):
    """JSON formatter for structured logging."""

    def format(self, record):
        log_entry = {
            'timestamp': self.formatTime(record),
            'level': record.levelname,
            'logger': record.name,
            'message': record.getMessage(),
            'module': record.module,
            'function': record.funcName,
            'line': record.lineno
        }

        # Add exception info if present
        if record.exc_info:
            log_entry['exception'] = self.formatException(record.exc_info)

        return json.dumps(log_entry)

# Usage
logger = logging.getLogger('json_app')
handler = logging.StreamHandler()
handler.setFormatter(JSONFormatter())
logger.addHandler(handler)

logger.info("Application started", extra={'user': 'admin'})
# Output: {"timestamp": "2023-12-07 10:30:15", "level": "INFO", ...}
```

---

## ⚙️ Configuration Methods

### Dictionary Configuration (Recommended)

```python
import logging.config

# Dictionary configuration
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
            'filename': 'app.log'
        }
    },

    'loggers': {
        '': {  # Root logger
            'level': 'DEBUG',
            'handlers': ['console', 'file']
        },
        'my_app.database': {
            'level': 'WARNING',
            'handlers': ['file'],
            'propagate': False
        }
    }
}

# Apply configuration
logging.config.dictConfig(LOGGING_CONFIG)

# Test
logger = logging.getLogger('my_app')
db_logger = logging.getLogger('my_app.database')

logger.info("This goes to console and file")
db_logger.warning("This goes to file only")
```

### INI File Configuration

```python
# logging.ini
[loggers]
keys=root,my_app

[handlers]
keys=consoleHandler,fileHandler

[formatters]
keys=simpleFormatter,detailedFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler,fileHandler

[logger_my_app]
level=INFO
handlers=fileHandler
qualname=my_app
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=INFO
formatter=simpleFormatter
args=(sys.stdout,)

[handler_fileHandler]
class=FileHandler
level=DEBUG
formatter=detailedFormatter
args=('app.log',)

[formatter_simpleFormatter]
format=%(levelname)s: %(message)s

[formatter_detailedFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
```

```python
import logging.config

# Load from INI file
logging.config.fileConfig('logging.ini')

logger = logging.getLogger('my_app')
logger.info("Configured from INI file")
```

---

## 🎯 Best Practices

### Logger Naming

```python
# Good: Use module names
logger = logging.getLogger(__name__)

# Good: Hierarchical naming
db_logger = logging.getLogger('app.database')
api_logger = logging.getLogger('app.api')

# Bad: Generic names
logger = logging.getLogger('logger')
logger = logging.getLogger('my_logger')
```

### Log Message Formatting

```python
# Good: Descriptive messages
logger.info("User %s logged in from %s", username, ip_address)
logger.error("Failed to connect to database: %s", error_message)

# Bad: Non-descriptive messages
logger.info("Done")
logger.error("Error")

# Bad: String formatting in log call
logger.info(f"User {username} logged in")  # Creates string even if not logged
```

### Configuration Management

```python
# Good: Centralized configuration
def setup_logging(config_dict):
    logging.config.dictConfig(config_dict)

# Good: Environment-based configuration
import os
log_level = os.getenv('LOG_LEVEL', 'INFO')
logging.basicConfig(level=getattr(logging, log_level))

# Good: Separate config files
# Keep logging config in separate files for different environments
```

### Exception Logging

```python
import traceback

try:
    risky_operation()
except Exception as e:
    logger.error("Operation failed: %s", e)
    logger.debug("Traceback: %s", traceback.format_exc())
    raise
```

### Performance Considerations

```python
# Good: Lazy evaluation
logger.debug("Processing %d items", len(items))  # Only evaluates if DEBUG enabled

# Bad: Expensive operations always executed
logger.debug(f"Processing {len(items)} items: {expensive_function(items)}")
```

---

## 🔍 Debugging with Logging

### Adding Debug Logging

```python
def complex_calculation(data):
    logger = logging.getLogger(__name__)

    logger.debug("Starting calculation with %d data points", len(data))

    # Process data
    result = 0
    for i, item in enumerate(data):
        logger.debug("Processing item %d: %s", i, item)
        result += item
        if i % 100 == 0:
            logger.debug("Progress: %d/%d items processed", i, len(data))

    logger.debug("Calculation completed. Result: %s", result)
    return result

# Configure debug logging
logging.basicConfig(level=logging.DEBUG)

# Test
result = complex_calculation(list(range(1000)))
```

### Tracing Function Calls

```python
def traced_function(func):
    """Decorator to add logging to function calls."""
    def wrapper(*args, **kwargs):
        logger = logging.getLogger(func.__module__)
        logger.debug("Calling %s with args=%s kwargs=%s",
                    func.__name__, args, kwargs)

        try:
            result = func(*args, **kwargs)
            logger.debug("Function %s returned %s", func.__name__, result)
            return result
        except Exception as e:
            logger.error("Function %s raised %s", func.__name__, e)
            raise
    return wrapper

@traced_function
def divide(a, b):
    return a / b

# Test
logging.basicConfig(level=logging.DEBUG)
result = divide(10, 2)  # Logs entry and return
divide(10, 0)           # Logs entry and exception
```

### Conditional Logging

```python
class DebugLogger:
    """Logger with conditional debug output."""

    def __init__(self, name):
        self.logger = logging.getLogger(name)
        self.debug_enabled = self.logger.isEnabledFor(logging.DEBUG)

    def debug_data(self, data, message="Data dump"):
        """Log data only if debug is enabled."""
        if self.debug_enabled:
            import json
            self.logger.debug("%s: %s", message, json.dumps(data, indent=2))

# Usage
debug_logger = DebugLogger(__name__)
large_data = {"users": list(range(1000))}
debug_logger.debug_data(large_data)  # Only logs if DEBUG enabled
```

---

## 🎯 Key Takeaways

1. **Logging vs Print**: Logging is configurable, structured, and production-ready
2. **Log Levels**: Use appropriate levels (DEBUG, INFO, WARNING, ERROR, CRITICAL)
3. **Handlers**: Control where logs go (console, files, network)
4. **Formatters**: Define how log messages look and what information they contain
5. **Logger Hierarchy**: Use named loggers with proper hierarchy
6. **Configuration**: Use dictConfig for complex logging setups
7. **Performance**: Logging can be disabled and is more efficient than print
8. **Debugging**: Use logging strategically to trace program execution

---

## 🔗 Further Reading

- [Logging Documentation](https://docs.python.org/3/library/logging.html)
- [Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html)
- [Twelve-Factor App Logging](https://12factor.net/logs)

---

## 📝 Practice Exercises

1. **Basic Logger Setup**: Create a logger that writes INFO+ to console and DEBUG+ to file
2. **Exception Logging**: Modify a function to log exceptions with full tracebacks
3. **Hierarchical Loggers**: Set up loggers for different modules with different levels
4. **Custom Formatter**: Create a formatter that includes request IDs for web apps
5. **Configuration File**: Set up logging using dictConfig with multiple handlers

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
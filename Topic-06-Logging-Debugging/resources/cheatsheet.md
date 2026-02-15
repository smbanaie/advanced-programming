# Logging & Debugging Cheatsheet - Python Logging Fundamentals

**Topic 6**: Logging & Debugging
**Quick Reference**: logging module, handlers, formatters, debugging

---

## 🧪 Basic Logging Setup

### Importing and Basic Usage

```python
import logging

# Simple setup
logging.basicConfig(level=logging.INFO)
logging.info("Application started")

# Output: INFO:root:Application started
```

### Log Levels

```python
# Standard levels (numeric values)
logging.DEBUG     # 10 - Detailed diagnostic info
logging.INFO      # 20 - General information
logging.WARNING   # 30 - Warning messages (default)
logging.ERROR     # 40 - Error conditions
logging.CRITICAL  # 50 - Serious errors

# Usage
logging.debug("Debug message")      # Development details
logging.info("Info message")        # Normal operations
logging.warning("Warning message")  # Potential issues
logging.error("Error message")      # Errors that don't stop execution
logging.critical("Critical message") # Errors that may stop execution
```

---

## 🏗️ Advanced Logging Configuration

### Dictionary Configuration (Recommended)

```python
import logging.config

LOGGING_CONFIG = {
    'version': 1,
    'disable_existing_loggers': False,

    'formatters': {
        'simple': {
            'format': '%(levelname)s: %(message)s'
        },
        'detailed': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        },
        'verbose': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s:%(lineno)d - %(message)s'
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
        },
        'rotating_file': {
            'class': 'logging.handlers.RotatingFileHandler',
            'level': 'DEBUG',
            'formatter': 'verbose',
            'filename': 'app.log',
            'maxBytes': 1024*1024,  # 1MB
            'backupCount': 5
        }
    },

    'loggers': {
        '': {  # Root logger
            'level': 'DEBUG',
            'handlers': ['console', 'rotating_file']
        },
        'my_app.database': {
            'level': 'WARNING',
            'handlers': ['file'],
            'propagate': False
        }
    }
}

logging.config.dictConfig(LOGGING_CONFIG)
```

### Getting Loggers

```python
# Root logger (avoid)
root_logger = logging.getLogger()

# Named logger (recommended)
app_logger = logging.getLogger(__name__)  # Module name
db_logger = logging.getLogger('my_app.database')
api_logger = logging.getLogger('my_app.api')

# Logger hierarchy
print(app_logger.name)     # my_app
print(db_logger.name)      # my_app.database
print(api_logger.name)     # my_app.api
```

---

## 🎯 Handlers Quick Reference

### Built-in Handlers

```python
# Console output
console_handler = logging.StreamHandler()  # stderr by default
console_handler.setLevel(logging.INFO)

# File output
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.DEBUG)

# Rotating files
from logging.handlers import RotatingFileHandler
rotating = RotatingFileHandler(
    'app.log',
    maxBytes=1024*1024,  # 1MB
    backupCount=5
)

# Time-based rotation
from logging.handlers import TimedRotatingFileHandler
time_rotating = TimedRotatingFileHandler(
    'app.log',
    when='midnight',
    interval=1,
    backupCount=30
)
```

### Handler Configuration

```python
# Configure handler
handler = logging.StreamHandler()
handler.setLevel(logging.DEBUG)

# Set formatter
formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)

# Add to logger
logger = logging.getLogger('my_app')
logger.addHandler(handler)
logger.setLevel(logging.DEBUG)
```

---

## 🎨 Formatters

### Built-in Attributes

```python
# Available in all log records
formatter = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

# Extended attributes
formatter = logging.Formatter(
    '%(asctime)s - %(name)s - %(levelname)s - '
    '%(funcName)s:%(lineno)d - %(message)s'
)

# All available attributes:
# asctime, name, levelname, message, funcName, lineno
# pathname, filename, module, process, thread, threadName
# processName, created, msecs, relativeCreated, levelno
```

### Custom Formatting

```python
class CustomFormatter(logging.Formatter):
    """Custom formatter with colors."""

    COLORS = {
        'DEBUG': '\033[94m',    # Blue
        'INFO': '\033[92m',     # Green
        'WARNING': '\033[93m',  # Yellow
        'ERROR': '\033[91m',    # Red
        'CRITICAL': '\033[95m'  # Magenta
    }
    RESET = '\033[0m'

    def format(self, record):
        color = self.COLORS.get(record.levelname, '')
        formatted = super().format(record)
        return f"{color}{formatted}{self.RESET}"

# Usage
handler = logging.StreamHandler()
handler.setFormatter(CustomFormatter('%(levelname)s: %(message)s'))
```

---

## 📊 Exception Logging

### Basic Exception Logging

```python
import logging

try:
    risky_operation()
except ValueError as e:
    logging.error(f"Value error: {e}")
except Exception as e:
    logging.critical(f"Unexpected error: {e}")
    raise
```

### Automatic Exception Info

```python
# logging.exception() automatically includes traceback
try:
    risky_operation()
except Exception:
    logging.exception("An error occurred")

# Equivalent to:
# logging.error("An error occurred", exc_info=True)
```

### Manual Traceback

```python
import traceback

try:
    risky_operation()
except Exception as e:
    logging.error(f"Error: {e}")
    logging.debug(f"Traceback: {traceback.format_exc()}")
```

---

## 🔍 Debugging with Logging

### Adding Debug Logging

```python
def complex_function(data, user_id):
    logger = logging.getLogger(__name__)

    logger.debug(f"Processing data for user {user_id}")
    logger.debug(f"Data size: {len(data)} items")

    result = []
    for i, item in enumerate(data):
        logger.debug(f"Processing item {i}: {item}")
        # Processing logic...
        result.append(processed_item)

        if i % 100 == 0:
            logger.debug(f"Progress: {i}/{len(data)} items processed")

    logger.debug(f"Processing complete. Result size: {len(result)}")
    return result
```

### Conditional Logging

```python
class PerformanceLogger:
    """Logger that checks if debug is enabled before expensive operations."""

    def __init__(self, name):
        self.logger = logging.getLogger(name)
        self.debug_enabled = self.logger.isEnabledFor(logging.DEBUG)

    def debug_expensive_operation(self, data):
        """Only perform expensive logging if debug is enabled."""
        if self.debug_enabled:
            expensive_summary = self._create_expensive_summary(data)
            self.logger.debug(f"Data summary: {expensive_summary}")

    def _create_expensive_summary(self, data):
        """Expensive operation that creates debug summary."""
        # Complex analysis...
        return f"Summary of {len(data)} items"
```

### Context Managers for Timing

```python
import time
from contextlib import contextmanager

@contextmanager
def log_execution_time(logger, operation_name):
    """Context manager to log execution time."""
    start_time = time.time()
    logger.debug(f"Starting {operation_name}")

    try:
        yield
    finally:
        end_time = time.time()
        duration = end_time - start_time
        logger.debug(".2f")

# Usage
logger = logging.getLogger(__name__)

with log_execution_time(logger, "database query"):
    results = execute_query()

with log_execution_time(logger, "file processing"):
    process_file("large_file.txt")
```

---

## ⚙️ Configuration Patterns

### Environment-Based Configuration

```python
import os

def setup_logging():
    """Set up logging based on environment."""
    log_level = os.getenv('LOG_LEVEL', 'INFO')
    log_file = os.getenv('LOG_FILE', 'app.log')

    numeric_level = getattr(logging, log_level.upper(), logging.INFO)

    logging.basicConfig(
        level=numeric_level,
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        handlers=[
            logging.StreamHandler(),
            logging.FileHandler(log_file)
        ]
    )

# Usage
setup_logging()  # Respects LOG_LEVEL and LOG_FILE environment variables
```

### Class-Based Configuration

```python
class LoggingConfig:
    """Centralized logging configuration."""

    @staticmethod
    def get_config(env='development'):
        """Get logging configuration for environment."""

        if env == 'production':
            return {
                'version': 1,
                'root': {
                    'level': 'INFO',
                    'handlers': ['file', 'email']
                },
                'handlers': {
                    'file': {
                        'class': 'logging.FileHandler',
                        'filename': 'app.log',
                        'formatter': 'detailed'
                    },
                    'email': {
                        'class': 'logging.handlers.SMTPHandler',
                        'mailhost': 'localhost',
                        'fromaddr': 'app@server.com',
                        'toaddrs': ['admin@server.com'],
                        'subject': 'Application Error',
                        'level': 'ERROR'
                    }
                }
            }
        else:  # development
            return {
                'version': 1,
                'root': {
                    'level': 'DEBUG',
                    'handlers': ['console']
                },
                'handlers': {
                    'console': {
                        'class': 'logging.StreamHandler',
                        'formatter': 'simple'
                    }
                }
            }

# Usage
config = LoggingConfig.get_config('production')
logging.config.dictConfig(config)
```

---

## 🚀 Best Practices

### Logger Naming

```python
# Good: Hierarchical naming
logger = logging.getLogger(__name__)  # my_app.module

# Good: Descriptive names
db_logger = logging.getLogger('my_app.database')
api_logger = logging.getLogger('my_app.api')

# Bad: Generic names
logger = logging.getLogger('logger')
logger = logging.getLogger('my_logger')
```

### Log Message Formatting

```python
# Good: Clear, contextual messages
logger.info("User %s logged in from %s", username, ip_address)
logger.error("Failed to connect to database: %s", error_message)

# Bad: Unclear messages
logger.info("Done")  # What was done?
logger.error("Error")  # What error?

# Bad: String formatting in log call
logger.info(f"User {username} logged in")  # Evaluated even if not logged
```

### Performance Considerations

```python
# Good: Lazy evaluation
logger.debug("Processing %d items", len(items))  # Only if DEBUG enabled

# Bad: Expensive operations always executed
logger.debug(f"Processing {len(items)} items: {expensive_function(items)}")

# Good: Check level before expensive operations
if logger.isEnabledFor(logging.DEBUG):
    expensive_data = create_expensive_debug_data()
    logger.debug("Debug data: %s", expensive_data)
```

### Exception Handling

```python
# Good: Comprehensive exception logging
try:
    risky_operation()
except ValueError as e:
    logger.warning("Invalid input: %s", e)
except ConnectionError as e:
    logger.error("Network error: %s", e)
    logger.debug("Connection details: %s", traceback.format_exc())
except Exception as e:
    logger.critical("Unexpected error: %s", e, exc_info=True)
    raise
```

---

## 🐛 Common Issues and Solutions

### Issue: Too Many Debug Logs in Production

```python
# Solution: Use appropriate levels and conditional logging
logger.debug("Detailed info")  # Only in development
logger.info("Important info")  # Always logged

# Or use different handlers
console_handler.setLevel(logging.WARNING)  # Console: warnings+
file_handler.setLevel(logging.DEBUG)       # File: all messages
```

### Issue: Log Files Growing Too Large

```python
# Solution: Use rotating handlers
from logging.handlers import RotatingFileHandler

handler = RotatingFileHandler(
    'app.log',
    maxBytes=10*1024*1024,  # 10MB
    backupCount=5
)
```

### Issue: Missing Context in Logs

```python
# Solution: Include relevant context
logger.info("User %s performed action %s", user_id, action)
logger.error("Database query failed: %s", query, extra={'user': user_id})
```

### Issue: Performance Impact

```python
# Solution: Minimize logging overhead
# 1. Use appropriate levels
# 2. Avoid expensive operations in log calls
# 3. Use lazy formatting
# 4. Consider async logging for high-volume apps
```

---

## 🎯 Quick Examples

### Complete Logging Setup

```python
import logging
import logging.config

# Configuration
LOGGING_CONFIG = {
    'version': 1,
    'formatters': {
        'simple': {'format': '%(levelname)s: %(message)s'},
        'detailed': {'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s'}
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
            'filename': 'app.log',
            'maxBytes': 1024*1024,
            'backupCount': 3
        }
    },
    'loggers': {
        '': {
            'level': 'DEBUG',
            'handlers': ['console', 'file']
        }
    }
}

# Setup
logging.config.dictConfig(LOGGING_CONFIG)
logger = logging.getLogger(__name__)

# Usage
logger.info("Application started")
logger.debug("Debug information")
logger.error("An error occurred")

try:
    risky_operation()
except Exception:
    logger.exception("Operation failed")
```

### Class with Logging

```python
class DataProcessor:
    """Example class with proper logging."""

    def __init__(self):
        self.logger = logging.getLogger(f"{__name__}.{self.__class__.__name__}")
        self.logger.info("DataProcessor initialized")

    def process_data(self, data):
        """Process data with comprehensive logging."""
        self.logger.debug("Starting data processing")
        self.logger.debug("Input data size: %d", len(data))

        try:
            result = self._perform_processing(data)
            self.logger.info("Data processing completed successfully")
            self.logger.debug("Output data size: %d", len(result))
            return result
        except Exception as e:
            self.logger.error("Data processing failed: %s", e)
            self.logger.debug("Error details", exc_info=True)
            raise

    def _perform_processing(self, data):
        """Internal processing logic."""
        # Processing logic here...
        return processed_data
```

---

## 📚 Key Takeaways

1. **Logging > Print**: Logging is configurable, structured, and production-ready
2. **Log Levels**: Use DEBUG for development, INFO for operations, WARNING/ERROR for issues
3. **Configuration**: Use dictConfig for complex setups with multiple handlers
4. **Logger Naming**: Use __name__ for hierarchical, organized logging
5. **Exception Logging**: Use logging.exception() or include traceback in errors
6. **Performance**: Check levels before expensive operations, use lazy formatting
7. **Context**: Include relevant context (user IDs, operation names) in log messages
8. **Monitoring**: Design logs for production monitoring and debugging

---

**Cheatsheet Version**: 1.0
**Last Updated**: February 2026
**Pages**: 1
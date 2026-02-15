# Homework 1: Add Logging to Previous Projects

**Topic**: 6 - Logging & Debugging
**Section**: 1 of 2 sections (90 min workshop + homework)
**Level**: Intermediate to Advanced
**Prerequisites**: Topics 1-5 (Basic Python, OOP, Testing)

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Integrate Logging**: Add comprehensive logging to existing codebases
2. **Configure Production Logging**: Set up logging suitable for production environments
3. **Apply Best Practices**: Follow logging conventions and performance considerations
4. **Debug with Logs**: Use logging output to identify and resolve issues
5. **Maintain Code**: Ensure logging doesn't interfere with existing functionality
6. **Monitor Applications**: Create logs useful for application monitoring and troubleshooting

---

## 📋 Assignment Overview

You will take code from previous topics and enhance it with comprehensive logging. The goal is to transform basic applications into production-ready systems with proper monitoring, debugging, and error tracking capabilities.

Choose **one** of the following projects to enhance (or use your own project from previous topics):

### Option A: Student Management System (from Topic 4)
### Option B: Inventory Management (from Topic 3)
### Option C: Data Processor (from Topic 3)
### Option D: Your Own Project

---

## 🛠️ Implementation Requirements

### 1. Logging Infrastructure

Create a centralized logging configuration:

```python
# logging_config.py
"""Centralized logging configuration for the application."""

import logging
import logging.config
from pathlib import Path

def setup_logging(
    log_level='INFO',
    log_to_file=True,
    log_to_console=True,
    max_file_size=10*1024*1024,  # 10MB
    backup_count=5
):
    """
    Set up comprehensive logging for the application.

    Args:
        log_level: Minimum log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
        log_to_file: Whether to log to files
        log_to_console: Whether to log to console
        max_file_size: Maximum size of log files before rotation
        backup_count: Number of backup log files to keep
    """
    # TODO: Create logging configuration with:
    # - Console handler with simple formatting
    # - Rotating file handler for main logs
    # - Separate error log file
    # - Appropriate formatters with timestamps and context
    # - Logger hierarchy for different modules
    pass

def get_logger(name):
    """
    Get a configured logger with the specified name.

    Args:
        name: Logger name (typically __name__)

    Returns:
        Configured logger instance
    """
    # TODO: Return logger with proper configuration
    pass
```

### 2. Enhanced Application Code

Take your chosen application and enhance it with comprehensive logging:

```python
# Example: Enhanced Student Management System
from logging_config import setup_logging, get_logger

# TODO: Set up logging at application startup
# TODO: Use appropriate log levels throughout the code

class University:
    """Enhanced university management with logging."""

    def __init__(self):
        # TODO: Get logger for this class
        # TODO: Log initialization

        self.students = {}
        self.courses = {}

    def add_student(self, student):
        """Add a student with logging."""
        # TODO: Log method entry with parameters
        # TODO: Log success/failure with appropriate levels
        # TODO: Include relevant context in log messages
        pass

    def enroll_student(self, student_id, course_id):
        """Enroll student in course with detailed logging."""
        # TODO: Log enrollment process steps
        # TODO: Log validation results
        # TODO: Log final outcome
        pass

    def generate_report(self):
        """Generate report with performance logging."""
        # TODO: Log report generation start
        # TODO: Log processing steps and counts
        # TODO: Log completion with summary statistics
        pass

# TODO: Add logging to all classes and methods
# TODO: Include debug logging for troubleshooting
# TODO: Add error logging with exception details
# TODO: Log performance information where relevant
```

### 3. Logging Best Practices Implementation

Implement advanced logging features:

```python
# structured_logging.py
"""Advanced logging features."""

import logging
import json
import time
from functools import wraps

class StructuredLogger:
    """Logger with structured data and performance tracking."""

    def __init__(self, name):
        self.logger = logging.getLogger(name)

    def log_performance(self, operation, duration, **context):
        """Log performance information."""
        # TODO: Log operation duration with context
        pass

    def log_user_action(self, user_id, action, **details):
        """Log user actions for audit trail."""
        # TODO: Log user actions with structured data
        pass

    def log_error_with_context(self, error, user_id=None, operation=None, **context):
        """Log errors with full context."""
        # TODO: Log errors with user, operation, and additional context
        pass

def performance_logger(func):
    """Decorator to log function performance."""
    # TODO: Create decorator that logs execution time
    # TODO: Include function name and parameters
    pass

def audit_logger(func):
    """Decorator to log function calls for auditing."""
    # TODO: Create decorator that logs function entry/exit
    # TODO: Include user context if available
    pass
```

### 4. Log Analysis and Monitoring

Create tools for log analysis:

```python
# log_analyzer.py
"""Tools for analyzing log files."""

import re
from collections import defaultdict, Counter
from datetime import datetime

class LogAnalyzer:
    """Analyze log files for insights and monitoring."""

    def __init__(self, log_file):
        self.log_file = log_file

    def parse_logs(self):
        """Parse log file and extract structured data."""
        # TODO: Read log file
        # TODO: Parse log entries into structured format
        # TODO: Extract timestamps, levels, messages, etc.
        pass

    def get_error_summary(self):
        """Get summary of errors and warnings."""
        # TODO: Count errors by type
        # TODO: Identify most common error patterns
        # TODO: Calculate error rates over time
        pass

    def get_performance_stats(self):
        """Extract performance statistics from logs."""
        # TODO: Find performance log entries
        # TODO: Calculate average execution times
        # TODO: Identify performance bottlenecks
        pass

    def generate_report(self):
        """Generate comprehensive log analysis report."""
        # TODO: Combine all analysis into readable report
        # TODO: Include recommendations for improvements
        pass
```

---

## 📋 Submission Requirements

### Required Deliverables

1. **Enhanced Application**: Original code with comprehensive logging added
2. **Logging Configuration**: Centralized logging setup with multiple handlers
3. **Log Analysis Tools**: Scripts to analyze and monitor log files
4. **Documentation**: README explaining the logging implementation
5. **Log Samples**: Example log output showing different scenarios

### Logging Features to Implement

- [ ] **Multiple Log Levels**: DEBUG, INFO, WARNING, ERROR, CRITICAL appropriately used
- [ ] **Multiple Handlers**: Console and rotating file handlers configured
- [ ] **Structured Logging**: Include relevant context in log messages
- [ ] **Exception Logging**: Proper error logging with stack traces
- [ ] **Performance Logging**: Track execution times for key operations
- [ ] **Security Logging**: Log authentication and authorization events
- [ ] **Configuration Management**: Environment-based log level configuration

### Quality Standards

**Code Quality:**
- [ ] Logging doesn't break existing functionality
- [ ] Appropriate log levels chosen for each message type
- [ ] Log messages are clear and informative
- [ ] No sensitive information logged
- [ ] Performance impact minimized

**Configuration Quality:**
- [ ] Production-ready logging configuration
- [ ] Proper log rotation and file management
- [ ] Environment-specific configurations
- [ ] Easy to modify and maintain

**Monitoring Quality:**
- [ ] Logs provide sufficient information for debugging
- [ ] Error patterns are identifiable
- [ ] Performance issues are detectable
- [ ] Security events are logged

---

## 🎯 Evaluation Criteria

### Logging Implementation (40%)
- [ ] Comprehensive logging coverage across all modules
- [ ] Appropriate log levels for different types of messages
- [ ] Proper exception handling and error logging
- [ ] Structured logging with relevant context
- [ ] Performance logging for key operations

### Configuration Quality (25%)
- [ ] Production-ready logging configuration
- [ ] Multiple handlers properly configured
- [ ] Log rotation and file management implemented
- [ ] Environment-based configuration support
- [ ] Easy maintenance and modification

### Monitoring and Debugging (20%)
- [ ] Logs enable effective debugging and troubleshooting
- [ ] Error patterns and trends are identifiable
- [ ] Performance issues can be detected from logs
- [ ] Security and audit events are properly logged
- [ ] Log analysis tools provide actionable insights

### Code Quality (15%)
- [ ] Logging integration doesn't break existing functionality
- [ ] Clean, maintainable logging code
- [ ] Proper separation of concerns
- [ ] Documentation and comments for logging logic
- [ ] Performance considerations addressed

---

## 🚀 Bonus Challenges

### Advanced Logging Features

1. **Distributed Logging**: Implement logging that works across multiple processes or machines
2. **Log Aggregation**: Set up centralized log collection and analysis
3. **Real-time Monitoring**: Create dashboards or alerts based on log data
4. **Log Encryption**: Implement encrypted logging for sensitive applications
5. **Custom Log Levels**: Add application-specific log levels beyond the standard ones

### Production Enhancements

1. **Log Shipping**: Configure log shipping to external services (ELK stack, etc.)
2. **Structured JSON Logging**: Implement JSON-formatted logs for better parsing
3. **Context Propagation**: Pass request IDs and user context through log chains
4. **Sampling**: Implement log sampling for high-volume applications
5. **Metrics Integration**: Connect logging with application metrics and monitoring

---

## 📞 Getting Help

### Common Issues
- **Log Storm**: Too many debug logs in production - adjust log levels
- **Performance Impact**: Logging slowing down application - use lazy formatting
- **Log Rotation**: Logs not rotating properly - check file permissions and configuration
- **Encoding Issues**: Special characters in logs - use proper encoding
- **Context Loss**: Losing request context in logs - implement context propagation

### Resources
- [Python Logging Documentation](https://docs.python.org/3/library/logging.html)
- [Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html)
- [Twelve-Factor App Logging](https://12factor.net/logs)
- [Structured Logging](https://www.thoughtworks.com/radar/techniques/structured-logging)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **Logging Integration**: Adding logging to existing codebases without breaking functionality
- **Configuration Management**: Setting up production-ready logging systems
- **Monitoring and Debugging**: Using logs for application monitoring and issue resolution
- **Best Practices**: Following logging conventions and performance considerations
- **Quality Assurance**: Implementing logging for better code quality and maintainability
- **Production Readiness**: Creating logs suitable for production environments and monitoring

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 8-12 hours
**Total Points**: 100
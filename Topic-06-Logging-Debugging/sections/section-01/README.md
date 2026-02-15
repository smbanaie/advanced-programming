# Section 1: Python Logging Fundamentals

**Topic**: 6 - Logging & Debugging
**Section**: 1 of 2 sections (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-5 (Basic Python, OOP, Testing)

---

## 📋 Section Overview

This section introduces Python's built-in logging module, the standard library for implementing logging in Python applications. You'll learn to configure loggers, handlers, and formatters to create comprehensive logging systems. Through practical examples, you'll understand how to monitor applications and debug issues effectively.

---

## 🎯 Learning Objectives

By the end of this section, you will be able to:

1. **Configure Logging**: Set up Python's logging module with appropriate configuration
2. **Use Log Levels**: Apply DEBUG, INFO, WARNING, ERROR, and CRITICAL levels correctly
3. **Implement Handlers**: Configure console, file, and custom log destinations
4. **Format Log Messages**: Create readable, informative log message formats
5. **Create Loggers**: Use logger hierarchy and proper naming conventions
6. **Debug with Logging**: Use logging to identify and resolve application issues

---

## 📚 Section Materials

### **Tutorial**: Python Logging Fundamentals
- logging module overview and basic configuration
- Log levels (DEBUG through CRITICAL) and their appropriate use
- Handlers for directing log output (console, files, etc.)
- Formatters for controlling log message appearance
- Logger hierarchy and naming best practices
- Configuration methods (basicConfig, dictConfig, fileConfig)

### **Workshop**: Adding Logging to Existing Apps
- Hands-on logging implementation in existing code
- Converting print statements to proper logging
- Setting up multiple handlers and formatters
- Implementing logging in classes and functions
- Debugging with logging output

### **Homework**: Add Logging to Previous Projects
- Independent logging implementation in existing codebases
- Comprehensive logging configuration for production use
- Log analysis and debugging using logging output
- Best practices application in real projects

---

## 🔄 Progression Path

### **Within This Section**
1. **Tutorial**: Learn logging module fundamentals and configuration
2. **Workshop**: Apply concepts by adding logging to existing applications
3. **Homework**: Independent implementation of production-ready logging

### **Topic Foundation**
- Introduces logging as essential monitoring and debugging tool
- Foundation for all subsequent logging topics

### **Leading to Section 2**
- Section 1 provides logging module knowledge and basic setup
- Section 2 extends with Loguru for simplified, advanced logging
- Combined foundation enables comprehensive application monitoring

---

## 📋 Prerequisites

### Required Knowledge
- Basic Python syntax and functions
- Understanding of exceptions and error handling
- File I/O operations and path handling
- Command-line familiarity for log file inspection

### Recommended Experience
- Completion of Topics 1-5
- Experience with Python applications and debugging
- Understanding of software development workflow

### Environment Setup
- Python 3.8+ installed (logging module is built-in)
- Text editor or IDE with Python support
- Terminal/command prompt for running examples
- No additional libraries required for this section

---

## 🛠️ Key Concepts Covered

### Logging Module Components
- **Loggers**: Objects that generate log messages
- **Handlers**: Objects that determine where log messages go
- **Formatters**: Objects that determine log message format
- **Filters**: Objects that determine which log records to process

### Log Levels Hierarchy
- **DEBUG (10)**: Detailed diagnostic information
- **INFO (20)**: General information about application operation
- **WARNING (30)**: Indicators of potential problems
- **ERROR (40)**: Error conditions that don't stop execution
- **CRITICAL (50)**: Serious errors that may stop the application

### Handler Types
- **StreamHandler**: Sends logs to console or files
- **FileHandler**: Writes logs to files with optional rotation
- **RotatingFileHandler**: Automatically rotates log files
- **TimedRotatingFileHandler**: Rotates logs based on time intervals

### Configuration Methods
- **basicConfig()**: Simple configuration for basic use cases
- **dictConfig()**: Dictionary-based configuration for complex setups
- **fileConfig()**: Configuration from INI-style configuration files
- **Logger Objects**: Direct configuration of logger instances

---

## 🎯 Success Criteria

You will have successfully completed this section when you can:

- ✅ Configure logging systems using Python's logging module
- ✅ Apply appropriate log levels for different types of messages
- ✅ Set up multiple handlers for different logging destinations
- ✅ Create custom log message formats with relevant information
- ✅ Implement logger hierarchies and proper naming
- ✅ Use logging effectively for debugging and monitoring

---

## 📞 Getting Help

### During Section
- Review tutorial examples for logging configuration patterns
- Check workshop solutions for handler and formatter setup
- Use homework requirements as logging implementation guides

### Resources
- `resources/cheatsheet.md` - Logging syntax and configuration reference
- `resources/useful-links.md` - Additional logging learning materials
- Python logging documentation

---

## ⏰ Time Allocation

- **Tutorial**: 30-40 minutes (logging concepts and configuration examples)
- **Workshop**: 40-50 minutes (hands-on logging implementation)
- **Homework**: 4-6 hours (comprehensive logging integration)

---

**Section Version**: 1.0
**Last Updated**: February 2026
**Section Leads To**: Section 2 (Advanced Logging with Loguru)
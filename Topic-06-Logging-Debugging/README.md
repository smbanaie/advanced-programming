# Topic 6: Logging & Debugging

**Sections**: 2 sections (numbered 1-2 within this topic)
**Total Duration**: 2 × 90 minutes = 3 hours
**Level**: Intermediate to Advanced
**Prerequisites**: Topics 1-5 (Basic Python, OOP, Testing)

---

## 📋 Topic Overview

This topic introduces professional logging and debugging practices essential for maintaining and troubleshooting Python applications. You'll learn to implement comprehensive logging systems and master debugging techniques to identify and resolve issues efficiently. Through practical examples, you'll build skills for monitoring production applications and diagnosing complex problems.

---

## 🎯 Learning Outcomes

By the end of this topic, you will be able to:

1. **Implement Logging Systems**: Configure and use Python's logging module effectively
2. **Apply Log Levels**: Use appropriate logging levels for different types of messages
3. **Configure Handlers**: Set up file, console, and custom log handlers
4. **Format Log Messages**: Create readable, structured log formats
5. **Debug Python Code**: Use debugging tools to identify and fix issues
6. **Handle Production Issues**: Implement logging for monitoring and troubleshooting

---

## 📚 Topic Structure

### **Section 1: Python Logging Fundamentals** (90 min)
**Focus**: logging module, log levels, handlers, formatters, configuration

### **Section 2: Advanced Logging with Loguru** (90 min)
**Focus**: Loguru library, log rotation, structured logging, production setup

---

## 🔧 Key Concepts Covered

### Logging Fundamentals
- **Logging Module**: Python's built-in logging infrastructure
- **Log Levels**: DEBUG, INFO, WARNING, ERROR, CRITICAL hierarchy
- **Handlers**: Managing where log messages are sent (files, console, etc.)
- **Formatters**: Controlling how log messages are displayed
- **Loggers**: Creating and configuring logger instances

### Advanced Logging
- **Loguru Library**: Modern logging with simplified API
- **Log Rotation**: Automatic log file rotation and management
- **Structured Logging**: JSON formatting and metadata
- **Production Logging**: Scalable logging for deployed applications

### Debugging Techniques
- **Python Debugger**: Using pdb for interactive debugging
- **Breakpoint Setting**: Strategic placement of debug points
- **Variable Inspection**: Examining program state during execution
- **Exception Handling**: Proper error catching and reporting

---

## 📈 Progression Path

### **Building on Previous Topics**
- **Topic 1**: Git/version control for tracking debugged code changes
- **Topic 2**: Virtual environments for isolated debugging sessions
- **Topic 3**: Data structures as debugging subjects
- **Topic 4**: OOP classes and methods requiring logging
- **Topic 5**: Testing integrated with logging for better diagnostics

### **Section Dependencies**
1. **Section 1** → Foundation: Master Python's logging module basics
2. **Section 2** → Enhancement: Apply advanced logging with Loguru

### **Leading to Future Topics**
- **Topic 7**: API logging for request/response monitoring
- **Topic 8**: Web scraping logging for debugging crawler issues
- **Topic 9**: Production application logging and monitoring

---

## 🛠️ Prerequisites

### Required Knowledge
- Basic Python syntax and control structures
- Understanding of functions, classes, and exceptions
- File I/O operations and error handling
- Command-line familiarity

### Recommended Experience
- Completion of Topics 1-5
- Experience with Python applications and debugging
- Understanding of software development workflow

### Environment Setup
- Python 3.8+ installed and configured
- Text editor or IDE with debugging support
- Terminal/command prompt for log file inspection
- Optional: Loguru library for Section 2

---

## 🎯 Success Criteria

You will have successfully completed this topic when you can:

- ✅ Configure comprehensive logging systems using Python's logging module
- ✅ Implement appropriate log levels and message formatting
- ✅ Set up multiple handlers for different logging destinations
- ✅ Use Loguru for simplified, production-ready logging
- ✅ Implement log rotation and structured logging
- ✅ Debug Python applications using systematic techniques
- ✅ Monitor and troubleshoot applications with logging

---

## 📞 Getting Help

### During Topic Study
- Review tutorial examples for logging configuration patterns
- Complete workshop exercises to practice logging implementation
- Use homework projects to apply logging in real applications

### Resources
- `resources/cheatsheet.md` - Logging syntax and configuration reference
- `resources/useful-links.md` - Additional logging learning materials
- Python logging documentation and Loguru docs

---

## ⏰ Time Allocation

- **Section 1**: 90 minutes (logging fundamentals + basic debugging)
- **Section 2**: 90 minutes (Loguru advanced features + production logging)
- **Total**: 3 hours of structured learning + homework practice

---

## 🏗️ Topic Materials

```
Topic-06-Logging-Debugging/
├── README.md                           # This topic overview
├── sections/
│   ├── section-01/                     # Python Logging Fundamentals
│   │   ├── README.md                   # Section overview
│   │   ├── tutorial.md                 # logging module basics
│   │   ├── workshop.md                 # Adding logging to apps
│   │   └── homework.md                 # Add logging to projects
│   └── section-02/                     # Advanced Logging with Loguru
│       ├── README.md
│       ├── tutorial.md                 # Loguru features & rotation
│       ├── workshop.md                 # Production logging setup
│       └── homework.md                 # Loguru implementation
├── resources/                          # Topic-level resources
│   ├── cheatsheet.md                   # Logging syntax reference
│   ├── useful-links.md                 # Logging resources
│   └── best-practices.md               # Logging guidelines
├── installation/                       # Setup guides
└── assessments/                        # Topic quizzes/rubrics
```

---

## 📊 The Importance of Logging

### Why Logging Matters
- **Monitoring**: Track application behavior and performance
- **Debugging**: Identify issues in production environments
- **Auditing**: Maintain records of system activities
- **Troubleshooting**: Diagnose problems without direct access
- **Analytics**: Understand user behavior and system usage
- **Compliance**: Meet regulatory logging requirements

### Logging vs Printing
```python
# Bad: Using print statements
print("User logged in")
print("Database connection failed")

# Good: Using logging
import logging
logging.info("User logged in")
logging.error("Database connection failed")
```

### Log Levels and Their Uses
- **DEBUG**: Detailed diagnostic information for developers
- **INFO**: General information about application operation
- **WARNING**: Indicators of potential problems
- **ERROR**: Error conditions that don't stop the application
- **CRITICAL**: Serious errors that may stop the application

---

## 🐛 Debugging in Python

### Python Debugging Tools
- **pdb**: Python's built-in debugger
- **IDE Debuggers**: PyCharm, VS Code debugging features
- **Logging for Debugging**: Using logs to trace program execution
- **Exception Handling**: Proper error catching and reporting

### Systematic Debugging Approach
1. **Reproduce the Issue**: Create a minimal test case
2. **Add Logging**: Insert log statements to trace execution
3. **Use Debugger**: Step through code with breakpoints
4. **Isolate the Problem**: Narrow down the root cause
5. **Test the Fix**: Verify the solution works
6. **Add Tests**: Prevent regression with unit tests

---

## 🚀 Topic Challenges

### Progressive Difficulty
- **Section 1**: Basic logging setup and configuration (straightforward)
- **Section 2**: Advanced Loguru features and production setup (complex)

### Common Learning Curves
- **Configuration Complexity**: Understanding logging hierarchy and configuration
- **Loguru Paradigm Shift**: Moving from standard logging to Loguru's approach
- **Production Thinking**: Designing logs for monitoring rather than debugging
- **Performance Considerations**: Balancing logging detail with application speed

---

**Topic Version**: 1.0
**Last Updated**: February 2026
**Topic Leads To**: Topic 7 (API Development with FastAPI)
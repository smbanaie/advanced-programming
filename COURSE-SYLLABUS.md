# Advanced Programming in Python - Course Syllabus
**Target Audience**: Computer Engineering Students  
**Total Sessions**: 25 sections × 90 minutes each  
**Total Duration**: 37.5 hours

---

## Course Overview

This course covers advanced Python programming concepts essential for modern software development, including version control, dependency management, object-oriented programming, testing, logging, API development, and web scraping. Students will build practical skills through tutorials, workshops, and hands-on projects, culminating in a final crawler project.

---

## Course Structure

Each subject includes:
- **Tutorials**: Theoretical concepts and explanations
- **Workshops**: Hands-on guided exercises
- **Homeworks**: Independent practice assignments
- **Resources**: Cheatsheets, references, and additional materials

---

## Detailed Syllabus

### **Module 1: Development Environment & Version Control (Sections 1-3)**

#### **Section 1: Git Basics - Part 1** (90 min)
**Topics**:
- Introduction to version control systems
- Git installation and configuration
- Basic Git concepts (repository, commit, staging area)
- Creating your first repository
- Basic commands: init, add, commit, status, log

**Materials**:
- Tutorial: Git introduction and concepts
- Workshop: Setting up Git and creating first repository
- Homework: Configure Git profile and create personal repository

---

#### **Section 2: Git Basics - Part 2** (90 min)
**Topics**:
- Working with remote repositories (GitHub/GitLab)
- Cloning, pulling, and pushing
- Understanding branches
- Basic branching and merging
- Resolving merge conflicts

**Materials**:
- Tutorial: Remote repositories and branching
- Workshop: Working with GitHub, creating branches
- Homework: Branching exercise with conflict resolution

---

#### **Section 3: Git Advanced Workflows** (90 min)
**Topics**:
- Git workflows (Feature Branch, Gitflow)
- Pull requests and code review
- Rebasing vs merging
- Git best practices
- .gitignore and repository hygiene

**Materials**:
- Tutorial: Advanced Git workflows
- Workshop: Collaborative development simulation
- Homework: Implement a workflow in team project

---

### **Module 2: Python Environment & Dependency Management (Sections 4-5)**

#### **Section 4: Virtual Environments & Dependency Management** (90 min)
**Topics**:
- Why virtual environments matter
- Traditional venv and pip
- Introduction to modern package managers
- Understanding requirements.txt and lock files
- Dependency conflicts and resolution

**Materials**:
- Tutorial: Virtual environments and dependency concepts
- Workshop: Creating and managing venv
- Homework: Set up isolated environments for multiple projects

---

#### **Section 5: UV Package Manager & Project Setup** (90 min)
**Topics**:
- UV installation and configuration
- Creating projects with UV
- Adding and managing dependencies
- Running scripts with UV
- UV vs pip/poetry comparison
- Project structure best practices

**Materials**:
- Tutorial: UV package manager deep dive
- Workshop: Building a complete UV project
- Homework: Migrate an existing project to UV

---

### **Module 3: Python Data Structures & JSON API Work (Sections 6-7)**

#### **Section 6: Working with Lists and Sets** (90 min)
**Topics**:
- Python list operations (add, remove, update, sort, filter)
- List comprehensions and advanced operations
- Set operations (union, intersection, difference)
- Set methods and use cases
- Converting between lists and sets
- Performance considerations

**Materials**:
- Tutorial: Lists and sets operations in Python
- Workshop: Building data processing utilities
- Homework: Implement data manipulation functions

---

#### **Section 7: JSON Handling & API Integration** (90 min)
**Topics**:
- JSON data structure and Python dictionaries
- Dictionary operations (add, update, delete, merge)
- Reading and writing JSON files
- Making HTTP requests with requests library
- Working with randomuser.me API
- Processing API responses and data manipulation

**Materials**:
- Tutorial: JSON handling and API integration
- Workshop: Fetching and processing user data from randomuser.me
- Homework: Build a user data processor with JSON operations

---

### **Module 4: Object-Oriented Programming (Sections 8-11)**

#### **Section 8: OOP Fundamentals** (90 min)
**Topics**:
- Classes and objects
- Attributes and methods
- Constructor (__init__)
- Instance vs class variables
- self parameter
- Basic encapsulation

**Materials**:
- Tutorial: OOP basics in Python
- Workshop: Creating classes for a library management system
- Homework: Design and implement a student management system

---

#### **Section 9: OOP - Inheritance & Polymorphism** (90 min)
**Topics**:
- Inheritance basics
- Method overriding
- super() function
- Multiple inheritance
- Polymorphism and duck typing
- Abstract base classes (ABC)

**Materials**:
- Tutorial: Inheritance and polymorphism
- Workshop: Building a shape hierarchy with polymorphic behavior
- Homework: Implement a vehicle management system with inheritance

---

#### **Section 10: OOP - Advanced Concepts** (90 min)
**Topics**:
- Magic methods (__str__, __repr__, __eq__, etc.)
- Property decorators (@property)
- Class methods and static methods
- Composition vs inheritance
- Mixins
- Data classes (dataclasses module)

**Materials**:
- Tutorial: Advanced OOP patterns
- Workshop: Building a banking system with advanced OOP
- Homework: Implement a custom container class with magic methods

---

#### **Section 11: OOP - Design Patterns** (90 min)
**Topics**:
- Introduction to design patterns
- Singleton pattern
- Factory pattern
- Observer pattern
- Strategy pattern
- Practical applications in Python

**Materials**:
- Tutorial: Common design patterns in Python
- Workshop: Implementing design patterns
- Homework: Refactor code using appropriate design patterns

---

### **Module 5: Testing & Quality Assurance (Sections 12-14)**

#### **Section 12: Unit Testing Fundamentals** (90 min)
**Topics**:
- Why testing matters
- Testing pyramid (unit, integration, e2e)
- unittest module basics
- Test structure (Arrange-Act-Assert)
- Writing your first tests
- Running tests

**Materials**:
- Tutorial: Introduction to unit testing
- Workshop: Writing tests for existing code
- Homework: Achieve 80% code coverage for a module

---

#### **Section 13: Advanced Testing with pytest** (90 min)
**Topics**:
- pytest installation and setup
- pytest vs unittest
- Fixtures and setup/teardown
- Parametrized tests
- Mocking and patching
- Test coverage with pytest-cov

**Materials**:
- Tutorial: pytest framework deep dive
- Workshop: Building a comprehensive test suite
- Homework: Write tests for OOP classes from previous modules

---

#### **Section 14: Test-Driven Development (TDD)** (90 min)
**Topics**:
- TDD methodology
- Red-Green-Refactor cycle
- Writing tests first
- Benefits and challenges of TDD
- Integration testing basics
- Continuous Integration concepts

**Materials**:
- Tutorial: TDD principles and practices
- Workshop: Building a feature using TDD
- Homework: Implement a calculator using TDD approach

---

### **Module 6: Logging & Debugging (Sections 15-16)**

#### **Section 15: Logging Fundamentals** (90 min)
**Topics**:
- Why logging is important
- Python logging module
- Log levels (DEBUG, INFO, WARNING, ERROR, CRITICAL)
- Loggers, handlers, and formatters
- Logging configuration
- Best practices for logging

**Materials**:
- Tutorial: Python logging module
- Workshop: Adding logging to existing applications
- Homework: Implement comprehensive logging in previous projects

---

#### **Section 16: Advanced Logging with Loguru & Rotation** (90 min)
**Topics**:
- Limitations of standard logging
- Introduction to Loguru
- Loguru features and benefits
- Log rotation strategies
- Log file management
- Structured logging
- Logging in production environments

**Materials**:
- Tutorial: Loguru and log rotation
- Workshop: Setting up production-grade logging
- Homework: Implement Loguru with rotation in a multi-module project

---

### **Module 7: API Development with FastAPI (Sections 17-20)**

#### **Section 17: FastAPI Fundamentals** (90 min)
**Topics**:
- Introduction to web APIs
- REST principles
- FastAPI installation and setup with UV
- Creating your first endpoint
- Path parameters and query parameters
- Request and response models with Pydantic

**Materials**:
- Tutorial: FastAPI basics and REST concepts
- Workshop: Building a simple API
- Homework: Create a TODO API with basic CRUD operations

---

#### **Section 18: FastAPI - Request Handling** (90 min)
**Topics**:
- Request body and validation
- Pydantic models in depth
- Data validation and serialization
- Response models and status codes
- Error handling and exceptions
- Dependency injection basics

**Materials**:
- Tutorial: Advanced request handling in FastAPI
- Workshop: Building a user management API
- Homework: Add validation and error handling to previous API

---

#### **Section 19: FastAPI - Advanced Features** (90 min)
**Topics**:
- Authentication and authorization
- JWT tokens
- Middleware
- CORS configuration
- Background tasks
- File uploads and downloads
- API documentation (Swagger/OpenAPI)

**Materials**:
- Tutorial: Advanced FastAPI features
- Workshop: Implementing authentication in API
- Homework: Build a secure file management API

---

#### **Section 20: FastAPI - Database Integration & Testing** (90 min)
**Topics**:
- Database integration (SQLAlchemy basics)
- Async database operations
- API testing with TestClient
- Testing authenticated endpoints
- API best practices
- Deployment considerations with Uvicorn

**Materials**:
- Tutorial: Database integration and API testing
- Workshop: Building a complete API with database
- Homework: Create a blog API with full CRUD and tests

---

### **Module 8: Web Scraping Fundamentals (Sections 21-23)**

#### **Section 21: Web Scraping Basics** (90 min)
**Topics**:
- Introduction to web scraping
- HTML/CSS basics for scraping
- requests library
- BeautifulSoup basics
- Parsing HTML content
- Extracting data
- Ethics and legal considerations

**Materials**:
- Tutorial: Web scraping fundamentals
- Workshop: Scraping a simple website
- Homework: Extract data from multiple pages

---

#### **Section 22: Advanced Scraping Techniques** (90 min)
**Topics**:
- Handling JavaScript-rendered content
- Selenium basics
- Working with dynamic websites
- Handling pagination
- Rate limiting and politeness
- User agents and headers
- Handling errors and retries

**Materials**:
- Tutorial: Advanced scraping techniques
- Workshop: Scraping dynamic websites
- Homework: Build a scraper for a JavaScript-heavy site

---

#### **Section 23: Crawlee Python Framework** (90 min)
**Topics**:
- Introduction to Crawlee
- Crawlee architecture
- Setting up Crawlee projects
- Crawlers vs scrapers
- Request queues
- Data storage
- Crawlee best practices

**Materials**:
- Tutorial: Crawlee framework overview
- Workshop: Building your first Crawlee crawler
- Homework: Create a multi-page crawler with Crawlee

---

### **Module 9: Final Project Development (Sections 24-25)**

#### **Section 24: Final Project - Planning & Implementation** (90 min)
**Topics**:
- Project requirements and scope
- Designing the crawler architecture
- Choosing target websites
- Data model design
- Setting up project with UV
- Implementing core crawler functionality
- Data extraction logic
- Implementing logging with Loguru
- Code organization and OOP principles

**Materials**:
- Tutorial: Project planning and implementation best practices
- Workshop: Guided planning and implementation session
- Homework: Complete core crawler implementation with planning document

---

#### **Section 25: Final Project - API Integration & Presentation** (90 min)
**Topics**:
- Adding API endpoints with FastAPI
- Exposing scraped data via API
- Implementing tests (unit and integration)
- Error handling and edge cases
- Performance optimization
- Code review best practices
- Project presentations and deployment

**Materials**:
- Tutorial: API integration, testing, and deployment
- Workshop: Adding API layer and final presentations
- Homework: Submit complete project with documentation and tests

---

#### **Section 25: Final Project - Presentation & Review** (90 min)
**Topics**:
- Code review best practices
- Project presentations
- Deployment considerations
- Scaling and production readiness
- Course review and next steps
- Q&A session

**Materials**:
- Tutorial: Deployment and production best practices
- Workshop: Project presentations and peer review
- Homework: Submit final project with documentation

---

## Final Project Requirements

### **Crawler Project with Crawlee**

**Objective**: Build a production-ready web crawler with API interface

**Requirements**:
1. **Crawler Implementation**:
   - Use Crawlee Python framework
   - Scrape data from at least 3 related websites
   - Handle pagination and navigation
   - Implement proper error handling
   - Use Loguru for logging with rotation

2. **Object-Oriented Design**:
   - Well-structured classes and modules
   - Apply appropriate design patterns
   - Follow SOLID principles

3. **API Layer**:
   - FastAPI endpoints to query scraped data
   - Proper request/response models
   - Authentication (optional but recommended)
   - API documentation

4. **Testing**:
   - Unit tests for core logic (>70% coverage)
   - Integration tests for API endpoints
   - pytest framework

5. **Project Management**:
   - Git repository with meaningful commits
   - UV for dependency management
   - Comprehensive README
   - Code documentation

6. **Deliverables**:
   - Source code on GitHub
   - Documentation (README, API docs)
   - Test suite
   - Presentation (10 minutes)

---

## Assessment Breakdown

| Component | Weight |
|-----------|--------|
| Workshop Participation | 15% |
| Homework Assignments | 25% |
| Module Quizzes | 15% |
| Midterm Project (API Project) | 20% |
| Final Project (Crawler) | 25% |

---

## Prerequisites

- Basic Python programming knowledge
- Understanding of functions, loops, and data structures
- Command line familiarity
- Basic understanding of web technologies (HTML, HTTP)

---

## Learning Outcomes

By the end of this course, students will be able to:

1. ✅ Use Git for version control and collaborative development
2. ✅ Manage Python projects with modern tools (UV)
3. ✅ Design and implement object-oriented solutions
4. ✅ Write comprehensive unit tests using pytest
5. ✅ Implement production-grade logging systems
6. ✅ Build RESTful APIs with FastAPI
7. ✅ Develop web scrapers and crawlers
8. ✅ Apply best practices for code quality and maintainability
9. ✅ Work with modern Python development workflows
10. ✅ Build and deploy complete Python applications

---

## Recommended Tools & Technologies

- **Version Control**: Git, GitHub/GitLab
- **Package Manager**: UV
- **IDE**: VS Code, PyCharm
- **Testing**: pytest, pytest-cov
- **Logging**: Loguru
- **API Framework**: FastAPI, Uvicorn
- **Web Scraping**: Crawlee, BeautifulSoup, Selenium
- **Database** (optional): SQLite, PostgreSQL
- **Documentation**: Markdown, Swagger/OpenAPI

---

## Additional Resources

- Official Python documentation
- FastAPI documentation
- Crawlee Python documentation
- Real Python tutorials
- GitHub repositories with example projects
- Stack Overflow for troubleshooting

---

## Notes for Instructors

- Each 90-minute section should include:
  - 30-40 min: Lecture/Tutorial
  - 40-50 min: Hands-on workshop
  - 10 min: Q&A and homework assignment
  
- Encourage students to:
  - Commit code regularly to Git
  - Write tests alongside code
  - Use logging from the beginning
  - Ask questions and collaborate
  
- Provide code review sessions periodically
- Use real-world examples throughout
- Adapt pace based on student progress

---

**Course Version**: 1.0  
**Last Updated**: February 2026

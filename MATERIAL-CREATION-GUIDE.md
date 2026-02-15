# Material Creation Guide - Advanced Programming in Python

This document serves as a systematic guide for creating course materials for each section. Use this as a checklist and instruction set for AI agents or instructors developing the course content.

---

## 📋 Overall Structure

Each subject folder should follow this structure:

```
Subject-XX-Topic-Name/
├── README.md                    # Subject overview and learning objectives
├── installation/                # Setup guides (if applicable)
│   └── *.md
├── tutorials/                   # Theoretical content
│   ├── 01-*.md
│   ├── 02-*.md
│   └── ...
├── workshops/                   # Hands-on exercises with solutions
│   ├── workshop-01-*.md
│   ├── workshop-02-*.md
│   └── ...
├── homeworks/                   # Independent assignments
│   ├── homework-01-*.md
│   ├── homework-02-*.md
│   └── ...
├── resources/                   # Additional materials
│   ├── cheatsheet.md
│   ├── useful-links.md
│   └── troubleshooting.md
└── assessments/                 # Quizzes and evaluation materials
    ├── quiz-*.md
    └── rubric-*.md
```

---

## 🎯 Material Creation Checklist for Each Section

### For Each Tutorial Document:
- [ ] Clear title and learning objectives
- [ ] Prerequisites section
- [ ] Theoretical explanation with examples
- [ ] Code snippets with explanations
- [ ] Visual aids (diagrams, flowcharts) when helpful
- [ ] Common pitfalls and best practices
- [ ] Summary/key takeaways
- [ ] Links to further reading

### For Each Workshop Document:
- [ ] Clear objectives and expected outcomes
- [ ] Prerequisites and setup instructions
- [ ] Step-by-step instructions
- [ ] Code examples to follow along
- [ ] Expected output at each step
- [ ] Troubleshooting section
- [ ] Challenge exercises (optional)
- [ ] Solution code (in separate file or collapsed section)

### For Each Homework Assignment:
- [ ] Clear problem statement
- [ ] Learning objectives
- [ ] Requirements and specifications
- [ ] Submission guidelines
- [ ] Evaluation criteria/rubric
- [ ] Hints (optional)
- [ ] Estimated time to complete
- [ ] Due date placeholder

### For Each Subject README:
- [ ] Subject overview
- [ ] Learning outcomes
- [ ] Topics covered
- [ ] Materials structure
- [ ] Prerequisites
- [ ] Progression path
- [ ] Assessment strategy

---

## 📝 Section-by-Section Creation Plan

### ✅ COMPLETED SECTIONS

#### ✅ Section 1-3: Git Basics (Module 1)
**Status**: COMPLETED  
**Location**: `Subject-01-Git-basics/`  
**Materials**:
- ✅ Tutorials (4 files)
- ✅ Workshops (4 files)
- ✅ Homeworks (3 files)
- ✅ Installation guides
- ✅ Resources (cheatsheet, links)
- ✅ README

#### 📦 Section 4-5: Python Environment & UV (Module 2)
**Status**: COMPLETED
**Location**: `Subject-02-Environment-UV/`
**Priority**: N/A (Already completed)

**Required Materials**:

**Tutorials** (2 files):
1. `01-lists-sets.md` - List operations, set operations, conversions, performance
2. `02-json-dictionaries.md` - JSON handling, dictionary operations, API requests with requests

**Workshops** (2 files):
1. `workshop-01-list-set-operations.md` - Building data processing utilities
2. `workshop-02-json-api-integration.md` - Fetching and processing randomuser.me data

**Homeworks** (2 files):
1. `homework-01-data-manipulation.md` - Implement data manipulation functions
2. `homework-02-user-data-processor.md` - Build user data processor with JSON operations

**Installation**:
- `requests-setup.md` - Installing requests library

**Resources**:
- `cheatsheet.md` - Data structures and JSON operations
- `useful-links.md` - Python data structures resources
- `api-examples.md` - Common API patterns

#### 📦 Section 6-7: Python Data Structures & JSON (Module 3)
**Status**: PENDING
**Location**: `Subject-03-Data-Structures/`
**Priority**: HIGH (Next to create)

**Required Materials**:

**Tutorials** (2 files):
1. `01-lists-sets.md` - List operations, set operations, conversions, performance
2. `02-json-dictionaries.md` - JSON handling, dictionary operations, API requests with requests

**Workshops** (2 files):
1. `workshop-01-list-set-operations.md` - Building data processing utilities
2. `workshop-02-json-api-integration.md` - Fetching and processing randomuser.me data

**Homeworks** (2 files):
1. `homework-01-data-manipulation.md` - Implement data manipulation functions
2. `homework-02-user-data-processor.md` - Build user data processor with JSON operations

**Installation**:
- `requests-setup.md` - Installing requests library

**Resources**:
- `cheatsheet.md` - Data structures and JSON operations
- `useful-links.md` - Python data structures resources
- `api-examples.md` - Common API patterns

#### ✅ Section 8-11: Object-Oriented Programming (Module 4)

---

### 🔄 PENDING SECTIONS

#### 📦 Section 8-11: Object-Oriented Programming (Module 4)
**Status**: PENDING  
**Location**: `Subject-03-OOP/`  
**Priority**: HIGH (Next to create)

**Required Materials**:

**Tutorials** (4 files):
1. `01-oop-fundamentals.md` - Classes, objects, attributes, methods, __init__, self
2. `02-inheritance-polymorphism.md` - Inheritance, method overriding, super(), ABC
3. `03-advanced-oop.md` - Magic methods, properties, class/static methods, dataclasses
4. `04-design-patterns.md` - Singleton, Factory, Observer, Strategy patterns

**Workshops** (4 files):
1. `workshop-01-library-system.md` - Creating classes for library management
2. `workshop-02-shape-hierarchy.md` - Inheritance and polymorphism with shapes
3. `workshop-03-banking-system.md` - Advanced OOP with banking example
4. `workshop-04-design-patterns.md` - Implementing common design patterns

**Homeworks** (4 files):
1. `homework-01-student-management.md` - Design and implement student system
2. `homework-02-vehicle-system.md` - Inheritance with vehicles
3. `homework-03-custom-container.md` - Magic methods implementation
4. `homework-04-pattern-refactoring.md` - Refactor code with design patterns

**Resources**:
- `cheatsheet.md` - OOP concepts and syntax reference
- `useful-links.md` - Python OOP resources
- `best-practices.md` - OOP design principles

**Assessments**:
- `quiz-oop-fundamentals.md`
- `quiz-design-patterns.md`
- `rubric-oop-project.md`

---

#### 🧪 Section 12-14: Testing & Quality Assurance (Module 5)
**Status**: PENDING  
**Location**: `Subject-04-Testing/`  
**Priority**: HIGH

**Required Materials**:

**Tutorials** (3 files):
1. `01-unit-testing-fundamentals.md` - Testing basics, unittest module, AAA pattern
2. `02-pytest-framework.md` - pytest features, fixtures, parametrization, mocking
3. `03-tdd-methodology.md` - TDD principles, Red-Green-Refactor, CI concepts

**Workshops** (3 files):
1. `workshop-01-first-tests.md` - Writing tests for existing code
2. `workshop-02-pytest-suite.md` - Building comprehensive test suite
3. `workshop-03-tdd-feature.md` - Building feature with TDD

**Homeworks** (3 files):
1. `homework-01-code-coverage.md` - Achieve 80% coverage
2. `homework-02-oop-tests.md` - Test OOP classes from Module 3
3. `homework-03-tdd-calculator.md` - Calculator using TDD

**Installation**:
- `pytest-setup.md` - Installing and configuring pytest

**Resources**:
- `cheatsheet.md` - Testing commands and patterns
- `best-practices.md` - Testing best practices
- `useful-links.md` - Testing resources

---

#### 📊 Section 15-16: Logging & Debugging (Module 6)
**Status**: PENDING  
**Location**: `Subject-05-Logging/`  
**Priority**: HIGH

**Required Materials**:

**Tutorials** (2 files):
1. `01-python-logging.md` - Logging module, levels, handlers, formatters
2. `02-loguru-rotation.md` - Loguru features, rotation strategies, production logging

**Workshops** (2 files):
1. `workshop-01-add-logging.md` - Adding logging to existing apps
2. `workshop-02-production-logging.md` - Setting up production-grade logging

**Homeworks** (2 files):
1. `homework-01-logging-integration.md` - Add logging to previous projects
2. `homework-02-loguru-rotation.md` - Implement Loguru with rotation

**Installation**:
- `loguru-setup.md` - Installing Loguru

**Resources**:
- `cheatsheet.md` - Logging patterns and commands
- `best-practices.md` - Logging best practices
- `log-levels-guide.md` - When to use each log level

---

#### 🌐 Section 17-20: API Development with FastAPI (Module 7)
**Status**: PENDING  
**Location**: `Subject-06-FastAPI/`  
**Priority**: HIGH

**Required Materials**:

**Tutorials** (4 files):
1. `01-fastapi-fundamentals.md` - REST, FastAPI basics, endpoints, Pydantic
2. `02-request-handling.md` - Request body, validation, error handling
3. `03-advanced-features.md` - Auth, JWT, middleware, CORS, background tasks
4. `04-database-testing.md` - SQLAlchemy, async operations, API testing

**Workshops** (4 files):
1. `workshop-01-simple-api.md` - Building first API
2. `workshop-02-user-management.md` - User management API
3. `workshop-03-authentication.md` - Implementing authentication
4. `workshop-04-database-api.md` - Complete API with database

**Homeworks** (4 files):
1. `homework-01-todo-api.md` - TODO API with CRUD
2. `homework-02-validation-errors.md` - Add validation and error handling
3. `homework-03-file-management.md` - Secure file management API
4. `homework-04-blog-api.md` - Blog API with full CRUD and tests

**Installation**:
- `fastapi-setup.md` - Installing FastAPI and Uvicorn with UV

**Resources**:
- `cheatsheet.md` - FastAPI commands and patterns
- `best-practices.md` - API design best practices
- `authentication-guide.md` - Auth implementation guide

---

#### 🕷️ Section 21-23: Web Scraping Fundamentals (Module 8)
**Status**: PENDING  
**Location**: `Subject-07-Web-Scraping/`  
**Priority**: MEDIUM

**Required Materials**:

**Tutorials** (3 files):
1. `01-scraping-basics.md` - HTML/CSS, requests, BeautifulSoup, ethics
2. `02-advanced-scraping.md` - Selenium, dynamic sites, pagination, rate limiting
3. `03-crawlee-framework.md` - Crawlee architecture, crawlers vs scrapers

**Workshops** (3 files):
1. `workshop-01-simple-scraper.md` - Scraping simple website
2. `workshop-02-dynamic-sites.md` - Scraping JavaScript-heavy sites
3. `workshop-03-crawlee-crawler.md` - Building Crawlee crawler

**Homeworks** (3 files):
1. `homework-01-multi-page-scraper.md` - Extract data from multiple pages
2. `homework-02-dynamic-scraper.md` - Scrape JavaScript-heavy site
3. `homework-03-crawlee-project.md` - Multi-page crawler with Crawlee

**Installation**:
- `scraping-tools-setup.md` - Installing scraping libraries
- `crawlee-setup.md` - Installing Crawlee

**Resources**:
- `cheatsheet.md` - Scraping commands and patterns
- `ethics-legal.md` - Ethics and legal considerations
- `troubleshooting.md` - Common scraping issues
- `useful-selectors.md` - CSS/XPath selector guide

---

#### 🎓 Section 24-25: Final Project Development (Module 9)
**Status**: PENDING  
**Location**: `Subject-08-Final-Project/`  
**Priority**: MEDIUM

**Required Materials**:

**Tutorials** (4 files):
1. `01-project-planning.md` - Requirements, architecture, design
2. `02-implementation-guide.md` - Implementation best practices
3. `03-api-integration.md` - Adding API layer to crawler
4. `04-deployment-production.md` - Deployment and production readiness

**Workshops** (4 files):
1. `workshop-01-planning-session.md` - Group project planning
2. `workshop-02-implementation-part1.md` - Guided implementation (crawler)
3. `workshop-03-implementation-part2.md` - Guided implementation (API)
4. `workshop-04-presentations.md` - Project presentations and peer review

**Project Templates**:
- `project-proposal-template.md` - Template for project proposals
- `architecture-document-template.md` - Architecture documentation template
- `README-template.md` - README template for final project

**Resources**:
- `project-requirements.md` - Detailed project requirements
- `evaluation-rubric.md` - Grading rubric
- `example-projects.md` - Example project ideas
- `deployment-guide.md` - Deployment options and guides

---

## 🤖 Instructions for AI Agents

When asked to create materials for a specific section, follow these steps:

### Step 1: Understand the Section
- Review the syllabus for the section number and topic
- Identify learning objectives
- Check prerequisites from previous sections

### Step 2: Create Folder Structure
- Create the subject folder: `Subject-XX-Topic-Name/`
- Create subfolders: `tutorials/`, `workshops/`, `homeworks/`, `resources/`, `installation/`, `assessments/`

### Step 3: Create README.md
- Write subject overview
- List learning outcomes
- Describe materials structure
- Define prerequisites
- Outline progression path

### Step 4: Create Tutorials
- Write theoretical content with examples
- Include code snippets with explanations
- Add diagrams where helpful
- Provide best practices and common pitfalls
- Link to additional resources

### Step 5: Create Workshops
- Design hands-on exercises
- Provide step-by-step instructions
- Include expected outputs
- Add troubleshooting tips
- Provide solution code

### Step 6: Create Homeworks
- Design independent assignments
- Write clear requirements
- Create evaluation rubrics
- Provide hints if needed
- Estimate completion time

### Step 7: Create Resources
- Write cheatsheet with key commands/concepts
- Compile useful links
- Create troubleshooting guide
- Add any additional reference materials

### Step 8: Create Assessments (if applicable)
- Design quizzes with multiple choice and short answer
- Create evaluation rubrics
- Provide answer keys

### Step 9: Quality Check
- Ensure all materials align with learning objectives
- Check for consistency in formatting
- Verify code examples work
- Proofread for clarity and errors

---

## 📐 Content Guidelines

### Writing Style:
- **Clear and concise**: Avoid unnecessary jargon
- **Student-friendly**: Explain concepts thoroughly
- **Practical**: Include real-world examples
- **Progressive**: Build on previous knowledge
- **Engaging**: Use analogies and relatable examples

### Code Examples:
- **Runnable**: All code should be executable
- **Commented**: Explain complex parts
- **Complete**: Include imports and setup
- **Tested**: Verify code works before including
- **Formatted**: Follow PEP 8 style guide

### Markdown Formatting:
- Use headers (# ## ###) for structure
- Use code blocks with language specification
- Use bullet points and numbered lists
- Include tables for comparisons
- Add horizontal rules (---) for sections
- Use **bold** for emphasis, `code` for inline code

---

## 🎯 Priority Order for Creation

Based on course progression and dependencies:

1. **IMMEDIATE** (Sections 6-7): Data Structures & JSON - Foundation for data manipulation
2. **HIGH** (Sections 8-11): OOP - Foundation for everything else
3. **HIGH** (Sections 12-14): Testing - Should be learned alongside OOP
4. **HIGH** (Sections 15-16): Logging - Needed before API development
5. **HIGH** (Sections 17-20): FastAPI - Major module, needs OOP and logging
6. **MEDIUM** (Sections 21-23): Web Scraping - Builds toward final project
7. **MEDIUM** (Sections 24-25): Final Project - Integrates everything

---

## 📊 Progress Tracking

### Completion Status:
- ✅ **Module 1** (Sections 1-3): Git Basics - COMPLETE
- ✅ **Module 2** (Sections 4-5): Environment & UV - COMPLETE
- ⏳ **Module 3** (Sections 6-7): Data Structures & JSON - PENDING
- ⏳ **Module 4** (Sections 8-11): OOP - PENDING
- ⏳ **Module 5** (Sections 12-14): Testing - PENDING
- ⏳ **Module 6** (Sections 15-16): Logging - PENDING
- ⏳ **Module 7** (Sections 17-20): FastAPI - PENDING
- ⏳ **Module 8** (Sections 21-23): Web Scraping - PENDING
- ⏳ **Module 9** (Sections 24-25): Final Project - PENDING

### Next Steps:
1. Create Module 3 (Data Structures & JSON) materials
2. Create Module 4 (OOP) materials
3. Create Module 5 (Testing) materials
4. Create Module 6 (Logging) materials
5. Create Module 7 (FastAPI) materials
6. Create Module 8 (Web Scraping) materials
7. Create Module 9 (Final Project) materials

---

## 🔄 Iterative Improvement

After initial creation:
- [ ] Review student feedback
- [ ] Update based on common questions
- [ ] Add more examples if needed
- [ ] Refine difficulty levels
- [ ] Update for new library versions
- [ ] Add video links if available

---

## 📞 Agent Invocation Template

**To create materials for a specific module, use this prompt:**

```
Please create all materials for Module X (Sections Y-Z): [Topic Name]

Follow the structure defined in MATERIAL-CREATION-GUIDE.md:
1. Create folder structure
2. Write README.md with overview
3. Create all tutorial files
4. Create all workshop files
5. Create all homework files
6. Create resource files (cheatsheet, links, etc.)
7. Create assessment materials

Ensure all materials:
- Align with learning objectives from COURSE-SYLLABUS.md
- Follow the writing style and formatting guidelines
- Include runnable, tested code examples
- Build progressively on previous modules
- Are practical and student-friendly

Location: Subject-0X-[Topic-Name]/
```

---

## 📝 Example Agent Commands

### Create Data Structures Module:
```
Create all materials for Module 3 (Sections 6-7): Python Data Structures & JSON API Work
Location: Subject-03-Data-Structures/
```

### Create OOP Module:
```
Create all materials for Module 4 (Sections 8-11): Object-Oriented Programming
Location: Subject-04-OOP/
```

### Create Testing Module:
```
Create all materials for Module 5 (Sections 12-14): Testing & Quality Assurance
Location: Subject-05-Testing/
```

### Create Logging Module:
```
Create all materials for Module 5 (Sections 13-14): Logging & Debugging
Location: Subject-05-Logging/
```

---

## ✅ Quality Assurance Checklist

Before considering a module complete:

- [ ] All required files created
- [ ] README is comprehensive
- [ ] Tutorials explain concepts clearly
- [ ] Workshops have step-by-step instructions
- [ ] Homeworks have clear requirements and rubrics
- [ ] Code examples are tested and working
- [ ] Resources include cheatsheet and links
- [ ] Formatting is consistent
- [ ] No spelling/grammar errors
- [ ] Materials build on previous modules appropriately
- [ ] Learning objectives are met

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Maintained By**: Course Development Team

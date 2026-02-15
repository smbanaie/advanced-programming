# Material Creation Guide - Advanced Programming in Python

This document serves as a systematic guide for creating course materials for each section. Use this as a checklist and instruction set for AI agents or instructors developing the course content.

---

## 📋 Overall Structure

The course is organized into **Topics** (formerly called Modules), where each topic contains **Sections** numbered starting from 1 within that topic.

```
Topic-XX-Topic-Name/
├── README.md                    # Topic overview and learning objectives
├── sections/
│   ├── section-01/
│   │   ├── README.md            # Section overview
│   │   ├── tutorial.md          # Theoretical content for this section
│   │   ├── workshop.md          # Hands-on exercises for this section
│   │   ├── homework.md          # Independent assignment for this section
│   │   └── resources/           # Section-specific resources
│   │       ├── cheatsheet.md
│   │       └── useful-links.md
│   ├── section-02/
│   │   └── ... (same structure)
│   └── section-03/
│       └── ... (same structure)
├── resources/                   # Topic-level resources (shared across sections)
│   ├── cheatsheet.md
│   ├── useful-links.md
│   └── troubleshooting.md
├── installation/                # Setup guides (if applicable)
│   └── *.md
└── assessments/                 # Topic-level quizzes and evaluation materials
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

## 📝 Topic-by-Topic Creation Plan

### ✅ COMPLETED TOPICS

#### ✅ **Topic 1: Development Environment & Version Control**
**Status**: COMPLETED
**Location**: `Topic-01-Development-Environment/`
**Sections**: 3 sections (numbered 1-3 within this topic)
**Materials**:
- ✅ Tutorials (3 files)
- ✅ Workshops (3 files)
- ✅ Homeworks (3 files)
- ✅ Installation guides
- ✅ Resources (cheatsheet, links)
- ✅ README

#### ✅ **Topic 2: Python Environment & Dependency Management**
**Status**: COMPLETED
**Location**: `Topic-02-Python-Environment/`
**Sections**: 2 sections (numbered 1-2 within this topic)

#### ✅ **Topic 3: Python Data Structures & JSON API Work**
**Status**: ✅ COMPLETE
**Location**: `Topic-03-Data-Structures/`
**Sections**: 2 sections (numbered 1-2 within this topic)

**Required Materials**:

**Section 1: Working with Lists and Sets** ✅ COMPLETE
- `tutorial.md` - List operations, set operations, conversions, performance
- `workshop.md` - Building data processing utilities
- `homework.md` - Implement data manipulation functions

**Section 2: JSON Handling & API Integration** ✅ COMPLETE
- `tutorial.md` - JSON handling, dictionary operations, API requests with requests
- `workshop.md` - Fetching and processing randomuser.me data
- `homework.md` - Build user data processor with JSON operations

**Topic Resources**:
- `cheatsheet.md` - Data structures and JSON operations
- `useful-links.md` - Python data structures resources
- `api-examples.md` - Common API patterns

**Installation**:
- `requests-setup.md` - Installing requests library

#### ✅ **Topic 4: Object-Oriented Programming**
**Status**: ✅ COMPLETE (4/4 sections done)
**Location**: `Topic-04-Object-Oriented-Programming/`
**Sections**: 4 sections (numbered 1-4 within this topic)

**Section 1: OOP Fundamentals** ✅ COMPLETE
- `tutorial.md` - Classes, objects, attributes, methods, __init__, self
- `workshop.md` - Creating classes for library management
- `homework.md` - Design and implement student system

**Section 2: Inheritance & Polymorphism** ✅ COMPLETE
- `tutorial.md` - Inheritance, method overriding, super(), ABC, polymorphism
- `workshop.md` - Shape hierarchy system with polymorphic operations
- `homework.md` - Vehicle management system with inheritance hierarchies

**Section 3: Advanced OOP Concepts** ✅ COMPLETE
- `tutorial.md` - Magic methods, property decorators, class/static methods, dataclasses
- `workshop.md` - Banking system with advanced OOP features
- `homework.md` - Custom container class with sequence protocol

**Section 4: Design Patterns** ✅ COMPLETE
- `tutorial.md` - Singleton, Factory, Observer, Strategy patterns + combinations
- `workshop.md` - E-commerce system integrating multiple design patterns
- `homework.md` - Game development framework with design pattern architecture

#### ✅ **Topic 5: Testing & Quality Assurance**
**Status**: PARTIALLY COMPLETE (Section 1 done, Sections 2-3 pending)
**Location**: `Topic-05-Testing-Quality-Assurance/`
**Sections**: 3 sections (numbered 1-3 within this topic)

#### ✅ **Topic 6: Logging & Debugging**
**Status**: PARTIALLY COMPLETE (Section 1 done, Section 2 pending)
**Location**: `Topic-06-Logging-Debugging/`
**Sections**: 2 sections (numbered 1-2 within this topic)

#### ✅ **Topic 7: API Development with FastAPI**
**Status**: PARTIALLY COMPLETE (Section 1 done, Sections 2-4 pending)
**Location**: `Topic-07-API-Development/`
**Sections**: 4 sections (numbered 1-4 within this topic)

#### ✅ **Topic 8: Web Scraping Fundamentals**
**Status**: PARTIALLY COMPLETE (Section 1 done, Sections 2-3 pending)
**Location**: `Topic-08-Web-Scraping/`
**Sections**: 3 sections (numbered 1-3 within this topic)

**Section 1: Web Scraping Basics** ✅ COMPLETE
- `tutorial.md` - HTML/CSS, requests, BeautifulSoup, ethics
- `workshop.md` - Scraping simple website
- `homework.md` - Extract data from multiple pages

**Section 2: Advanced Scraping Techniques** ⏳ PENDING
- `tutorial.md` - Selenium, dynamic sites, pagination, rate limiting
- `workshop.md` - Scraping JavaScript-heavy sites
- `homework.md` - Scrape JavaScript-heavy site

**Section 3: Crawlee Framework** ⏳ PENDING
- `tutorial.md` - Crawlee architecture, crawlers vs scrapers
- `workshop.md` - Building Crawlee crawler
- `homework.md` - Multi-page crawler with Crawlee


#### 🎓 **Topic 9: Final Project Development**
**Status**: PENDING
**Location**: `Topic-09-Final-Project/`
**Sections**: 2 sections (numbered 1-2 within this topic)
**Priority**: MEDIUM

**Section 1: Project Planning & Implementation**
- `tutorial.md` - Requirements, architecture, design, implementation
- `workshop.md` - Guided planning and implementation
- `homework.md` - Complete core crawler implementation

**Section 2: API Integration & Presentation**
- `tutorial.md` - API integration, testing, deployment
- `workshop.md` - Adding API layer and final presentations
- `homework.md` - Submit complete project with documentation

---

## 🤖 Instructions for AI Agents

When asked to create materials for a specific topic, follow these steps:

### Step 1: Understand the Topic
- Review the syllabus for the topic and its sections
- Identify learning objectives for each section within the topic
- Check prerequisites from previous topics

### Step 2: Create Folder Structure
- Create the topic folder: `Topic-XX-Topic-Name/`
- Create subfolders: `sections/`, `resources/`, `installation/`, `assessments/`
- For each section within the topic, create: `sections/section-01/`, `sections/section-02/`, etc.

### Step 3: Create Topic README.md
- Write topic overview covering all sections
- List learning outcomes
- Describe materials structure
- Define prerequisites
- Outline progression path through sections

### Step 4: Create Section Materials
For each section within the topic:
- Create `sections/section-XX/README.md` - Section overview
- Create `sections/section-XX/tutorial.md` - Theoretical content
- Create `sections/section-XX/workshop.md` - Hands-on exercises
- Create `sections/section-XX/homework.md` - Independent assignment
- Create `sections/section-XX/resources/` - Section-specific resources

### Step 5: Create Topic-Level Resources
- Write topic-level `resources/cheatsheet.md`
- Compile `resources/useful-links.md`
- Create `resources/troubleshooting.md` if applicable

### Step 6: Quality Check
- Ensure all materials align with learning objectives
- Check for consistency in formatting
- Verify code examples work
- Proofread for clarity and errors
- Ensure section numbering starts from 1 within each topic

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

1. **COMPLETED** (Topic 3): Data Structures & JSON - Foundation for data manipulation ✅
2. **HIGH** (Topic 4): Object-Oriented Programming - Foundation for everything else
3. **HIGH** (Topic 5): Testing & Quality Assurance - Should be learned alongside OOP
4. **HIGH** (Topic 6): Logging & Debugging - Needed before API development
5. **HIGH** (Topic 7): API Development with FastAPI - Major topic, needs OOP and logging
6. **MEDIUM** (Topic 8): Web Scraping Fundamentals - Builds toward final project
7. **MEDIUM** (Topic 9): Final Project Development - Integrates everything

---

## 📊 Progress Tracking

### Completion Status:
- ✅ **Topic 1**: Development Environment & Version Control - COMPLETE (migrated from Subject-01)
- ✅ **Topic 2**: Python Environment & Dependency Management - COMPLETE (migrated from Subject-02)
- ✅ **Topic 3**: Python Data Structures & JSON API Work - COMPLETE (2 sections)
- ✅ **Topic 4**: Object-Oriented Programming - COMPLETE (4/4 sections)
- 🔄 **Topic 5**: Testing & Quality Assurance - PARTIALLY COMPLETE (Section 1/3 done)
- 🔄 **Topic 6**: Logging & Debugging - PARTIALLY COMPLETE (Section 1/2 done)
- 🔄 **Topic 7**: API Development with FastAPI - PARTIALLY COMPLETE (Section 1/4 done)
- 🔄 **Topic 8**: Web Scraping Fundamentals - PARTIALLY COMPLETE (Section 1/3 done)
- ⏳ **Topic 9**: Final Project Development - PENDING

### Next Steps:
1. ✅ Topic 3 complete - All data structures and JSON materials ready
2. ✅ Topic 4 complete - All Object-Oriented Programming materials ready
3. Complete remaining sections for Topic 5 (Testing & Quality Assurance) - 2 sections pending
4. Complete remaining sections for Topic 6 (Logging & Debugging) - 1 section pending
5. Complete remaining sections for Topic 7 (API Development with FastAPI) - 3 sections pending
6. Complete remaining sections for Topic 8 (Web Scraping Fundamentals) - 2 sections pending
7. Create Topic 9 (Final Project Development) materials - 2 sections

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

**To create materials for a specific topic, use this prompt:**

```
Please create all materials for Topic X: [Topic Name]

Follow the structure defined in MATERIAL-CREATION-GUIDE.md:
1. Create folder structure with sections/ subdirectory
2. Write topic README.md with overview of all sections
3. For each section (numbered 1, 2, 3, ... within the topic):
   - Create sections/section-XX/ directory
   - Create tutorial.md, workshop.md, homework.md
   - Create section-specific resources
4. Create topic-level resources (cheatsheet, useful-links, etc.)
5. Create installation guides if applicable

Ensure all materials:
- Align with learning objectives from COURSE-SYLLABUS.md
- Follow the writing style and formatting guidelines
- Include runnable, tested code examples
- Build progressively on previous topics
- Are practical and student-friendly
- Number sections starting from 1 within each topic

Location: Topic-0X-[Topic-Name]/
```

---

## 📝 Example Agent Commands

### Create Data Structures Topic:
```
Create all materials for Topic 3: Python Data Structures & JSON API Work
Location: Topic-03-Data-Structures/
```

### Create OOP Topic:
```
Create all materials for Topic 4: Object-Oriented Programming
Location: Topic-04-Object-Oriented-Programming/
```

### Create Testing Topic:
```
Create all materials for Topic 5: Testing & Quality Assurance
Location: Topic-05-Testing-Quality-Assurance/
```

### Create FastAPI Topic:
```
Create all materials for Topic 7: API Development with FastAPI
Location: Topic-07-API-Development/
```

### Create Logging Topic:
```
Create all materials for Topic 6: Logging & Debugging
Location: Topic-06-Logging-Debugging/
```

### Create Web Scraping Topic:
```
Create all materials for Topic 8: Web Scraping Fundamentals
Location: Topic-08-Web-Scraping/
```

### Create Final Project Topic:
```
Create all materials for Topic 9: Final Project Development
Location: Topic-09-Final-Project/
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

---

## 🔍 **Material Completion Audit Results**

### **Audit Summary (Completed February 2026)**

Based on filesystem analysis, here are the exact missing materials:

#### **Topic 4: Object-Oriented Programming**
- ✅ **Section 1**: COMPLETE (tutorial.md, workshop.md, homework.md, README.md)
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 3**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 4**: MISSING tutorial.md, workshop.md, homework.md, README.md

#### **Topic 5: Testing & Quality Assurance**
- ✅ **Section 1**: COMPLETE (tutorial.md, workshop.md, homework.md, README.md)
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 3**: MISSING tutorial.md, workshop.md, homework.md, README.md

#### **Topic 6: Logging & Debugging**
- ✅ **Section 1**: COMPLETE (tutorial.md, workshop.md, homework.md, README.md)
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md

#### **Topic 7: API Development with FastAPI**
- ✅ **Section 1**: COMPLETE (tutorial.md, workshop.md, homework.md, README.md)
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 3**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 4**: MISSING tutorial.md, workshop.md, homework.md, README.md

#### **Topic 8: Web Scraping Fundamentals**
- ✅ **Section 1**: COMPLETE (tutorial.md, workshop.md, homework.md, README.md)
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 3**: MISSING tutorial.md, workshop.md, homework.md, README.md

#### **Topic 9: Final Project Development**
- ❌ **Section 1**: MISSING tutorial.md, workshop.md, homework.md, README.md
- ❌ **Section 2**: MISSING tutorial.md, workshop.md, homework.md, README.md

---

## 📋 **Implementation Tasks**

### **High Priority Missing Materials (Start Here)**

1. **Topic 4, Section 2: Inheritance & Polymorphism**
   - Create `Topic-04-Object-Oriented-Programming/sections/section-02/`
   - Write `tutorial.md` - Inheritance, method overriding, super(), ABC
   - Write `workshop.md` - Building shape hierarchy with polymorphism
   - Write `homework.md` - Vehicle management system with inheritance
   - Write `README.md` - Section overview

2. **Topic 4, Section 3: Advanced OOP Concepts**
   - Create `Topic-04-Object-Oriented-Programming/sections/section-03/`
   - Write `tutorial.md` - Magic methods, properties, class/static methods, dataclasses
   - Write `workshop.md` - Building banking system with advanced OOP
   - Write `homework.md` - Custom container class with magic methods
   - Write `README.md` - Section overview

3. **Topic 5, Section 2: Advanced Testing with pytest**
   - Create `Topic-05-Testing-Quality-Assurance/sections/section-02/`
   - Write `tutorial.md` - pytest features, fixtures, parametrization, mocking
   - Write `workshop.md` - Building comprehensive test suite
   - Write `homework.md` - Test OOP classes from previous topics
   - Write `README.md` - Section overview

---

## 📊 **Implementation Progress**

- **Total Missing Section Files**: 22 (3 topics × 4 files + 3 topics × 3 files + 2 topics × 2 files)
- **Completed**: Topic 4 (4 sections × 3 files = 12 files completed)
- **Estimated Implementation Time**: 2-3 weeks for remaining completion
- **Priority Order**: Topic 5 → Topic 6 → Topic 7 → Topic 8 → Topic 9

---

**Document Version**: 1.0  
**Last Updated**: February 2026  
**Maintained By**: Course Development Team

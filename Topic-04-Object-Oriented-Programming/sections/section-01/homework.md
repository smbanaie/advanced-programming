# Homework 1: Student Management System

**Topic**: 4 - Object-Oriented Programming
**Section**: 1 of 4 sections (90 min workshop + homework)
**Level**: Intermediate
**Prerequisites**: Section 1 tutorial and workshop

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Design Complete OOP Systems**: Create multiple interacting classes
2. **Implement Data Management**: Handle complex object relationships
3. **Apply Encapsulation**: Protect data integrity through proper access control
4. **Build CRUD Operations**: Create, Read, Update, Delete functionality
5. **Manage Collections**: Work with lists and dictionaries of objects
6. **Implement Business Logic**: Add validation and business rules

---

## 📋 Assignment Overview

You will build a comprehensive **Student Management System** that demonstrates professional object-oriented design. The system should manage students, courses, and enrollments with full CRUD operations.

### System Requirements

**Core Features:**
- Student management (add, update, remove, search)
- Course management (create courses, manage enrollment)
- Enrollment system (students enroll in courses)
- Grade management (record and calculate grades)
- Report generation (student transcripts, course rosters)
- Data persistence (save/load system state)

### Technical Specifications

1. **Project Structure**:
   ```
   student_management/
   ├── src/
   │   ├── __init__.py
   │   ├── student.py          # Student class
   │   ├── course.py           # Course class
   │   ├── enrollment.py       # Enrollment management
   │   ├── gradebook.py        # Grade management
   │   └── university.py       # Main system class
   ├── tests/
   │   ├── test_student.py
   │   ├── test_course.py
   │   └── test_university.py
   ├── data/
   │   └── sample_data.py      # Sample data generator
   ├── pyproject.toml          # Project configuration
   └── README.md               # Documentation
   ```

2. **Dependencies**:
   - `pydantic` for data validation
   - `pytest` for testing
   - Built-in `json` for data persistence
   - Built-in `datetime` for date handling

---

## 🛠️ Implementation Requirements

### 1. Student Class

Create `src/student.py`:

```python
"""Student class for the student management system."""

from datetime import datetime
from typing import Dict, List, Optional
from pydantic import BaseModel, validator


class Student:
    """Represents a student in the university system."""

    def __init__(self, student_id: str, name: str, email: str, major: str = "Undeclared"):
        """Initialize a student with basic information."""
        # TODO: Initialize instance variables
        # - student_id (unique identifier)
        # - name, email, major
        # - enrollment_date (current date)
        # - enrolled_courses (empty dict: course_id -> enrollment_date)
        # - grades (empty dict: course_id -> grade)

    @property
    def gpa(self) -> float:
        """Calculate and return the student's GPA."""
        # TODO: Calculate GPA from grades
        # TODO: Handle case with no grades
        # GPA scale: A=4.0, B=3.0, C=2.0, D=1.0, F=0.0

    def enroll_in_course(self, course_id: str) -> bool:
        """
        Enroll student in a course.

        Args:
            course_id: Unique course identifier

        Returns:
            bool: True if enrollment successful
        """
        # TODO: Check if already enrolled
        # TODO: Add to enrolled_courses with current date
        # TODO: Return success status

    def drop_course(self, course_id: str) -> bool:
        """
        Drop a course enrollment.

        Args:
            course_id: Course to drop

        Returns:
            bool: True if drop successful
        """
        # TODO: Check if enrolled in course
        # TODO: Remove from enrolled_courses
        # TODO: Remove any grades for the course
        # TODO: Return success status

    def record_grade(self, course_id: str, grade: str) -> bool:
        """
        Record a grade for a course.

        Args:
            course_id: Course identifier
            grade: Grade (A, B, C, D, F)

        Returns:
            bool: True if grade recorded successfully
        """
        # TODO: Validate grade is in enrolled courses
        # TODO: Validate grade format (A-F)
        # TODO: Store grade
        # TODO: Return success status

    def get_transcript(self) -> Dict[str, any]:
        """
        Generate student transcript.

        Returns:
            Dictionary with transcript information
        """
        # TODO: Return dict with student info, courses, grades, GPA

    def is_enrolled_in(self, course_id: str) -> bool:
        """Check if student is enrolled in a specific course."""
        # TODO: Check enrolled_courses dict

    def get_enrolled_course_ids(self) -> List[str]:
        """Get list of enrolled course IDs."""
        # TODO: Return list of course IDs

    @classmethod
    def create_sample_student(cls, student_id: str) -> 'Student':
        """Create a sample student for testing."""
        # TODO: Create student with sample data

    @staticmethod
    def validate_email(email: str) -> bool:
        """Validate email format."""
        # TODO: Basic email validation
```

### 2. Course Class

Create `src/course.py`:

```python
"""Course class for the university system."""

from typing import Dict, List, Optional, Set
from pydantic import BaseModel


class Course:
    """Represents a course in the university system."""

    def __init__(self, course_id: str, name: str, instructor: str, credits: int = 3, max_enrollment: int = 30):
        """Initialize a course with basic information."""
        # TODO: Initialize instance variables
        # - course_id, name, instructor, credits, max_enrollment
        # - enrolled_students (set of student_ids)
        # - description (optional)

    def enroll_student(self, student_id: str) -> bool:
        """
        Enroll a student in the course.

        Args:
            student_id: Student identifier

        Returns:
            bool: True if enrollment successful
        """
        # TODO: Check if course is full
        # TODO: Check if student already enrolled
        # TODO: Add student to enrolled_students
        # TODO: Return success status

    def drop_student(self, student_id: str) -> bool:
        """
        Remove a student from the course.

        Args:
            student_id: Student identifier

        Returns:
            bool: True if removal successful
        """
        # TODO: Check if student is enrolled
        # TODO: Remove from enrolled_students
        # TODO: Return success status

    def get_enrollment_count(self) -> int:
        """Get current enrollment count."""
        # TODO: Return number of enrolled students

    def is_full(self) -> bool:
        """Check if course is at maximum enrollment."""
        # TODO: Compare enrollment count with max_enrollment

    def has_student(self, student_id: str) -> bool:
        """Check if student is enrolled in this course."""
        # TODO: Check enrolled_students set

    def get_roster(self) -> List[str]:
        """Get list of enrolled student IDs."""
        # TODO: Return list of student IDs

    @classmethod
    def create_sample_course(cls, course_id: str) -> 'Course':
        """Create a sample course for testing."""
        # TODO: Create course with sample data

    @staticmethod
    def validate_course_id(course_id: str) -> bool:
        """Validate course ID format (e.g., 'CS101', 'MATH200')."""
        # TODO: Basic course ID validation
```

### 3. Enrollment Class

Create `src/enrollment.py`:

```python
"""Enrollment management for the student system."""

from typing import Dict, List, Tuple
from datetime import datetime
from student import Student
from course import Course


class EnrollmentManager:
    """Manages student-course enrollments."""

    def __init__(self):
        """Initialize enrollment manager."""
        # TODO: Initialize data structures
        # - enrollments dict: student_id -> set of course_ids
        # - course_enrollments dict: course_id -> set of student_ids

    def enroll_student(self, student: Student, course: Course) -> bool:
        """
        Enroll a student in a course.

        Args:
            student: Student object
            course: Course object

        Returns:
            bool: True if enrollment successful
        """
        # TODO: Validate student and course objects
        # TODO: Check course is not full
        # TODO: Update both student and course
        # TODO: Update internal data structures
        # TODO: Return success status

    def drop_student(self, student: Student, course: Course) -> bool:
        """
        Drop a student from a course.

        Args:
            student: Student object
            course: Course object

        Returns:
            bool: True if drop successful
        """
        # TODO: Validate student and course objects
        # TODO: Check student is enrolled in course
        # TODO: Update both student and course
        # TODO: Update internal data structures
        # TODO: Return success status

    def get_student_courses(self, student_id: str) -> List[str]:
        """Get all courses for a student."""
        # TODO: Return course IDs for student

    def get_course_students(self, course_id: str) -> List[str]:
        """Get all students in a course."""
        # TODO: Return student IDs for course

    def get_enrollment_stats(self) -> Dict[str, int]:
        """
        Get enrollment statistics.

        Returns:
            Dictionary with various counts
        """
        # TODO: Count total enrollments, students, courses
```

### 4. Gradebook Class

Create `src/gradebook.py`:

```python
"""Grade management system."""

from typing import Dict, List, Tuple, Optional
from student import Student
from course import Course


class Gradebook:
    """Manages student grades for courses."""

    def __init__(self):
        """Initialize gradebook."""
        # TODO: Initialize grades dict: (student_id, course_id) -> grade

    def record_grade(self, student: Student, course: Course, grade: str) -> bool:
        """
        Record a grade for a student in a course.

        Args:
            student: Student object
            course: Course object
            grade: Grade (A, B, C, D, F)

        Returns:
            bool: True if grade recorded successfully
        """
        # TODO: Validate inputs
        # TODO: Check student is enrolled in course
        # TODO: Validate grade format
        # TODO: Store grade in student and internal storage
        # TODO: Return success status

    def get_student_grades(self, student_id: str) -> Dict[str, str]:
        """Get all grades for a student."""
        # TODO: Return dict of course_id -> grade

    def get_course_grades(self, course_id: str) -> Dict[str, str]:
        """Get all grades for a course."""
        # TODO: Return dict of student_id -> grade

    def calculate_gpa(self, grades: Dict[str, str]) -> float:
        """
        Calculate GPA from grades dictionary.

        Args:
            grades: Dictionary of course_id -> grade

        Returns:
            float: GPA value
        """
        # TODO: Convert grades to GPA points
        # TODO: Calculate weighted average

    def get_course_statistics(self, course_id: str) -> Dict[str, any]:
        """
        Get grade statistics for a course.

        Returns:
            Dictionary with grade distribution, average GPA, etc.
        """
        # TODO: Analyze grades for the course
        # TODO: Return comprehensive statistics
```

### 5. University Class (Main System)

Create `src/university.py`:

```python
"""Main university management system."""

import json
from typing import Dict, List, Optional
from pathlib import Path
from student import Student
from course import Course
from enrollment import EnrollmentManager
from gradebook import Gradebook


class University:
    """Main university management system."""

    def __init__(self, name: str = "State University"):
        """Initialize university system."""
        # TODO: Initialize instance variables
        # - name, students (dict: id -> Student), courses (dict: id -> Course)
        # - enrollment_manager, gradebook

    def add_student(self, student: Student) -> bool:
        """
        Add a student to the university.

        Args:
            student: Student object

        Returns:
            bool: True if added successfully
        """
        # TODO: Validate student ID is unique
        # TODO: Add to students dict
        # TODO: Return success status

    def add_course(self, course: Course) -> bool:
        """
        Add a course to the university.

        Args:
            course: Course object

        Returns:
            bool: True if added successfully
        """
        # TODO: Validate course ID is unique
        # TODO: Add to courses dict
        # TODO: Return success status

    def enroll_student_in_course(self, student_id: str, course_id: str) -> bool:
        """
        Enroll a student in a course.

        Args:
            student_id: Student identifier
            course_id: Course identifier

        Returns:
            bool: True if enrollment successful
        """
        # TODO: Get student and course objects
        # TODO: Use enrollment manager to enroll
        # TODO: Return success status

    def record_student_grade(self, student_id: str, course_id: str, grade: str) -> bool:
        """
        Record a grade for a student in a course.

        Args:
            student_id: Student identifier
            course_id: Course identifier
            grade: Grade to record

        Returns:
            bool: True if grade recorded successfully
        """
        # TODO: Get student and course objects
        # TODO: Use gradebook to record grade
        # TODO: Return success status

    def get_student_transcript(self, student_id: str) -> Optional[Dict]:
        """Get transcript for a student."""
        # TODO: Get student object
        # TODO: Return transcript or None

    def get_course_roster(self, course_id: str) -> Optional[Dict]:
        """
        Get course roster with student information.

        Returns:
            Dictionary with course info and enrolled students
        """
        # TODO: Get course object
        # TODO: Get enrolled students
        # TODO: Return roster information

    def generate_university_report(self) -> Dict[str, any]:
        """
        Generate comprehensive university statistics.

        Returns:
            Dictionary with university-wide statistics
        """
        # TODO: Compile stats from all components
        # TODO: Return comprehensive report

    def save_data(self, filename: str = "university_data.json"):
        """
        Save university data to JSON file.

        Args:
            filename: Output filename
        """
        # TODO: Convert objects to serializable format
        # TODO: Save to JSON file

    def load_data(self, filename: str = "university_data.json"):
        """
        Load university data from JSON file.

        Args:
            filename: Input filename
        """
        # TODO: Load from JSON file
        # TODO: Recreate objects from data
        # TODO: Handle file not found
```

---

## 📋 Submission Requirements

### Required Deliverables

1. **Complete Source Code**:
   - All classes implemented with proper methods
   - Comprehensive error handling and validation
   - Type hints and docstrings for all methods

2. **Comprehensive Tests**:
   - Unit tests for each class
   - Integration tests for system interactions
   - Edge case testing and error conditions

3. **Sample Data and Demo**:
   - Sample data generation functions
   - Demonstration script showing system usage
   - Example reports and outputs

4. **Documentation**:
   - Complete README with setup and usage instructions
   - API documentation for all classes and methods
   - Example code and use cases

### Testing the System

```bash
# Install dependencies
pip install pydantic pytest

# Run tests
pytest

# Run demo
python demo.py
```

### Key Features to Verify

- [ ] Student CRUD operations (create, read, update, delete)
- [ ] Course management with enrollment limits
- [ ] Enrollment system with validation
- [ ] Grade recording and GPA calculation
- [ ] Report generation (transcripts, rosters, statistics)
- [ ] Data persistence (save/load functionality)
- [ ] Error handling for all operations
- [ ] Comprehensive test coverage

---

## 🎯 Evaluation Criteria

### OOP Design (30%)
- [ ] Proper class design with clear responsibilities
- [ ] Effective use of encapsulation and data hiding
- [ ] Appropriate use of instance vs class variables
- [ ] Clean method signatures with proper parameters
- [ ] Meaningful class and method names

### Functionality (35%)
- [ ] All CRUD operations work correctly
- [ ] Enrollment system handles business rules
- [ ] Grade management and GPA calculation accurate
- [ ] Report generation provides useful information
- [ ] Data persistence works reliably
- [ ] Error handling prevents system crashes

### Code Quality (20%)
- [ ] Comprehensive docstrings and type hints
- [ ] Proper error handling and validation
- [ ] Clean, readable code following PEP 8
- [ ] Modular design with appropriate separation of concerns
- [ ] Effective use of Python language features

### Testing & Documentation (15%)
- [ ] Comprehensive test coverage (80%+)
- [ ] Tests verify both success and failure cases
- [ ] Clear documentation and usage examples
- [ ] Working demonstration of system capabilities
- [ ] Professional project structure

---

## 🚀 Bonus Challenges

### Advanced Features
1. **Database Integration**: Replace JSON persistence with SQLite database
2. **Web Interface**: Add Flask/FastAPI web interface for the system
3. **Advanced Reporting**: Add data visualization and advanced analytics
4. **User Authentication**: Implement role-based access control
5. **Audit Logging**: Track all system changes with timestamps

### System Enhancements
1. **Prerequisites System**: Add course prerequisite validation
2. **Waitlist Management**: Handle course capacity limits with waitlists
3. **Grade Appeals**: System for students to appeal grades
4. **Email Notifications**: Send notifications for important events
5. **Backup and Recovery**: Automated backup and restore functionality

---

## 📞 Getting Help

### Common Issues
- **Object Relationships**: Ensure proper linking between students, courses, and enrollments
- **Data Consistency**: Keep enrollment data synchronized between objects
- **Grade Validation**: Ensure grades are only recorded for enrolled students
- **Memory Management**: Be careful with object references in collections
- **File I/O**: Handle JSON serialization/deserialization properly

### Resources
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)
- [Python Data Model](https://docs.python.org/3/reference/datamodel.html)
- [OOP Design Patterns](https://realpython.com/python3-object-oriented-programming/)
- [pytest Documentation](https://docs.pytest.org/)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **Class Design**: Creating well-structured, encapsulated classes
- **Object Relationships**: Managing complex interactions between objects
- **Data Management**: Implementing CRUD operations with validation
- **Business Logic**: Adding rules and constraints to object behavior
- **System Architecture**: Designing modular, maintainable systems
- **Testing Practices**: Writing comprehensive tests for OOP code
- **Documentation**: Creating clear, professional code documentation

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 10-15 hours
**Total Points**: 100
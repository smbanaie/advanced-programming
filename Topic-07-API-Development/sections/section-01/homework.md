# Homework 1: TODO API with CRUD Operations

**Topic**: 7 - API Development with FastAPI
**Section**: 1 of 4 sections (90 min workshop + homework)
**Level**: Intermediate
**Prerequisites**: Section 1 tutorial and workshop

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Build Complete APIs**: Create production-ready REST APIs with FastAPI
2. **Implement CRUD Operations**: Handle Create, Read, Update, Delete operations
3. **Use Data Models**: Define Pydantic models for data validation
4. **Handle HTTP Requests**: Process different HTTP methods and status codes
5. **Provide Documentation**: Generate automatic API documentation
6. **Manage Application State**: Implement in-memory data storage
7. **Test API Endpoints**: Verify API functionality with HTTP clients

---

## 📋 Assignment Overview

You will build a complete **TODO Management API** using FastAPI. The API should allow users to create, read, update, and delete TODO items with proper validation, error handling, and documentation. This is a fundamental exercise in REST API development that demonstrates all core concepts learned in Section 1.

### Project Requirements

**Core Features:**
- Create TODO items with title, description, priority, and due date
- Retrieve individual TODOs and lists of TODOs
- Update existing TODO items
- Delete TODO items
- Filter and search TODOs by various criteria
- Proper error handling and validation
- Automatic API documentation

### Technical Specifications

1. **Project Structure**:
   ```
   todo-api/
   ├── main.py              # FastAPI application
   ├── models.py            # Pydantic data models
   ├── database.py          # In-memory data storage
   ├── pyproject.toml       # Project configuration
   └── README.md            # Documentation
   ```

2. **Dependencies**:
   - `fastapi` - Web framework
   - `uvicorn` - ASGI server
   - Optional: `pydantic[email]` for email validation

---

## 🛠️ Implementation Requirements

### 1. Data Models

Create `models.py` with Pydantic models:

```python
from pydantic import BaseModel, Field
from typing import Optional, List
from datetime import datetime
from enum import Enum

class Priority(str, Enum):
    """TODO priority levels."""
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    URGENT = "urgent"

class Status(str, Enum):
    """TODO status."""
    PENDING = "pending"
    IN_PROGRESS = "in_progress"
    COMPLETED = "completed"

class TodoCreate(BaseModel):
    """Model for creating new TODO items."""
    # TODO: Define fields:
    # - title: str, required, 1-100 characters
    # - description: str, optional, max 500 characters
    # - priority: Priority, default MEDIUM
    # - due_date: Optional[datetime], must be future date if provided
    # - tags: List[str], optional, default empty list

class TodoUpdate(BaseModel):
    """Model for updating TODO items."""
    # TODO: Define fields (all optional):
    # - title, description, priority, due_date, tags, status

class TodoResponse(BaseModel):
    """Model for TODO responses."""
    # TODO: Define fields:
    # - id: int
    # - title, description, priority, due_date, tags
    # - status: Status, default PENDING
    # - created_at: datetime
    # - updated_at: datetime

class TodoList(BaseModel):
    """Model for paginated TODO lists."""
    # TODO: Define fields:
    # - todos: List[TodoResponse]
    # - total: int
    # - page: int
    # - per_page: int
    # - has_next: bool
    # - has_prev: bool
```

### 2. In-Memory Database

Create `database.py` for data storage:

```python
from typing import Dict, List, Optional
from models import TodoResponse, TodoCreate, TodoUpdate, Priority, Status
from datetime import datetime

class TodoDatabase:
    """In-memory database for TODO items."""

    def __init__(self):
        """Initialize the database."""
        # TODO: Initialize storage
        # - todos: Dict[int, TodoResponse]
        # - next_id: int = 1

    def create_todo(self, todo_data: TodoCreate) -> TodoResponse:
        """Create a new TODO item."""
        # TODO: Create TodoResponse from TodoCreate
        # TODO: Assign ID and timestamps
        # TODO: Store in database
        # TODO: Return created todo

    def get_todo(self, todo_id: int) -> Optional[TodoResponse]:
        """Get a TODO item by ID."""
        # TODO: Return todo or None

    def get_all_todos(self) -> List[TodoResponse]:
        """Get all TODO items."""
        # TODO: Return list of all todos

    def update_todo(self, todo_id: int, update_data: TodoUpdate) -> Optional[TodoResponse]:
        """Update a TODO item."""
        # TODO: Find existing todo
        # TODO: Apply updates
        # TODO: Update timestamp
        # TODO: Return updated todo

    def delete_todo(self, todo_id: int) -> bool:
        """Delete a TODO item."""
        # TODO: Remove todo if exists
        # TODO: Return success status

    def search_todos(
        self,
        priority: Optional[Priority] = None,
        status: Optional[Status] = None,
        tag: Optional[str] = None,
        overdue: Optional[bool] = None
    ) -> List[TodoResponse]:
        """Search TODOs by criteria."""
        # TODO: Filter todos based on criteria
        # TODO: Return filtered list

    def get_paginated_todos(
        self,
        page: int = 1,
        per_page: int = 10,
        priority: Optional[Priority] = None,
        status: Optional[Status] = None
    ) -> Dict:
        """Get paginated TODO list."""
        # TODO: Filter todos
        # TODO: Apply pagination
        # TODO: Return paginated result
```

### 3. FastAPI Application

Create `main.py` with the API endpoints:

```python
from fastapi import FastAPI, HTTPException, Query, Path, status
from typing import Optional, List
from models import TodoCreate, TodoUpdate, TodoResponse, TodoList, Priority, Status
from database import TodoDatabase

app = FastAPI(
    title="TODO API",
    description="A REST API for managing TODO items",
    version="1.0.0",
    contact={
        "name": "API Homework",
        "email": "homework@example.com",
    }
)

# Initialize database
db = TodoDatabase()

# TODO: Implement these endpoints:

# POST /todos/ - Create todo
@app.post("/todos/", response_model=TodoResponse, status_code=status.HTTP_201_CREATED)
async def create_todo(todo: TodoCreate):
    """Create a new TODO item."""
    pass

# GET /todos/ - List todos with pagination and filtering
@app.get("/todos/", response_model=TodoList)
async def list_todos(
    page: int = Query(1, ge=1),
    per_page: int = Query(10, ge=1, le=100),
    priority: Optional[Priority] = None,
    status: Optional[Status] = None
):
    """List TODO items with pagination and filtering."""
    pass

# GET /todos/{todo_id} - Get specific todo
@app.get("/todos/{todo_id}", response_model=TodoResponse)
async def get_todo(todo_id: int = Path(..., gt=0)):
    """Get a specific TODO item by ID."""
    pass

# PUT /todos/{todo_id} - Update todo
@app.put("/todos/{todo_id}", response_model=TodoResponse)
async def update_todo(todo_id: int = Path(..., gt=0), todo: TodoUpdate):
    """Update an existing TODO item."""
    pass

# DELETE /todos/{todo_id} - Delete todo
@app.delete("/todos/{todo_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_todo(todo_id: int = Path(..., gt=0)):
    """Delete a TODO item."""
    pass

# GET /todos/search - Search todos
@app.get("/todos/search", response_model=List[TodoResponse])
async def search_todos(
    priority: Optional[Priority] = None,
    status: Optional[Status] = None,
    tag: Optional[str] = None,
    overdue: Optional[bool] = None
):
    """Search TODO items by criteria."""
    pass

# GET /stats - Get todo statistics
@app.get("/stats")
async def get_stats():
    """Get TODO statistics."""
    # TODO: Return counts by priority, status, etc.
    pass

# Health check
@app.get("/health")
async def health_check():
    """API health check."""
    return {
        "status": "healthy",
        "timestamp": datetime.now(),
        "total_todos": len(db.get_all_todos())
    }

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000, reload=True)
```

---

## 📋 Submission Requirements

### Required Deliverables

1. **Complete API Implementation**:
   - All required endpoints implemented
   - Proper error handling and validation
   - Complete data models and database

2. **API Documentation**:
   - Automatic docs accessible at `/docs`
   - Clear endpoint descriptions and examples
   - Proper response schemas

3. **Testing Evidence**:
   - API tested with HTTP client (curl/HTTPie/Postman)
   - Screenshots of API docs or test results
   - Sample requests and responses

4. **Code Quality**:
   - Proper type hints and docstrings
   - PEP 8 compliant code
   - Modular, readable structure

### API Testing Checklist

**Core CRUD Operations:**
- [ ] Create TODO with valid data
- [ ] Create TODO with invalid data (validation errors)
- [ ] List all TODOs
- [ ] List TODOs with pagination
- [ ] List TODOs with filtering
- [ ] Get specific TODO by ID
- [ ] Get non-existent TODO (404 error)
- [ ] Update existing TODO
- [ ] Update non-existent TODO (404 error)
- [ ] Delete existing TODO
- [ ] Delete non-existent TODO (404 error)

**Advanced Features:**
- [ ] Search by priority
- [ ] Search by status
- [ ] Search by tag
- [ ] Search for overdue items
- [ ] Get statistics
- [ ] Health check endpoint

---

## 🎯 Evaluation Criteria

### API Functionality (40%)
- [ ] All CRUD endpoints work correctly
- [ ] Proper HTTP status codes returned
- [ ] Request/response validation implemented
- [ ] Error handling for edge cases
- [ ] Pagination and filtering work properly
- [ ] Search functionality implemented

### Data Models & Validation (25%)
- [ ] Pydantic models properly defined
- [ ] Input validation works correctly
- [ ] Response models control output format
- [ ] Enum types properly implemented
- [ ] Optional fields handled correctly
- [ ] Date/time validation implemented

### Code Quality (20%)
- [ ] Clean, readable code structure
- [ ] Proper type hints throughout
- [ ] Comprehensive docstrings
- [ ] Modular design (separate concerns)
- [ ] PEP 8 compliance
- [ ] Error messages are helpful

### Documentation & Testing (15%)
- [ ] Automatic API docs are complete
- [ ] Interactive documentation tested
- [ ] API tested with HTTP client
- [ ] Test evidence provided
- [ ] README with usage instructions
- [ ] Sample requests documented

---

## 🚀 Bonus Challenges

### Advanced Features
1. **Categories and Tags**: Add category management and advanced tagging
2. **Due Date Reminders**: Implement reminder system for upcoming due dates
3. **Bulk Operations**: Add endpoints for bulk create/update/delete
4. **Export Functionality**: Add CSV/JSON export capabilities
5. **User Authentication**: Add basic user management and todo ownership

### API Enhancements
1. **Rate Limiting**: Implement basic rate limiting for API endpoints
2. **Caching**: Add response caching for frequently accessed data
3. **API Versioning**: Implement API versioning (v1, v2)
4. **Webhooks**: Add webhook notifications for todo updates
5. **Metrics**: Add basic API usage metrics and monitoring

---

## 📞 Getting Help

### Common Issues
- **Model Validation**: Ensure Pydantic models are properly imported and used
- **Path Parameters**: Remember to use Path(...) for path parameter validation
- **Status Codes**: Use appropriate HTTP status codes from fastapi.status
- **Response Models**: Apply response_model to control output format
- **Datetime Handling**: Use proper datetime validation and serialization

### Testing Tips
- Use the interactive docs at `/docs` to test endpoints
- Use curl or HTTPie for command-line testing
- Test both success and error scenarios
- Verify pagination and filtering work correctly

### Resources
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)
- [REST API Design Guide](https://restfulapi.net/)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **FastAPI Fundamentals**: Application setup, routing, and configuration
- **REST API Design**: Proper HTTP methods, status codes, and resource design
- **Data Validation**: Pydantic models for input/output validation
- **CRUD Operations**: Complete Create, Read, Update, Delete functionality
- **Error Handling**: Proper HTTP exceptions and error responses
- **API Documentation**: Automatic interactive documentation generation
- **Type Safety**: Python type hints for API development
- **Testing**: Manual API testing with HTTP clients

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 6-8 hours
**Total Points**: 100
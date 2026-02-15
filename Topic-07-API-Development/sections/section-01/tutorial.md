# Tutorial 1: FastAPI Fundamentals

**Section**: 1 - FastAPI Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-6 (Basic Python, OOP, Testing, Logging)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Understand REST Concepts**: Apply RESTful API design principles
2. **Create FastAPI Applications**: Set up and run FastAPI applications
3. **Implement API Endpoints**: Build GET, POST, PUT, DELETE endpoints
4. **Handle Parameters**: Work with path parameters, query parameters, and request bodies
5. **Use Pydantic Models**: Define data models with automatic validation
6. **Generate Documentation**: Access automatic API documentation
7. **Run API Servers**: Start development servers with Uvicorn

---

## 📚 Table of Contents

1. [What is REST?](#what-is-rest)
2. [FastAPI Introduction](#fastapi-introduction)
3. [Your First FastAPI Application](#your-first-fastapi-application)
4. [Path Parameters](#path-parameters)
5. [Query Parameters](#query-parameters)
6. [Request Bodies](#request-bodies)
7. [Pydantic Models](#pydantic-models)
8. [Response Models](#response-models)
9. [Status Codes](#status-codes)
10. [Running the API](#running-the-api)
11. [API Documentation](#api-documentation)
12. [Best Practices](#best-practices)

---

## 🤔 What is REST?

REST (Representational State Transfer) is an architectural style for designing networked applications. REST APIs use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

### REST Principles

1. **Client-Server Architecture**: Separation between client and server
2. **Stateless**: Each request contains all necessary information
3. **Cacheable**: Responses can be cached to improve performance
4. **Uniform Interface**: Consistent way to interact with resources
5. **Layered System**: Client doesn't know if it's talking directly to server

### HTTP Methods in REST

```python
# CRUD Operations with HTTP Methods
GET    /users      # Read all users (Retrieve)
POST   /users      # Create new user (Create)
GET    /users/123  # Read specific user (Retrieve)
PUT    /users/123  # Update user completely (Update)
PATCH  /users/123  # Update user partially (Update)
DELETE /users/123  # Delete user (Delete)
```

### Resource-Based URLs

```python
# Good RESTful URLs
GET    /users          # Get all users
GET    /users/123      # Get user with ID 123
POST   /users          # Create new user
PUT    /users/123      # Update user 123
DELETE /users/123      # Delete user 123

# Bad non-RESTful URLs
GET    /getAllUsers
POST   /createNewUser
GET    /findUser?id=123
POST   /updateUser
```

---

## ⚡ FastAPI Introduction

FastAPI is a modern, fast web framework for building APIs with Python 3.7+ based on standard Python type hints.

### Why FastAPI?

- **Fast**: Very high performance, on par with NodeJS and Go
- **Fast to code**: Increase development speed by 200-300%
- **Fewer bugs**: Reduce human-induced errors by 40%
- **Intuitive**: Great editor support with auto-completion
- **Easy**: Designed to be easy to use and learn
- **Short**: Minimize code duplication
- **Robust**: Get production-ready code with automatic documentation

### Installation

```bash
# Install FastAPI and Uvicorn
pip install fastapi uvicorn

# Optional: Install for development
pip install "uvicorn[standard]"
```

---

## 🚀 Your First FastAPI Application

### Basic Application

```python
from fastapi import FastAPI

# Create FastAPI application instance
app = FastAPI()

# Define a simple endpoint
@app.get("/")
async def read_root():
    return {"message": "Hello World"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}

# Run with: uvicorn main:app --reload
```

### Understanding the Code

- **`FastAPI()`**: Creates the application instance
- **`@app.get("/")`**: Decorator defining a GET endpoint at root path
- **`async def`**: Async function (optional but recommended)
- **Return dictionaries**: Automatically converted to JSON

### Running the Application

```bash
# Save as main.py, then run:
uvicorn main:app --reload

# Output:
# INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
# INFO:     Started reloader process [xxxxx] using stat
# INFO:     Started server process [xxxxx]
# INFO:     Waiting for application startup.
# INFO:     Application startup complete.
```

### Testing the API

```bash
# Test with curl
curl http://127.0.0.1:8000/
# {"message":"Hello World"}

curl http://127.0.0.1:8000/items/42
# {"item_id":42}
```

---

## 🛤️ Path Parameters

Path parameters are dynamic parts of the URL path that are passed to the function.

### Basic Path Parameters

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
async def read_user(user_id: int):
    return {"user_id": user_id}

@app.get("/posts/{post_id}")
async def read_post(post_id: str):
    return {"post_id": post_id, "title": f"Post {post_id}"}
```

### Path Parameter Types

```python
from fastapi import FastAPI
from enum import Enum

app = FastAPI()

class UserType(str, Enum):
    ADMIN = "admin"
    USER = "user"
    GUEST = "guest"

@app.get("/users/{user_id}")
async def read_user(user_id: int):
    """Get user by ID - integer parameter"""
    return {"user_id": user_id, "type": "integer"}

@app.get("/users/{user_name}")
async def read_user_by_name(user_name: str):
    """Get user by name - string parameter"""
    return {"user_name": user_name, "type": "string"}

@app.get("/users/type/{user_type}")
async def read_users_by_type(user_type: UserType):
    """Get users by type - enum parameter"""
    return {"user_type": user_type, "type": "enum"}

@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    """Get file by path - path parameter"""
    return {"file_path": file_path, "type": "path"}
```

### Multiple Path Parameters

```python
@app.get("/users/{user_id}/posts/{post_id}")
async def read_user_post(user_id: int, post_id: int):
    """Get specific post from specific user"""
    return {
        "user_id": user_id,
        "post_id": post_id,
        "content": f"Post {post_id} by user {user_id}"
    }

@app.get("/api/v{version}/users/{user_id}")
async def read_user_v2(version: int, user_id: int):
    """API versioning with path parameters"""
    return {
        "api_version": version,
        "user_id": user_id,
        "endpoint": f"v{version}/users/{user_id}"
    }
```

---

## ❓ Query Parameters

Query parameters are optional parameters that come after the `?` in URLs.

### Basic Query Parameters

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/")
async def read_items(skip: int = 0, limit: int = 10):
    """Get items with pagination"""
    return {
        "skip": skip,
        "limit": limit,
        "message": f"Returning items {skip} to {skip + limit}"
    }

# Usage:
# GET /items/           -> skip=0, limit=10
# GET /items/?skip=20   -> skip=20, limit=10
# GET /items/?limit=5   -> skip=0, limit=5
# GET /items/?skip=10&limit=20 -> skip=10, limit=20
```

### Query Parameters with Validation

```python
from fastapi import FastAPI, Query
from typing import Optional

app = FastAPI()

@app.get("/search/")
async def search_items(
    q: Optional[str] = Query(None, max_length=50),
    category: str = Query("all", regex="^(all|electronics|books|clothing)$"),
    price_min: float = Query(0, ge=0),
    price_max: Optional[float] = Query(None, gt=0),
    in_stock: bool = Query(True)
):
    """Search items with multiple filters"""
    return {
        "query": q,
        "category": category,
        "price_range": {"min": price_min, "max": price_max},
        "in_stock_only": in_stock,
        "results": "mock search results"
    }

# Valid queries:
# GET /search/?q=laptop&category=electronics&price_min=500&price_max=1500&in_stock=true
# GET /search/?category=books&in_stock=false

# Invalid queries (will return validation errors):
# GET /search/?q=very_long_query_that_exceeds_max_length_limit
# GET /search/?category=invalid_category
# GET /search/?price_min=-100
```

### Required vs Optional Query Parameters

```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/filter/")
async def filter_items(
    required_param: str = Query(..., min_length=1),  # Required with validation
    optional_param: Optional[str] = None,             # Optional, defaults to None
    default_param: str = "default_value"              # Optional with default
):
    """Demonstrate required vs optional query parameters"""
    return {
        "required": required_param,
        "optional": optional_param,
        "default": default_param
    }

# Usage:
# GET /filter/?required_param=test          -> Works
# GET /filter/?required_param=test&optional_param=value -> Works
# GET /filter/                              -> Error: required_param is required
# GET /filter/?required_param=              -> Error: min_length validation
```

---

## 📦 Request Bodies

Request bodies are used to send data in POST, PUT, and PATCH requests.

### Basic Request Body

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    is_offer: Optional[bool] = None

@app.post("/items/")
async def create_item(item: Item):
    """Create a new item"""
    return {
        "item": item,
        "message": f"Item '{item.name}' created successfully"
    }

# Usage:
# POST /items/
# Content-Type: application/json
# {"name": "laptop", "price": 999.99, "is_offer": true}
```

### Complex Request Bodies

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional
from datetime import datetime

app = FastAPI()

class Address(BaseModel):
    street: str
    city: str
    country: str
    postal_code: Optional[str] = None

class User(BaseModel):
    username: str
    email: str
    full_name: Optional[str] = None
    addresses: List[Address] = []
    created_at: Optional[datetime] = None

@app.post("/users/")
async def create_user(user: User):
    """Create a new user with complex data"""
    # Simulate setting created_at if not provided
    if user.created_at is None:
        user.created_at = datetime.now()

    return {
        "user": user,
        "message": f"User {user.username} created",
        "address_count": len(user.addresses)
    }

# Usage:
# POST /users/
# {
#   "username": "john_doe",
#   "email": "john@example.com",
#   "full_name": "John Doe",
#   "addresses": [
#     {"street": "123 Main St", "city": "Anytown", "country": "USA"},
#     {"street": "456 Oak Ave", "city": "Somewhere", "country": "USA", "postal_code": "12345"}
#   ]
# }
```

### Embedding Request Bodies

```python
from fastapi import FastAPI, Body
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    item: Item,
    user: dict = Body(...),  # Required additional body
    importance: int = Body(5, ge=1, le=10)  # Additional field with validation
):
    """Update item with additional body fields"""
    result = item.dict()
    result.update({
        "item_id": item_id,
        "user": user,
        "importance": importance
    })
    return result

# Usage:
# PUT /items/42
# {
#   "item": {"name": "laptop", "price": 999.99},
#   "user": {"username": "admin", "role": "moderator"},
#   "importance": 8
# }
```

---

## 🛡️ Pydantic Models

Pydantic provides data validation and parsing using Python type hints.

### Basic Models

```python
from pydantic import BaseModel
from typing import Optional

class User(BaseModel):
    id: int
    name: str
    email: str
    age: Optional[int] = None
    is_active: bool = True

# Automatic validation
user = User(id=1, name="John", email="john@example.com")
print(user.id)        # 1
print(user.name)      # John
print(user.is_active) # True (default)

# Type conversion
user2 = User(id="123", name="Jane", email="jane@example.com", age="25")
print(user2.id)  # 123 (converted from string)
print(user2.age) # 25 (converted from string)
```

### Model Validation

```python
from pydantic import BaseModel, validator
from typing import List

class Product(BaseModel):
    name: str
    price: float
    tags: List[str] = []
    description: Optional[str] = None

    @validator('price')
    def price_must_be_positive(cls, v):
        if v <= 0:
            raise ValueError('Price must be positive')
        return v

    @validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v.strip()

# Valid usage
product = Product(
    name="Laptop",
    price=999.99,
    tags=["electronics", "computer"],
    description="A powerful laptop"
)

# Invalid usage (will raise ValidationError)
try:
    invalid_product = Product(name="", price=-100)
except ValidationError as e:
    print(f"Validation error: {e}")
```

### Field Configuration

```python
from pydantic import BaseModel, Field
from typing import Optional

class AdvancedUser(BaseModel):
    username: str = Field(..., min_length=3, max_length=20, regex=r'^[a-zA-Z0-9_]+$')
    email: str = Field(..., description="User's email address")
    age: Optional[int] = Field(None, ge=0, le=150, description="Age in years")
    password: str = Field(..., min_length=8, exclude=True)  # Not included in JSON

    class Config:
        schema_extra = {
            "example": {
                "username": "john_doe",
                "email": "john@example.com",
                "age": 30
            }
        }

# Usage
user = AdvancedUser(
    username="john_doe",
    email="john@example.com",
    password="secure_password_123",
    age=30
)

# Convert to dict (password excluded)
user_dict = user.dict()
print(user_dict)  # {'username': 'john_doe', 'email': 'john@example.com', 'age': 30}

# JSON schema
schema = AdvancedUser.schema()
print(schema)
```

---

## 📤 Response Models

Response models define the structure of API responses.

### Basic Response Models

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List

app = FastAPI()

class User(BaseModel):
    id: int
    name: str
    email: str

class UserResponse(BaseModel):
    success: bool
    data: User
    message: str

@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int):
    """Get user with structured response"""
    # Simulate database lookup
    user_data = {
        "id": user_id,
        "name": f"User {user_id}",
        "email": f"user{user_id}@example.com"
    }

    return UserResponse(
        success=True,
        data=user_data,
        message=f"User {user_id} retrieved successfully"
    )
```

### Response Model Benefits

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    description: Optional[str] = None

class ItemList(BaseModel):
    items: List[Item]
    total: int
    page: int
    per_page: int

@app.get("/items/", response_model=ItemList)
async def get_items(page: int = 1, per_page: int = 10):
    """Get paginated list of items"""
    # Simulate database query
    all_items = [
        {"name": "Item 1", "price": 10.99, "description": "First item"},
        {"name": "Item 2", "price": 15.50},
        {"name": "Item 3", "price": 8.75, "description": "Third item"},
    ]

    # Simple pagination
    start = (page - 1) * per_page
    end = start + per_page
    items = all_items[start:end]

    return ItemList(
        items=items,
        total=len(all_items),
        page=page,
        per_page=per_page
    )

# Response will only include fields defined in ItemList model
# Extra fields from internal data are filtered out
```

### Union Response Models

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import Union

app = FastAPI()

class SuccessResponse(BaseModel):
    success: bool = True
    data: dict
    message: str

class ErrorResponse(BaseModel):
    success: bool = False
    error: str
    code: int

# Union type for different response types
@app.get("/api/result/{status}")
async def get_result(status: str):
    """Return different response types based on status"""
    if status == "success":
        return SuccessResponse(
            data={"result": "operation completed"},
            message="Success!"
        )
    else:
        return ErrorResponse(
            error="Operation failed",
            code=500
        )
```

---

## 📊 Status Codes

HTTP status codes indicate the result of an API request.

### Common Status Codes in FastAPI

```python
from fastapi import FastAPI, HTTPException, status

app = FastAPI()

@app.post("/users/", status_code=status.HTTP_201_CREATED)
async def create_user(user_data: dict):
    """Create user - returns 201 Created"""
    # User creation logic
    return {"user_id": 123, "message": "User created"}

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    """Get user - various status codes"""
    if user_id == 999:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="User not found"
        )

    if user_id < 0:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="User ID must be positive"
        )

    return {"user_id": user_id, "name": f"User {user_id}"}

@app.delete("/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_user(user_id: int):
    """Delete user - returns 204 No Content"""
    # Deletion logic
    return  # No response body for 204
```

### Custom Status Codes

```python
from fastapi import FastAPI, Response

app = FastAPI()

@app.post("/custom-status/")
async def custom_status_operation(operation: str, response: Response):
    """Return custom status codes based on operation"""
    if operation == "success":
        response.status_code = 200
        return {"result": "Operation successful"}
    elif operation == "created":
        response.status_code = 201
        return {"result": "Resource created"}
    elif operation == "accepted":
        response.status_code = 202
        return {"result": "Operation accepted for processing"}
    else:
        response.status_code = 400
        return {"error": "Unknown operation"}
```

---

## ▶️ Running the API

### Development Server

```python
# main.py
from fastapi import FastAPI

app = FastAPI(title="My API", description="A simple FastAPI application")

@app.get("/")
async def root():
    return {"message": "Hello World"}

@app.get("/health")
async def health_check():
    return {"status": "healthy"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000, reload=True)
```

### Running Commands

```bash
# Basic run
python main.py

# Or with uvicorn directly
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Production run
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

### Environment Variables

```bash
# Set environment variables
export UVICORN_HOST=0.0.0.0
export UVICORN_PORT=8000
export UVICORN_RELOAD=true

# Run with environment variables
uvicorn main:app
```

---

## 📚 API Documentation

FastAPI automatically generates interactive API documentation.

### Automatic Documentation URLs

When you run your FastAPI application:

- **Swagger UI**: `http://127.0.0.1:8000/docs`
- **ReDoc**: `http://127.0.0.1:8000/redoc`
- **OpenAPI Schema**: `http://127.0.0.1:8000/openapi.json`

### Enhancing Documentation

```python
from fastapi import FastAPI, Query
from pydantic import BaseModel, Field

app = FastAPI(
    title="User Management API",
    description="A comprehensive API for managing users",
    version="1.0.0",
    contact={
        "name": "API Support",
        "email": "support@example.com",
    },
    license_info={
        "name": "MIT",
    },
)

class User(BaseModel):
    username: str = Field(..., description="Unique username", example="john_doe")
    email: str = Field(..., description="User's email address", example="john@example.com")
    age: int = Field(..., ge=0, le=150, description="Age in years", example=25)

    class Config:
        schema_extra = {
            "example": {
                "username": "john_doe",
                "email": "john@example.com",
                "age": 25
            }
        }

@app.post(
    "/users/",
    response_description="The created user",
    summary="Create a new user",
    description="Create a new user with the provided information."
)
async def create_user(user: User):
    """Create a new user."""
    return {"user": user, "message": "User created successfully"}

@app.get("/users/", summary="Get users", description="Retrieve a list of users with optional filtering")
async def get_users(
    limit: int = Query(10, description="Maximum number of users to return"),
    offset: int = Query(0, description="Number of users to skip"),
    active_only: bool = Query(True, description="Return only active users")
):
    """Get users with pagination and filtering."""
    return {
        "users": [],
        "limit": limit,
        "offset": offset,
        "active_only": active_only
    }
```

---

## 🎯 Best Practices

### API Design

```python
# Good: Consistent resource naming
@app.get("/users/{user_id}")        # ✓ Good
@app.get("/user-details/{id}")      # ✗ Inconsistent

# Good: Proper HTTP status codes
@app.post("/users/", status_code=201)  # Created
@app.delete("/users/{user_id}", status_code=204)  # No Content

# Good: Meaningful error messages
raise HTTPException(
    status_code=404,
    detail="User with ID 123 not found"
)
```

### Model Design

```python
# Good: Separate models for input/output
class UserCreate(BaseModel):
    username: str
    email: str
    password: str

class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    created_at: datetime

@app.post("/users/", response_model=UserResponse)
async def create_user(user: UserCreate):
    # Implementation
    pass

# Good: Field validation
class Product(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    price: float = Field(..., gt=0)
    category: str = Field(..., regex=r'^[a-zA-Z ]+$')
```

### Error Handling

```python
# Good: Consistent error responses
from fastapi import HTTPException

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    user = find_user(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")

    if not user.is_active:
        raise HTTPException(status_code=403, detail="User is deactivated")

    return user

# Good: Custom exception handlers
from fastapi import Request, status
from fastapi.responses import JSONResponse

@app.exception_handler(ValueError)
async def value_error_handler(request: Request, exc: ValueError):
    return JSONResponse(
        status_code=status.HTTP_400_BAD_REQUEST,
        content={"error": "Invalid input", "detail": str(exc)}
    )
```

### Performance

```python
# Good: Use async functions for I/O operations
@app.get("/users/")
async def get_users():
    # Database queries, API calls, file operations
    users = await database.get_all_users()
    return users

# Good: Use response models to filter data
@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: int):
    # Only fields in UserResponse will be returned
    user_data = await database.get_user(user_id)
    return user_data  # Extra fields automatically filtered
```

---

## 🎯 Key Takeaways

1. **FastAPI**: Modern, fast API framework with automatic documentation
2. **Type Hints**: Python type annotations enable automatic validation
3. **Pydantic Models**: Data validation and serialization with BaseModel
4. **REST Principles**: Resource-based URLs with proper HTTP methods
5. **Path Parameters**: Dynamic URL segments with automatic type conversion
6. **Query Parameters**: Optional URL parameters with validation
7. **Request Bodies**: JSON payloads handled automatically
8. **Response Models**: Typed responses with automatic filtering
9. **Status Codes**: Proper HTTP status codes for different operations
10. **Documentation**: Automatic interactive API docs at `/docs`

---

## 🔗 Further Reading

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)
- [REST API Design](https://restfulapi.net/)

---

## 📝 Practice Exercises

1. **Basic API**: Create a FastAPI app with GET/POST endpoints for managing a simple resource
2. **Path Parameters**: Build an API with nested path parameters (e.g., `/users/{user_id}/posts/{post_id}`)
3. **Query Parameters**: Implement search and filtering with multiple query parameters
4. **Pydantic Models**: Create complex nested models with validation for a blog API
5. **Error Handling**: Implement proper HTTP exceptions and custom error responses

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
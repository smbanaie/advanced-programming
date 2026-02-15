# FastAPI Cheatsheet - Python API Development

**Topic 7**: API Development with FastAPI
**Quick Reference**: FastAPI, Pydantic, REST APIs

---

## 🚀 FastAPI Basics

### Application Setup

```python
from fastapi import FastAPI

# Basic app
app = FastAPI()

# Configured app
app = FastAPI(
    title="My API",
    description="API description",
    version="1.0.0",
    docs_url="/docs",      # Swagger UI
    redoc_url="/redoc"     # ReDoc
)

# Run app
# uvicorn main:app --reload
```

### Basic Endpoints

```python
@app.get("/")
async def root():
    return {"message": "Hello World"}

@app.post("/items/")
async def create_item(item: dict):
    return {"item": item, "status": "created"}

@app.put("/items/{item_id}")
async def update_item(item_id: int, item: dict):
    return {"item_id": item_id, "item": item}

@app.delete("/items/{item_id}")
async def delete_item(item_id: int):
    return {"message": f"Item {item_id} deleted"}
```

---

## 🛤️ Path Parameters

### Basic Path Parameters

```python
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}

@app.get("/posts/{post_id}")
async def get_post(post_id: str):
    return {"post_id": post_id, "content": "Post content"}

# Multiple parameters
@app.get("/users/{user_id}/posts/{post_id}")
async def get_user_post(user_id: int, post_id: int):
    return {"user_id": user_id, "post_id": post_id}
```

### Path Parameter Validation

```python
from fastapi import Path

@app.get("/items/{item_id}")
async def get_item(
    item_id: int = Path(..., title="Item ID", gt=0, le=1000)
):
    return {"item_id": item_id}

# With description
@app.get("/files/{file_path:path}")
async def get_file(
    file_path: str = Path(..., description="File path to retrieve")
):
    return {"file_path": file_path}
```

---

## ❓ Query Parameters

### Basic Query Parameters

```python
@app.get("/items/")
async def list_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

# URL: /items/?skip=20&limit=50
```

### Query Parameter Validation

```python
from fastapi import Query
from typing import Optional

@app.get("/search/")
async def search_items(
    q: Optional[str] = Query(None, min_length=1, max_length=50),
    category: str = Query("all", regex="^(all|electronics|books|clothing)$"),
    price_min: float = Query(0.0, ge=0.0),
    price_max: Optional[float] = Query(None, gt=0.0),
    available: bool = Query(True)
):
    return {
        "query": q,
        "category": category,
        "price_range": {"min": price_min, "max": price_max},
        "available": available
    }
```

---

## 📦 Pydantic Models

### Basic Models

```python
from pydantic import BaseModel, Field
from typing import Optional, List
from datetime import datetime

class User(BaseModel):
    id: Optional[int] = None
    name: str = Field(..., min_length=1, max_length=100)
    email: str = Field(..., regex=r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$')
    age: Optional[int] = Field(None, ge=0, le=150)
    created_at: Optional[datetime] = None

class Address(BaseModel):
    street: str
    city: str
    country: str = "USA"
    postal_code: Optional[str] = None
```

### Model Usage

```python
# Request body
@app.post("/users/")
async def create_user(user: User):
    # user is automatically validated
    return {"user": user, "status": "created"}

# Response model
@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    # Response is validated against User model
    return User(id=user_id, name="John", email="john@example.com")

# Multiple models
class UserCreate(BaseModel):
    name: str
    email: str

class UserResponse(BaseModel):
    id: int
    name: str
    email: str
    created_at: datetime

@app.post("/users/", response_model=UserResponse)
async def create_user(user: UserCreate):
    # Different models for input/output
    pass
```

### Advanced Validation

```python
from pydantic import validator

class Product(BaseModel):
    name: str
    price: float = Field(gt=0)
    category: str
    tags: List[str] = []

    @validator('price')
    def price_must_be_reasonable(cls, v):
        if v > 10000:
            raise ValueError('Price must be less than $10,000')
        return v

    @validator('category')
    def category_must_be_valid(cls, v):
        valid_categories = ['electronics', 'books', 'clothing', 'home']
        if v.lower() not in valid_categories:
            raise ValueError(f'Category must be one of: {valid_categories}')
        return v.lower()
```

---

## 📤 Request Bodies

### Simple Request Body

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: Optional[bool] = None

@app.post("/items/")
async def create_item(item: Item):
    return {"item": item, "message": f"Created {item.name}"}

# POST /items/
# {
#   "name": "laptop",
#   "price": 999.99,
#   "is_offer": true
# }
```

### Complex Request Bodies

```python
class Order(BaseModel):
    customer_id: int
    items: List[dict]
    shipping_address: dict
    payment_method: str

@app.post("/orders/")
async def create_order(order: Order):
    return {"order_id": 123, "order": order}
```

### Multiple Body Parameters

```python
from fastapi import Body

@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    item: Item,
    admin_override: bool = Body(False, embed=True),
    notes: str = Body("", max_length=500)
):
    return {
        "item_id": item_id,
        "item": item,
        "admin_override": admin_override,
        "notes": notes
    }
```

---

## 📊 Response Models & Status Codes

### Response Models

```python
from typing import List

class ItemResponse(BaseModel):
    id: int
    name: str
    price: float
    category: str

class ItemList(BaseModel):
    items: List[ItemResponse]
    total: int
    page: int

@app.get("/items/", response_model=ItemList)
async def list_items(page: int = 1):
    # Response automatically validated
    return {
        "items": [
            {"id": 1, "name": "Laptop", "price": 999.99, "category": "electronics"}
        ],
        "total": 1,
        "page": page
    }
```

### Status Codes

```python
from fastapi import status

@app.post("/users/", status_code=status.HTTP_201_CREATED)
async def create_user(user: User):
    return {"user": user}

@app.delete("/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_user(user_id: int):
    return  # No response body for 204

# Common status codes
201: Created
204: No Content
400: Bad Request
401: Unauthorized
403: Forbidden
404: Not Found
409: Conflict
422: Unprocessable Entity
500: Internal Server Error
```

---

## ⚠️ Error Handling

### HTTP Exceptions

```python
from fastapi import HTTPException

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(
            status_code=404,
            detail=f"User {user_id} not found"
        )
    return users_db[user_id]

@app.post("/users/")
async def create_user(user: User):
    # Check for duplicate email
    for existing_user in users_db.values():
        if existing_user.email == user.email:
            raise HTTPException(
                status_code=409,
                detail="User with this email already exists"
            )
    return {"user": user}
```

### Custom Exception Handlers

```python
from fastapi import Request, HTTPException
from fastapi.responses import JSONResponse

class CustomException(Exception):
    def __init__(self, message: str, status_code: int = 400):
        self.message = message
        self.status_code = status_code

@app.exception_handler(CustomException)
async def custom_exception_handler(request: Request, exc: CustomException):
    return JSONResponse(
        status_code=exc.status_code,
        content={"error": exc.message}
    )

@app.exception_handler(ValueError)
async def value_error_handler(request: Request, exc: ValueError):
    return JSONResponse(
        status_code=400,
        content={"error": "Invalid input", "details": str(exc)}
    )
```

---

## 📚 API Documentation

### Enhanced Documentation

```python
from fastapi import Query, Path, Body

@app.get(
    "/users/",
    summary="List Users",
    description="Retrieve a paginated list of users with optional filtering",
    response_description="Paginated list of users",
    tags=["Users"]
)
async def list_users(
    skip: int = Query(0, description="Number of users to skip", ge=0),
    limit: int = Query(10, description="Maximum users to return", ge=1, le=100),
    name_filter: str = Query(None, description="Filter by name (partial match)")
):
    """Get users with pagination and filtering."""
    pass

@app.post(
    "/users/",
    summary="Create User",
    description="Create a new user account with validation",
    response_description="The newly created user",
    tags=["Users"]
)
async def create_user(
    user: User = Body(..., example={
        "name": "John Doe",
        "email": "john@example.com",
        "age": 30
    })
):
    """Create a new user."""
    pass
```

### Model Examples

```python
class User(BaseModel):
    name: str = Field(..., example="John Doe")
    email: str = Field(..., example="john@example.com")
    age: int = Field(..., example=30)

    class Config:
        schema_extra = {
            "example": {
                "name": "John Doe",
                "email": "john@example.com",
                "age": 30
            }
        }
```

---

## 🔧 Advanced Features

### Dependency Injection

```python
from fastapi import Depends

def get_database():
    # Simulate database connection
    return {"users": {}, "connection": "active"}

def get_current_user(user_id: int = Query(...)):
    # Simulate user authentication
    return {"id": user_id, "name": "John Doe"}

@app.get("/users/me")
async def get_current_user_info(
    current_user: dict = Depends(get_current_user),
    db: dict = Depends(get_database)
):
    return {
        "user": current_user,
        "db_status": db["connection"]
    }
```

### Middleware

```python
from fastapi import Request
import time

@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    response.headers["X-Process-Time"] = str(process_time)
    return response

@app.middleware("http")
async def log_requests(request: Request, call_next):
    print(f"Request: {request.method} {request.url}")
    response = await call_next(request)
    print(f"Response: {response.status_code}")
    return response
```

### File Uploads

```python
from fastapi import UploadFile, File
import shutil

@app.post("/upload/")
async def upload_file(file: UploadFile = File(...)):
    # Save uploaded file
    with open(f"uploads/{file.filename}", "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)

    return {"filename": file.filename, "status": "uploaded"}

@app.post("/upload-multiple/")
async def upload_multiple_files(files: List[UploadFile] = File(...)):
    filenames = []
    for file in files:
        with open(f"uploads/{file.filename}", "wb") as buffer:
            shutil.copyfileobj(file.file, buffer)
        filenames.append(file.filename)

    return {"uploaded_files": filenames}
```

---

## 🧪 Testing FastAPI

### Test Client

```python
from fastapi.testclient import TestClient

client = TestClient(app)

def test_create_user():
    response = client.post(
        "/users/",
        json={"name": "Alice", "email": "alice@example.com"}
    )
    assert response.status_code == 201
    data = response.json()
    assert data["user"]["name"] == "Alice"

def test_get_user():
    # Create user first
    response = client.post(
        "/users/",
        json={"name": "Bob", "email": "bob@example.com"}
    )
    user_id = response.json()["user"]["id"]

    # Get user
    response = client.get(f"/users/{user_id}")
    assert response.status_code == 200
    assert response.json()["name"] == "Bob"

def test_get_nonexistent_user():
    response = client.get("/users/999")
    assert response.status_code == 404
```

### Async Testing

```python
import pytest
from httpx import AsyncClient

@pytest.mark.asyncio
async def test_create_user_async():
    async with AsyncClient(app=app, base_url="http://testserver") as client:
        response = await client.post(
            "/users/",
            json={"name": "Charlie", "email": "charlie@example.com"}
        )
        assert response.status_code == 201
```

---

## 🎯 REST API Best Practices

### Resource Naming

```python
# Good RESTful URLs
GET    /users          # Get all users
GET    /users/123      # Get user 123
POST   /users          # Create user
PUT    /users/123      # Update user 123
DELETE /users/123      # Delete user 123

GET    /users/123/posts    # Get user's posts
POST   /users/123/posts    # Create post for user
```

### HTTP Methods

```python
# GET - Safe, idempotent
@app.get("/resources/")        # List resources
@app.get("/resources/{id}")    # Get single resource

# POST - Create resources
@app.post("/resources/")       # Create new resource

# PUT - Update entire resource (idempotent)
@app.put("/resources/{id}")    # Update/replace resource

# PATCH - Partial update
@app.patch("/resources/{id}")  # Partial resource update

# DELETE - Remove resource (idempotent)
@app.delete("/resources/{id}") # Delete resource
```

### Error Responses

```python
# Consistent error format
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "field": "email",
      "issue": "Invalid email format"
    }
  }
}

# Or simpler format
{
  "error": "User not found",
  "code": 404
}
```

---

## 📋 Common Patterns

### CRUD Operations

```python
# Complete CRUD for a resource
@app.post("/{resource}/", status_code=201)
async def create_resource(resource: ResourceCreate):
    # Create and return resource
    pass

@app.get("/{resource}/")
async def list_resources(page: int = 1, limit: int = 10):
    # Return paginated list
    pass

@app.get("/{resource}/{id}")
async def get_resource(id: int):
    # Return single resource or 404
    pass

@app.put("/{resource}/{id}")
async def update_resource(id: int, resource: ResourceUpdate):
    # Update and return resource or 404
    pass

@app.delete("/{resource}/{id}", status_code=204)
async def delete_resource(id: int):
    # Delete resource or 404
    pass
```

### Pagination

```python
from typing import Dict, Any

def paginate_items(items: list, page: int, per_page: int) -> Dict[str, Any]:
    """Paginate a list of items."""
    total = len(items)
    start = (page - 1) * per_page
    end = start + per_page

    return {
        "items": items[start:end],
        "total": total,
        "page": page,
        "per_page": per_page,
        "has_next": end < total,
        "has_prev": page > 1
    }

@app.get("/items/")
async def list_items(page: int = 1, per_page: int = 10):
    items = get_all_items()  # Your data source
    return paginate_items(items, page, per_page)
```

### Filtering and Search

```python
@app.get("/search/")
async def search_items(
    q: Optional[str] = None,
    category: Optional[str] = None,
    min_price: Optional[float] = None,
    max_price: Optional[float] = None,
    sort_by: str = "name",
    sort_order: str = "asc"
):
    """Search and filter items."""
    items = get_all_items()

    # Apply filters
    if q:
        items = [i for i in items if q.lower() in i["name"].lower()]
    if category:
        items = [i for i in items if i["category"] == category]
    if min_price is not None:
        items = [i for i in items if i["price"] >= min_price]
    if max_price is not None:
        items = [i for i in items if i["price"] <= max_price]

    # Apply sorting
    reverse = sort_order == "desc"
    if sort_by == "price":
        items.sort(key=lambda x: x["price"], reverse=reverse)
    else:  # sort by name
        items.sort(key=lambda x: x["name"], reverse=reverse)

    return {"items": items, "count": len(items)}
```

---

## 🎯 Key Takeaways

1. **FastAPI**: Modern, fast API framework with automatic docs
2. **Path Parameters**: Dynamic URL segments with automatic validation
3. **Query Parameters**: Optional parameters with defaults and constraints
4. **Pydantic Models**: Data validation and serialization with BaseModel
5. **Request Bodies**: JSON payloads handled automatically with validation
6. **Response Models**: Typed responses with automatic filtering
7. **HTTP Status Codes**: Proper status codes for different operations
8. **Error Handling**: HTTPException for consistent error responses
9. **API Documentation**: Automatic interactive docs at `/docs`
10. **REST Principles**: Proper HTTP methods and resource design

---

**Cheatsheet Version**: 1.0
**Last Updated**: February 2026
**Pages**: 1
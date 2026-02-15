# Workshop 1: Building First API

**Section**: 1 - FastAPI Fundamentals (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 1 (FastAPI Fundamentals)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Create FastAPI Applications**: Set up complete API projects
2. **Implement REST Endpoints**: Build GET, POST, PUT, DELETE operations
3. **Handle Parameters**: Work with path, query, and body parameters
4. **Use Pydantic Models**: Define data models with validation
5. **Generate Documentation**: Access and understand automatic API docs
6. **Test API Endpoints**: Use HTTP clients to test your API
7. **Run Development Servers**: Start and manage FastAPI applications

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Basic FastAPI Setup](#exercise-1-basic-fastapi-setup)
3. [Exercise 2: Path and Query Parameters](#exercise-2-path-and-query-parameters)
4. [Exercise 3: Pydantic Models and Request Bodies](#exercise-3-pydantic-models-and-request-bodies)
5. [Exercise 4: Complete CRUD API](#exercise-4-complete-crud-api)
6. [Exercise 5: API Documentation and Testing](#exercise-5-api-documentation-and-testing)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-fastapi-basics
cd workshop-fastapi-basics

# Create Python files
touch main.py models.py api.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate

# Install FastAPI and Uvicorn
pip install fastapi uvicorn

# Optional: Install HTTP client
pip install httpx  # For testing APIs
```

### Project Structure

```
workshop-fastapi-basics/
├── main.py      # FastAPI application entry point
├── models.py    # Pydantic data models
├── api.py       # API endpoint definitions
└── README.md    # Documentation (optional)
```

### Starting FastAPI Application

```python
# main.py
from fastapi import FastAPI

app = FastAPI(
    title="Workshop API",
    description="FastAPI workshop exercises",
    version="1.0.0"
)

@app.get("/")
async def root():
    return {"message": "FastAPI Workshop API"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000, reload=True)
```

**Run the application:**
```bash
python main.py
```

**Test in browser:** `http://127.0.0.1:8000`

---

## 🏗️ Exercise 1: Basic FastAPI Setup

**Goal**: Create a basic FastAPI application with simple endpoints

### Task 1.1: Hello World API

Create `main.py` with basic endpoints:

```python
from fastapi import FastAPI

app = FastAPI(
    title="My First API",
    description="A simple FastAPI application",
    version="1.0.0"
)

# TODO: Add these endpoints:
# GET / - Return welcome message
# GET /health - Return API health status
# GET /info - Return API information

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000, reload=True)
```

**Test your API:**
```bash
# Start server in one terminal
python main.py

# Test endpoints in another terminal
curl http://127.0.0.1:8000/
curl http://127.0.0.1:8000/health
curl http://127.0.0.1:8000/info
```

### Task 1.2: Basic Data Endpoints

Add endpoints that return different types of data:

```python
# TODO: Add these endpoints to main.py:
# GET /numbers - Return list of numbers
# GET /text - Return text message
# GET /boolean - Return boolean value
# GET /null - Return None/null value
# GET /object - Return dictionary object
```

**Expected responses:**
```json
GET /numbers  → [1, 2, 3, 4, 5]
GET /text     → "Hello, FastAPI!"
GET /boolean  → true
GET /null     → null
GET /object   → {"key": "value", "number": 42}
```

### Task 1.3: HTTP Methods

Implement different HTTP methods:

```python
# TODO: Add these endpoints:
# GET /echo/{message} - Echo back the message
# POST /uppercase - Convert text to uppercase
# PUT /reverse - Reverse a string
# DELETE /clear - Simulate clearing data
```

**Test with curl:**
```bash
# GET request
curl "http://127.0.0.1:8000/echo/hello"

# POST request with JSON
curl -X POST "http://127.0.0.1:8000/uppercase" \
     -H "Content-Type: application/json" \
     -d '{"text": "hello world"}'

# PUT request
curl -X PUT "http://127.0.0.1:8000/reverse" \
     -H "Content-Type: application/json" \
     -d '{"text": "FastAPI"}'
```

---

## 🛤️ Exercise 2: Path and Query Parameters

**Goal**: Implement endpoints with path and query parameters

### Task 2.1: Path Parameters

Create endpoints with path parameters:

```python
# TODO: Add these endpoints:
# GET /users/{user_id} - Get user by ID (integer)
# GET /posts/{post_id} - Get post by ID (string)
# GET /categories/{category}/items/{item_id} - Nested parameters
# GET /api/v{version}/status - API versioning
```

**Test path parameters:**
```bash
curl http://127.0.0.1:8000/users/123
curl http://127.0.0.1:8000/posts/abc123
curl http://127.0.0.1:8000/categories/electronics/items/laptop-001
curl http://127.0.0.1:8000/api/v2/status
```

### Task 2.2: Query Parameters

Implement endpoints with query parameters:

```python
# TODO: Add these endpoints:
# GET /search - Search with query parameter
# GET /filter - Multiple filter parameters
# GET /paginate - Pagination parameters
# GET /sort - Sorting parameters
```

**Implementation example:**
```python
@app.get("/search")
async def search(q: str = "", limit: int = 10):
    # TODO: Implement search logic
    pass

@app.get("/filter")
async def filter_items(
    category: str = "all",
    min_price: float = 0.0,
    max_price: float = float('inf'),
    in_stock: bool = True
):
    # TODO: Implement filtering logic
    pass
```

**Test query parameters:**
```bash
curl "http://127.0.0.1:8000/search?q=laptop&limit=5"
curl "http://127.0.0.1:8000/filter?category=electronics&min_price=500&max_price=1500&in_stock=true"
curl "http://127.0.0.1:8000/paginate?page=2&per_page=20"
curl "http://127.0.0.1:8000/sort?field=price&order=desc"
```

### Task 2.3: Parameter Validation

Add validation to your parameters:

```python
from fastapi import Query, Path
from typing import Optional

# TODO: Add validation to existing endpoints:
# - Path parameters: user_id > 0, post_id length > 0
# - Query parameters: limit between 1-100, price ranges valid
# - Optional parameters with defaults

@app.get("/users/{user_id}")
async def get_user(
    user_id: int = Path(..., gt=0, description="User ID must be positive")
):
    # TODO: Implement user retrieval
    pass

@app.get("/search")
async def search(
    q: str = Query(..., min_length=1, max_length=100),
    limit: int = Query(10, ge=1, le=100),
    offset: int = Query(0, ge=0)
):
    # TODO: Implement validated search
    pass
```

---

## 📦 Exercise 3: Pydantic Models and Request Bodies

**Goal**: Define data models and handle request bodies

### Task 3.1: Basic Models

Create `models.py` with Pydantic models:

```python
from pydantic import BaseModel, Field
from typing import Optional, List
from datetime import datetime

# TODO: Define these models:
# - User: id, name, email, age, created_at
# - Product: id, name, price, category, in_stock
# - Order: id, user_id, product_ids, total_amount, status
# - Address: street, city, country, postal_code

class User(BaseModel):
    # TODO: Define User model with validation
    pass

class Product(BaseModel):
    # TODO: Define Product model with validation
    pass

class Order(BaseModel):
    # TODO: Define Order model with validation
    pass

class Address(BaseModel):
    # TODO: Define Address model with validation
    pass
```

### Task 3.2: Request Body Endpoints

Create endpoints that accept request bodies:

```python
from models import User, Product, Order, Address
from fastapi import HTTPException

# TODO: Create these endpoints:
# POST /users/ - Create user
# POST /products/ - Create product
# POST /orders/ - Create order
# PUT /users/{user_id} - Update user
# PUT /products/{product_id} - Update product

@app.post("/users/", response_model=User)
async def create_user(user: User):
    # TODO: Simulate user creation
    # TODO: Return created user with generated ID
    pass

@app.post("/products/", response_model=Product)
async def create_product(product: Product):
    # TODO: Simulate product creation
    # TODO: Validate product data
    pass

@app.put("/users/{user_id}", response_model=User)
async def update_user(user_id: int, user: User):
    # TODO: Simulate user update
    # TODO: Check if user exists
    pass
```

**Test request bodies:**
```bash
# Create user
curl -X POST "http://127.0.0.1:8000/users/" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "John Doe",
       "email": "john@example.com",
       "age": 30
     }'

# Create product
curl -X POST "http://127.0.0.1:8000/products/" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Laptop",
       "price": 999.99,
       "category": "Electronics",
       "in_stock": true
     }'
```

### Task 3.3: Complex Request Bodies

Handle nested and complex data structures:

```python
# TODO: Create endpoints for:
# POST /users/{user_id}/address - Add address to user
# POST /orders/ - Create order with multiple products
# PUT /users/{user_id}/profile - Update user profile

@app.post("/users/{user_id}/address")
async def add_user_address(user_id: int, address: Address):
    # TODO: Add address to user
    # TODO: Validate user exists
    pass

@app.post("/orders/", response_model=Order)
async def create_order(order_data: dict):
    # TODO: Create order with validation
    # TODO: Calculate total amount
    # TODO: Set initial status
    pass
```

---

## 🔄 Exercise 4: Complete CRUD API

**Goal**: Build a complete CRUD API for a resource

### Task 4.1: In-Memory Storage

Create a simple in-memory data store:

```python
# TODO: Create in-memory storage
# - users_db: dict[int, User]
# - products_db: dict[int, Product]
# - next_id counters

users_db = {}
products_db = {}
orders_db = {}

user_id_counter = 1
product_id_counter = 1
order_id_counter = 1
```

### Task 4.2: Complete CRUD Operations

Implement full CRUD operations for users:

```python
# TODO: Implement these endpoints:
# POST /users/ - Create user
# GET /users/ - List all users (with pagination)
# GET /users/{user_id} - Get specific user
# PUT /users/{user_id} - Update user
# DELETE /users/{user_id} - Delete user

@app.post("/users/", response_model=User, status_code=201)
async def create_user(user: User):
    # TODO: Assign ID and store user
    # TODO: Return created user
    pass

@app.get("/users/", response_model=List[User])
async def list_users(skip: int = 0, limit: int = 10):
    # TODO: Return paginated list of users
    pass

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    # TODO: Find and return user or 404
    pass

@app.put("/users/{user_id}", response_model=User)
async def update_user(user_id: int, user: User):
    # TODO: Update existing user or 404
    pass

@app.delete("/users/{user_id}", status_code=204)
async def delete_user(user_id: int):
    # TODO: Delete user or 404
    pass
```

### Task 4.3: Error Handling

Add proper error handling:

```python
from fastapi import HTTPException, status

# TODO: Add error handling to all CRUD endpoints:
# - 404 for not found resources
# - 400 for validation errors
# - 409 for conflicts (duplicate creation)
# - 500 for server errors

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    if user_id not in users_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"User with ID {user_id} not found"
        )
    return users_db[user_id]

@app.post("/users/", response_model=User, status_code=201)
async def create_user(user: User):
    # Check for duplicate email
    for existing_user in users_db.values():
        if existing_user.email == user.email:
            raise HTTPException(
                status_code=status.HTTP_409_CONFLICT,
                detail="User with this email already exists"
            )

    # TODO: Create user with new ID
    pass
```

### Task 4.4: Products CRUD

Implement CRUD operations for products:

```python
# TODO: Implement product CRUD endpoints:
# POST /products/ - Create product
# GET /products/ - List products (with filtering)
# GET /products/{product_id} - Get product
# PUT /products/{product_id} - Update product
# DELETE /products/{product_id} - Delete product

@app.get("/products/")
async def list_products(
    category: Optional[str] = None,
    min_price: float = 0,
    max_price: Optional[float] = None,
    in_stock: Optional[bool] = None
):
    # TODO: Filter products by criteria
    pass
```

---

## 📚 Exercise 5: API Documentation and Testing

**Goal**: Explore automatic documentation and test the API

### Task 5.1: Enhance Documentation

Improve API documentation:

```python
# TODO: Enhance existing endpoints with:
# - Detailed descriptions
# - Parameter descriptions
# - Response examples
# - Tags for organization

@app.get(
    "/users/",
    summary="List Users",
    description="Retrieve a paginated list of all users",
    tags=["Users"]
)
async def list_users(
    skip: int = Query(0, description="Number of users to skip"),
    limit: int = Query(10, description="Maximum number of users to return", le=100)
):
    # TODO: Enhanced endpoint with documentation
    pass

@app.post(
    "/users/",
    summary="Create User",
    description="Create a new user account",
    response_description="The newly created user",
    tags=["Users"]
)
async def create_user(user: User):
    # TODO: Enhanced endpoint
    pass
```

### Task 5.2: Test with HTTP Client

Test your API using HTTP clients:

```bash
# Using curl
curl -X POST "http://127.0.0.1:8000/users/" \
     -H "Content-Type: application/json" \
     -d '{"name": "Alice", "email": "alice@example.com", "age": 25}'

curl http://127.0.0.1:8000/users/

curl http://127.0.0.1:8000/users/1

# Using HTTPie (if installed)
http POST http://127.0.0.1:8000/users/ name="Bob" email="bob@example.com" age:=30
http GET http://127.0.0.1:8000/users/
```

### Task 5.3: Interactive Documentation

Explore the automatic documentation:

```bash
# Open these URLs in your browser:
# Swagger UI: http://127.0.0.1:8000/docs
# ReDoc: http://127.0.0.1:8000/redoc
# OpenAPI Schema: http://127.0.0.1:8000/openapi.json

# TODO: Test your API endpoints through the interactive documentation
# TODO: Verify all endpoints work correctly
# TODO: Check request/response schemas are correct
```

### Task 5.4: API Validation

Create a comprehensive test of your API:

```python
# test_api.py
import requests
import json

BASE_URL = "http://127.0.0.1:8000"

def test_api_endpoints():
    """Test all API endpoints comprehensively."""

    # TODO: Test user CRUD operations
    # - Create user
    # - Get user
    # - Update user
    # - Delete user
    # - List users

    # TODO: Test product operations
    # - Create product
    # - Filter products
    # - Update/delete products

    # TODO: Test error conditions
    # - 404 for non-existent resources
    # - 400 for validation errors
    # - 409 for conflicts

    # TODO: Test parameter validation
    # - Invalid path parameters
    # - Invalid query parameters
    # - Invalid request bodies

    print("✅ All API tests passed!")

if __name__ == "__main__":
    test_api_endpoints()
```

---

## 🏆 Challenge Exercises

### Challenge 1: API Versioning

Implement API versioning:

```python
# TODO: Create versioned API endpoints:
# - /api/v1/users/
# - /api/v2/users/ (with additional fields)
# - Header-based versioning
# - Backward compatibility

from fastapi import Header, HTTPException

@app.get("/api/v1/users/")
async def get_users_v1():
    # TODO: V1 API response
    pass

@app.get("/api/v2/users/")
async def get_users_v2():
    # TODO: V2 API response with additional data
    pass

@app.get("/api/users/")
async def get_users(api_version: str = Header("v1")):
    # TODO: Header-based versioning
    pass
```

### Challenge 2: File Upload

Add file upload functionality:

```python
from fastapi import UploadFile, File

# TODO: Create file upload endpoints:
# - POST /upload/avatar - Upload user avatar
# - POST /upload/documents - Upload multiple documents
# - GET /files/{file_id} - Download file

@app.post("/upload/avatar")
async def upload_avatar(user_id: int, file: UploadFile = File(...)):
    # TODO: Handle avatar upload
    # TODO: Validate file type and size
    pass

@app.post("/upload/documents")
async def upload_documents(files: List[UploadFile] = File(...)):
    # TODO: Handle multiple file upload
    pass
```

### Challenge 3: Rate Limiting

Implement basic rate limiting:

```python
from fastapi import Request, HTTPException
from collections import defaultdict
import time

# TODO: Implement rate limiting:
# - Track requests per IP
# - Limit requests per minute/hour
# - Return 429 Too Many Requests

request_counts = defaultdict(list)

@app.middleware("http")
async def rate_limit_middleware(request: Request, call_next):
    # TODO: Implement rate limiting logic
    pass
```

---

## ✅ Solution Code

### Complete FastAPI Application

```python
from fastapi import FastAPI, HTTPException, Query, Path, status
from pydantic import BaseModel, Field
from typing import Optional, List
from datetime import datetime

app = FastAPI(
    title="Workshop API",
    description="Complete FastAPI workshop implementation",
    version="1.0.0"
)

# In-memory storage
users_db = {}
products_db = {}
user_id_counter = 1
product_id_counter = 1

# Pydantic Models
class User(BaseModel):
    id: Optional[int] = None
    name: str = Field(..., min_length=1, max_length=100)
    email: str = Field(..., regex=r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$')
    age: Optional[int] = Field(None, ge=0, le=150)
    created_at: Optional[datetime] = None

class Product(BaseModel):
    id: Optional[int] = None
    name: str = Field(..., min_length=1, max_length=200)
    price: float = Field(..., gt=0)
    category: str = Field(..., min_length=1)
    in_stock: bool = True

# CRUD Endpoints for Users
@app.post("/users/", response_model=User, status_code=status.HTTP_201_CREATED)
async def create_user(user: User):
    """Create a new user."""
    global user_id_counter

    # Check for duplicate email
    for existing_user in users_db.values():
        if existing_user.email == user.email:
            raise HTTPException(
                status_code=status.HTTP_409_CONFLICT,
                detail="User with this email already exists"
            )

    # Create user
    user.id = user_id_counter
    user.created_at = datetime.now()
    users_db[user_id_counter] = user
    user_id_counter += 1

    return user

@app.get("/users/", response_model=List[User])
async def list_users(
    skip: int = Query(0, ge=0),
    limit: int = Query(10, ge=1, le=100)
):
    """List all users with pagination."""
    users = list(users_db.values())[skip:skip + limit]
    return users

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int = Path(..., gt=0)):
    """Get a specific user by ID."""
    if user_id not in users_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"User with ID {user_id} not found"
        )
    return users_db[user_id]

@app.put("/users/{user_id}", response_model=User)
async def update_user(user_id: int = Path(..., gt=0), user: User):
    """Update an existing user."""
    if user_id not in users_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"User with ID {user_id} not found"
        )

    # Update user (keep original ID and created_at)
    existing_user = users_db[user_id]
    updated_user = user.copy()
    updated_user.id = user_id
    updated_user.created_at = existing_user.created_at
    users_db[user_id] = updated_user

    return updated_user

@app.delete("/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_user(user_id: int = Path(..., gt=0)):
    """Delete a user."""
    if user_id not in users_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"User with ID {user_id} not found"
        )
    del users_db[user_id]

# CRUD Endpoints for Products
@app.post("/products/", response_model=Product, status_code=status.HTTP_201_CREATED)
async def create_product(product: Product):
    """Create a new product."""
    global product_id_counter

    product.id = product_id_counter
    products_db[product_id_counter] = product
    product_id_counter += 1

    return product

@app.get("/products/", response_model=List[Product])
async def list_products(
    category: Optional[str] = None,
    min_price: float = Query(0, ge=0),
    max_price: Optional[float] = Query(None, ge=0),
    in_stock: Optional[bool] = None
):
    """List products with filtering."""
    products = list(products_db.values())

    # Apply filters
    if category:
        products = [p for p in products if p.category.lower() == category.lower()]

    if max_price is not None:
        products = [p for p in products if min_price <= p.price <= max_price]
    else:
        products = [p for p in products if p.price >= min_price]

    if in_stock is not None:
        products = [p for p in products if p.in_stock == in_stock]

    return products

@app.get("/products/{product_id}", response_model=Product)
async def get_product(product_id: int = Path(..., gt=0)):
    """Get a specific product by ID."""
    if product_id not in products_db:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Product with ID {product_id} not found"
        )
    return products_db[product_id]

# Health check endpoint
@app.get("/health")
async def health_check():
    """API health check endpoint."""
    return {
        "status": "healthy",
        "timestamp": datetime.now(),
        "users_count": len(users_db),
        "products_count": len(products_db)
    }

# Root endpoint
@app.get("/")
async def root():
    """API root endpoint with basic information."""
    return {
        "message": "FastAPI Workshop API",
        "version": "1.0.0",
        "endpoints": [
            "GET /",
            "GET /health",
            "POST /users/",
            "GET /users/",
            "GET /users/{user_id}",
            "PUT /users/{user_id}",
            "DELETE /users/{user_id}",
            "POST /products/",
            "GET /products/",
            "GET /products/{product_id}"
        ],
        "docs": {
            "swagger": "/docs",
            "redoc": "/redoc",
            "openapi": "/openapi.json"
        }
    }

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000, reload=True)
```

---

## 📝 Key Takeaways

1. **FastAPI Setup**: Create applications with proper configuration and metadata
2. **Path Parameters**: Use type-hinted parameters in endpoint paths with validation
3. **Query Parameters**: Add optional parameters with defaults and constraints
4. **Pydantic Models**: Define data structures with automatic validation
5. **Request Bodies**: Accept JSON data through model parameters
6. **Response Models**: Control response format and documentation
7. **HTTP Status Codes**: Use appropriate status codes for different operations
8. **CRUD Operations**: Implement complete Create, Read, Update, Delete functionality
9. **Error Handling**: Use HTTPException for proper error responses
10. **API Documentation**: Automatic interactive docs at `/docs` and `/redoc`

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
# Tutorial 2: JSON Handling & API Integration

**Section**: 2 - JSON Handling & API Integration (90 min)
**Level**: Intermediate
**Prerequisites**: Section 1 (Lists and Sets), basic Python knowledge

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Work with JSON Data**: Parse, create, and manipulate JSON structures
2. **Handle Dictionaries**: Perform advanced operations on nested dictionaries
3. **Make HTTP Requests**: Use the requests library to interact with web APIs
4. **Process API Responses**: Extract and transform data from real API endpoints
5. **Implement Error Handling**: Handle network errors and malformed data

---

## 📚 Table of Contents

1. [JSON Fundamentals](#json-fundamentals)
2. [Python JSON Module](#python-json-module)
3. [Working with Dictionaries](#working-with-dictionaries)
4. [HTTP Requests with Requests](#http-requests-with-requests)
5. [API Integration Examples](#api-integration-examples)
6. [Error Handling](#error-handling)
7. [Best Practices](#best-practices)

---

## 🔸 JSON Fundamentals

JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write, and easy for machines to parse and generate.

### JSON Data Types

```python
# JSON supports these data types:
# - Objects: {"key": "value"}
# - Arrays: [1, 2, 3]
# - Strings: "hello"
# - Numbers: 42, 3.14
# - Booleans: true/false
# - null: null

# Python equivalents:
# dict, list, str, int/float, bool, None
```

### JSON Structure

```json
{
  "name": "John Doe",
  "age": 30,
  "is_student": false,
  "courses": ["Python", "JavaScript", "SQL"],
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "country": "USA"
  },
  "metadata": null
}
```

**Key Characteristics:**
- **Human-readable**: Easy to understand and edit
- **Language-independent**: Works with any programming language
- **Lightweight**: Minimal overhead
- **Standardized**: RFC 8259 specification

---

## 📦 Python JSON Module

Python's built-in `json` module provides functions for encoding and decoding JSON data.

### Importing JSON Module

```python
import json
```

### Encoding (Python to JSON)

```python
# json.dumps() - Convert Python object to JSON string
data = {
    "name": "Alice",
    "age": 25,
    "courses": ["Math", "Physics"]
}

json_string = json.dumps(data)
print(json_string)
# Output: {"name": "Alice", "age": 25, "courses": ["Math", "Physics"]}

# With formatting
pretty_json = json.dumps(data, indent=2)
print(pretty_json)
# Output:
# {
#   "name": "Alice",
#   "age": 25,
#   "courses": ["Math", "Physics"]
# }
```

### Decoding (JSON to Python)

```python
# json.loads() - Convert JSON string to Python object
json_data = '{"name": "Bob", "age": 30, "active": true}'
python_data = json.loads(json_data)
print(python_data)
# Output: {'name': 'Bob', 'age': 30, 'active': True}
print(type(python_data))  # <class 'dict'>
```

### File Operations

```python
# Writing JSON to file
data = {"users": [{"name": "Alice"}, {"name": "Bob"}]}

with open('data.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=2, ensure_ascii=False)

# Reading JSON from file
with open('data.json', 'r', encoding='utf-8') as f:
    loaded_data = json.load(f)

print(loaded_data)
# Output: {'users': [{'name': 'Alice'}, {'name': 'Bob'}]}
```

### Handling Special Cases

```python
# Custom objects (don't work by default)
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

person = Person("Charlie", 35)

# This will fail:
# json.dumps(person)  # TypeError

# Solution: Convert to dict first
person_dict = {"name": person.name, "age": person.age}
json_str = json.dumps(person_dict)
print(json_str)  # {"name": "Charlie", "age": 35}
```

---

## 📚 Working with Dictionaries

Dictionaries are Python's mapping type and directly correspond to JSON objects.

### Basic Dictionary Operations

```python
# Creating dictionaries
user = {
    "name": "Diana",
    "email": "diana@example.com",
    "preferences": {
        "theme": "dark",
        "notifications": True
    }
}

# Accessing values
print(user["name"])        # Diana
print(user["preferences"]["theme"])  # dark

# Safe access with get()
print(user.get("phone", "Not provided"))  # Not provided
print(user.get("name", "Unknown"))        # Diana
```

### Advanced Dictionary Operations

```python
# Adding/updating values
user["phone"] = "+1-555-0123"
user["preferences"]["language"] = "en"

# Removing values
del user["email"]

# Checking existence
if "phone" in user:
    print(f"Phone: {user['phone']}")

# Iterating
for key, value in user.items():
    print(f"{key}: {value}")

# Dictionary comprehensions
squared = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### Nested Dictionary Operations

```python
# Working with nested structures
company = {
    "name": "Tech Corp",
    "departments": {
        "engineering": {
            "employees": ["Alice", "Bob", "Charlie"],
            "budget": 500000
        },
        "sales": {
            "employees": ["Diana", "Eve"],
            "budget": 300000
        }
    }
}

# Accessing nested values
eng_employees = company["departments"]["engineering"]["employees"]
print(eng_employees)  # ['Alice', 'Bob', 'Charlie']

# Modifying nested values
company["departments"]["engineering"]["budget"] += 50000

# Adding new nested structures
company["departments"]["hr"] = {
    "employees": ["Frank"],
    "budget": 150000
}
```

### Dictionary Merging and Updating

```python
# update() method
dict1 = {"a": 1, "b": 2}
dict2 = {"b": 20, "c": 30}

dict1.update(dict2)
print(dict1)  # {'a': 1, 'b': 20, 'c': 30}

# Dictionary unpacking (Python 3.5+)
dict3 = {**dict1, **{"d": 40}}
print(dict3)  # {'a': 1, 'b': 20, 'c': 30, 'd': 40}

# Merging nested dictionaries
from collections import ChainMap
nested1 = {"user": {"name": "Alice", "age": 25}}
nested2 = {"user": {"email": "alice@example.com", "city": "NYC"}}

# This doesn't merge nested dicts:
merged = {**nested1, **nested2}  # {'user': {'email': 'alice@example.com', 'city': 'NYC'}}
```

### Dictionary Utilities

```python
# collections.defaultdict
from collections import defaultdict

# Automatically creates missing keys
word_count = defaultdict(int)
words = ["apple", "banana", "apple", "cherry"]

for word in words:
    word_count[word] += 1

print(dict(word_count))  # {'apple': 2, 'banana': 1, 'cherry': 1}

# collections.Counter for counting
from collections import Counter
colors = ["red", "blue", "red", "green", "blue", "red"]
color_count = Counter(colors)
print(color_count)  # Counter({'red': 3, 'blue': 2, 'green': 1})
```

---

## 🌐 HTTP Requests with Requests

The `requests` library is the most popular HTTP library for Python.

### Installation

```bash
pip install requests
```

### Basic GET Request

```python
import requests

# Simple GET request
response = requests.get("https://jsonplaceholder.typicode.com/users/1")

print(f"Status Code: {response.status_code}")
print(f"Content Type: {response.headers['content-type']}")
print(f"Response Text: {response.text[:200]}...")

# Check if request was successful
if response.status_code == 200:
    print("Request successful!")
else:
    print(f"Request failed with status: {response.status_code}")
```

### Response Object

```python
response = requests.get("https://jsonplaceholder.typicode.com/users/1")

# Response attributes
print(f"URL: {response.url}")
print(f"Status: {response.status_code}")
print(f"Headers: {dict(response.headers)}")
print(f"Content: {response.content[:100]}...")  # Raw bytes
print(f"Text: {response.text[:100]}...")        # Decoded text
print(f"JSON: {response.json()}")               # Parsed JSON (if applicable)
```

### Request Parameters

```python
# Query parameters
params = {"key": "value", "another": "param"}
response = requests.get("https://httpbin.org/get", params=params)

print(f"URL: {response.url}")
# Output: https://httpbin.org/get?key=value&another=param
```

### Headers and Authentication

```python
# Custom headers
headers = {
    "User-Agent": "My Python App",
    "Accept": "application/json"
}

response = requests.get("https://api.example.com/data", headers=headers)

# Basic authentication
from requests.auth import HTTPBasicAuth
response = requests.get(
    "https://api.example.com/protected",
    auth=HTTPBasicAuth("username", "password")
)

# API key authentication
headers = {"Authorization": f"Bearer {api_key}"}
response = requests.get("https://api.example.com/data", headers=headers)
```

### POST Requests

```python
# POST with JSON data
data = {"name": "John", "email": "john@example.com"}
response = requests.post(
    "https://jsonplaceholder.typicode.com/users",
    json=data  # Automatically sets content-type and serializes
)

print(f"Status: {response.status_code}")
print(f"Created User: {response.json()}")

# POST with form data
form_data = {"username": "john", "password": "secret"}
response = requests.post(
    "https://httpbin.org/post",
    data=form_data
)
```

---

## 🔗 API Integration Examples

Let's work with the randomuser.me API, which provides fake user data.

### Basic API Call

```python
import requests

# Get a random user
response = requests.get("https://randomuser.me/api/")

if response.status_code == 200:
    data = response.json()
    user = data["results"][0]

    print(f"Name: {user['name']['first']} {user['name']['last']}")
    print(f"Email: {user['email']}")
    print(f"Country: {user['location']['country']}")
else:
    print(f"Error: {response.status_code}")
```

### Processing Multiple Users

```python
# Get multiple users
response = requests.get("https://randomuser.me/api/?results=5")

if response.status_code == 200:
    data = response.json()
    users = data["results"]

    for user in users:
        name = f"{user['name']['first']} {user['name']['last']}"
        email = user['email']
        country = user['location']['country']
        print(f"{name} - {email} - {country}")
```

### Filtering API Results

```python
# Filter by gender
params = {
    "results": 3,
    "gender": "female",
    "nat": "us,gb"  # US and UK nationalities
}

response = requests.get("https://randomuser.me/api/", params=params)

if response.status_code == 200:
    data = response.json()
    for user in data["results"]:
        name = f"{user['name']['first']} {user['name']['last']}"
        gender = user['gender']
        nat = user['nat'].upper()
        print(f"{name} ({gender}) - {nat}")
```

### Converting API Data to Custom Format

```python
def process_user_data(api_response):
    """Convert randomuser.me API response to custom format."""
    users = []

    for user_data in api_response["results"]:
        # Extract relevant information
        user = {
            "full_name": f"{user_data['name']['first']} {user_data['name']['last']}",
            "email": user_data["email"],
            "age": user_data["dob"]["age"],
            "country": user_data["location"]["country"],
            "city": user_data["location"]["city"],
            "phone": user_data["phone"],
            "gender": user_data["gender"],
            "profile_picture": user_data["picture"]["large"]
        }
        users.append(user)

    return users

# Usage
response = requests.get("https://randomuser.me/api/?results=2")
if response.status_code == 200:
    processed_users = process_user_data(response.json())

    for user in processed_users:
        print(f"Name: {user['full_name']}")
        print(f"Age: {user['age']}, Country: {user['country']}")
        print("---")
```

---

## ⚠️ Error Handling

Proper error handling is crucial when working with APIs.

### Network Errors

```python
import requests
from requests.exceptions import RequestException, Timeout, ConnectionError

def safe_api_call(url, timeout=10):
    """Make a safe API call with proper error handling."""
    try:
        response = requests.get(url, timeout=timeout)

        # Check HTTP status
        response.raise_for_status()  # Raises HTTPError for bad status codes

        return response.json()

    except Timeout:
        print("Request timed out")
        return None
    except ConnectionError:
        print("Connection error - check internet connection")
        return None
    except requests.HTTPError as e:
        print(f"HTTP error: {e.response.status_code}")
        return None
    except requests.RequestException as e:
        print(f"Request error: {e}")
        return None
    except ValueError as e:
        print(f"JSON parsing error: {e}")
        return None

# Usage
result = safe_api_call("https://randomuser.me/api/")
if result:
    print("Data retrieved successfully")
else:
    print("Failed to retrieve data")
```

### Handling Malformed JSON

```python
import json

def safe_json_loads(json_string):
    """Safely parse JSON with error handling."""
    try:
        return json.loads(json_string)
    except json.JSONDecodeError as e:
        print(f"Invalid JSON: {e}")
        return None
    except TypeError as e:
        print(f"Type error: {e}")
        return None

# Usage
valid_json = '{"name": "Alice", "age": 30}'
invalid_json = '{"name": "Alice", "age": }'  # Missing value

data1 = safe_json_loads(valid_json)    # Returns dict
data2 = safe_json_loads(invalid_json)  # Returns None, prints error
```

### Comprehensive Error Handling

```python
def fetch_and_process_users(count=5):
    """Fetch users from API and process with comprehensive error handling."""
    url = f"https://randomuser.me/api/?results={count}"

    # Make request with error handling
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
    except (requests.RequestException, TimeoutError) as e:
        print(f"Network error: {e}")
        return []

    # Parse JSON with error handling
    try:
        data = response.json()
    except ValueError as e:
        print(f"JSON parsing error: {e}")
        return []

    # Validate response structure
    if "results" not in data:
        print("Invalid API response: missing 'results' key")
        return []

    if not isinstance(data["results"], list):
        print("Invalid API response: 'results' is not a list")
        return []

    # Process users with error handling
    users = []
    for i, user_data in enumerate(data["results"]):
        try:
            user = {
                "name": f"{user_data['name']['first']} {user_data['name']['last']}",
                "email": user_data.get("email", "N/A"),
                "age": user_data.get("dob", {}).get("age", "Unknown")
            }
            users.append(user)
        except (KeyError, TypeError) as e:
            print(f"Error processing user {i}: {e}")
            continue

    return users

# Usage
users = fetch_and_process_users(3)
print(f"Successfully processed {len(users)} users")
for user in users:
    print(f"- {user['name']} ({user['age']} years old)")
```

---

## 🎯 Best Practices

### API Design

```python
class APIClient:
    """Reusable API client with best practices."""

    def __init__(self, base_url, timeout=10):
        self.base_url = base_url.rstrip('/')
        self.timeout = timeout
        self.session = requests.Session()  # Reuse connections

    def get(self, endpoint, params=None, **kwargs):
        """Make GET request with error handling."""
        url = f"{self.base_url}/{endpoint.lstrip('/')}"

        try:
            response = self.session.get(
                url,
                params=params,
                timeout=self.timeout,
                **kwargs
            )
            response.raise_for_status()
            return response.json()
        except requests.RequestException as e:
            raise APIError(f"API request failed: {e}") from e

    def close(self):
        """Clean up session."""
        self.session.close()

class APIError(Exception):
    """Custom exception for API errors."""
    pass

# Usage
client = APIClient("https://randomuser.me")
try:
    data = client.get("api", params={"results": 2})
    print(f"Retrieved {len(data['results'])} users")
finally:
    client.close()
```

### Data Validation

```python
from typing import Dict, List, Any, Optional

def validate_user_data(user: Dict[str, Any]) -> bool:
    """Validate user data structure."""
    required_fields = ["name", "email", "location"]

    # Check required fields exist
    for field in required_fields:
        if field not in user:
            return False

    # Validate email format (basic)
    email = user.get("email", "")
    if "@" not in email or "." not in email:
        return False

    # Validate location structure
    location = user.get("location", {})
    if not isinstance(location, dict):
        return False

    required_location = ["country", "city"]
    for field in required_location:
        if field not in location:
            return False

    return True

# Usage
response = requests.get("https://randomuser.me/api/")
if response.status_code == 200:
    data = response.json()
    for user in data["results"]:
        if validate_user_data(user):
            print(f"Valid user: {user['name']['first']}")
        else:
            print("Invalid user data")
```

### Configuration Management

```python
# config.py
API_CONFIG = {
    "base_url": "https://randomuser.me",
    "timeout": 10,
    "retry_attempts": 3,
    "retry_delay": 1
}

# api_client.py
import time
import requests
from config import API_CONFIG

def make_resilient_request(url, max_retries=None, **kwargs):
    """Make request with automatic retries."""
    max_retries = max_retries or API_CONFIG["retry_attempts"]

    for attempt in range(max_retries):
        try:
            response = requests.get(
                url,
                timeout=API_CONFIG["timeout"],
                **kwargs
            )
            response.raise_for_status()
            return response
        except requests.RequestException as e:
            if attempt == max_retries - 1:
                raise e
            time.sleep(API_CONFIG["retry_delay"])
            continue
```

---

## 🎯 Key Takeaways

1. **JSON is Python's Native Format**: Dictionaries and lists map directly to JSON objects and arrays
2. **Requests Library**: Simple, powerful HTTP client for Python
3. **Error Handling**: Always handle network errors, HTTP errors, and JSON parsing errors
4. **API Design**: Create reusable clients with proper configuration management
5. **Data Validation**: Validate API responses before processing
6. **Resource Management**: Use sessions for connection reuse and proper cleanup

---

## 🔗 Further Reading

- [JSON Official Specification](https://www.json.org/json-en.html)
- [Requests Documentation](https://requests.readthedocs.io/)
- [RandomUser.me API Documentation](https://randomuser.me/documentation)
- [HTTP Status Codes](https://httpstatuses.com/)

---

## 📝 Practice Exercises

1. **JSON File Operations**: Create a program that reads JSON from a file, modifies it, and writes it back
2. **API Data Aggregation**: Fetch data from multiple API endpoints and combine the results
3. **Error Recovery**: Implement a function that retries failed API calls with exponential backoff
4. **Data Transformation**: Convert API response data into different formats (CSV, XML)
5. **Caching**: Add caching to API calls to avoid repeated requests

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
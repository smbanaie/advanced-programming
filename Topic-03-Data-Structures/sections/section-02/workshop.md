# Workshop 2: Fetching and Processing User Data

**Section**: 2 - JSON Handling & API Integration (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 2 (JSON & API Integration)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Master API Integration**: Make HTTP requests and handle responses
2. **Process JSON Data**: Parse and manipulate real API responses
3. **Build Data Pipelines**: Create functions to transform API data
4. **Implement Error Handling**: Handle network and data processing errors
5. **Create Reusable Components**: Build modular API client functions

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Basic API Requests](#exercise-1-basic-api-requests)
3. [Exercise 2: JSON Data Processing](#exercise-2-json-data-processing)
4. [Exercise 3: Data Transformation Pipeline](#exercise-3-data-transformation-pipeline)
5. [Exercise 4: Error Handling and Validation](#exercise-4-error-handling-and-validation)
6. [Exercise 5: Advanced API Features](#exercise-5-advanced-api-features)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-api-integration
cd workshop-api-integration

# Create Python files
touch api_client.py data_processor.py test_api.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate

# Install requests
pip install requests
```

### Required Libraries

```python
import requests
import json
from typing import Dict, List, Any, Optional
from datetime import datetime
```

---

## 🏃 Exercise 1: Basic API Requests

**Goal**: Learn to make basic HTTP requests to the randomuser.me API

### Task 1.1: Single User Request

Create a function that fetches a single random user from the API.

```python
def fetch_single_user() -> Dict[str, Any]:
    """
    Fetch a single random user from randomuser.me API.

    Returns:
        Dictionary containing user data

    Expected API Response Structure:
    {
        "results": [
            {
                "name": {"first": "John", "last": "Doe"},
                "email": "john.doe@example.com",
                "location": {"country": "United States", ...},
                ...
            }
        ]
    }
    """
    # TODO: Make GET request to https://randomuser.me/api/
    # TODO: Check response status
    # TODO: Parse JSON response
    # TODO: Return the first user from results array
    pass
```

**Test your function:**
```python
# Test the function
user = fetch_single_user()
print(f"Name: {user['name']['first']} {user['name']['last']}")
print(f"Email: {user['email']}")
print(f"Country: {user['location']['country']}")
```

### Task 1.2: Multiple Users Request

Create a function that fetches multiple users with parameters.

```python
def fetch_multiple_users(count: int = 5, gender: Optional[str] = None) -> List[Dict[str, Any]]:
    """
    Fetch multiple random users with optional filtering.

    Args:
        count: Number of users to fetch (max 5000)
        gender: Filter by gender ("male", "female", or None)

    Returns:
        List of user dictionaries
    """
    # TODO: Build URL with query parameters
    # TODO: Handle gender parameter
    # TODO: Make request and parse response
    # TODO: Return list of users
    pass
```

**Test your function:**
```python
# Test with different parameters
users = fetch_multiple_users(3)
print(f"Fetched {len(users)} users")

female_users = fetch_multiple_users(2, gender="female")
print(f"Fetched {len(female_users)} female users")
```

---

## 🔄 Exercise 2: JSON Data Processing

**Goal**: Learn to extract and manipulate data from API responses

### Task 2.1: Extract User Information

Create functions to extract specific information from user data.

```python
def extract_basic_info(user: Dict[str, Any]) -> Dict[str, str]:
    """
    Extract basic information from a user dictionary.

    Args:
        user: User data from API

    Returns:
        Dictionary with basic info: name, email, country
    """
    # TODO: Extract name (first + last)
    # TODO: Extract email
    # TODO: Extract country from location
    # TODO: Return clean dictionary
    pass

def extract_contact_info(user: Dict[str, Any]) -> Dict[str, str]:
    """
    Extract contact information from user data.

    Args:
        user: User data from API

    Returns:
        Dictionary with contact info: phone, cell
    """
    # TODO: Extract phone and cell numbers
    # TODO: Handle missing fields gracefully
    pass
```

**Test your functions:**
```python
# Test with real API data
user = fetch_single_user()

basic = extract_basic_info(user)
print("Basic Info:", basic)

contact = extract_contact_info(user)
print("Contact Info:", contact)
```

### Task 2.2: Data Formatting

Create functions to format user data in different ways.

```python
def format_user_summary(user: Dict[str, Any]) -> str:
    """
    Create a formatted summary string for a user.

    Args:
        user: User data from API

    Returns:
        Formatted string summary

    Example:
        "John Doe (john.doe@email.com) from United States"
    """
    # TODO: Extract name, email, country
    # TODO: Format as readable string
    pass

def format_user_card(user: Dict[str, Any]) -> str:
    """
    Create a detailed user card format.

    Args:
        user: User data from API

    Returns:
        Multi-line formatted string

    Example:
        Name: John Doe
        Age: 30
        Location: New York, United States
        Email: john.doe@email.com
    """
    # TODO: Extract multiple fields
    # TODO: Format as multi-line card
    pass
```

**Test your functions:**
```python
user = fetch_single_user()

summary = format_user_summary(user)
print("Summary:", summary)

card = format_user_card(user)
print("Card:")
print(card)
```

---

## 🔧 Exercise 3: Data Transformation Pipeline

**Goal**: Build a complete data processing pipeline

### Task 3.1: User Data Transformer

Create a class that transforms raw API data into different formats.

```python
class UserDataTransformer:
    """Transform user data into various formats."""

    def __init__(self):
        self.transformed_count = 0

    def transform_to_csv_row(self, user: Dict[str, Any]) -> List[str]:
        """
        Transform user data to CSV row format.

        Args:
            user: Raw user data

        Returns:
            List of strings representing CSV columns
        """
        # TODO: Extract: name, age, gender, country, email
        # TODO: Return as list for CSV writing
        pass

    def transform_to_simple_dict(self, user: Dict[str, Any]) -> Dict[str, Any]:
        """
        Transform to simplified dictionary format.

        Args:
            user: Raw user data

        Returns:
            Simplified dictionary with essential fields
        """
        # TODO: Extract essential information only
        # TODO: Flatten nested structures
        pass

    def get_csv_header(self) -> List[str]:
        """Return CSV header row."""
        return ["Name", "Age", "Gender", "Country", "Email"]
```

**Test your transformer:**
```python
transformer = UserDataTransformer()

# Test transformations
user = fetch_single_user()

csv_row = transformer.transform_to_csv_row(user)
print("CSV Row:", csv_row)

simple = transformer.transform_to_simple_dict(user)
print("Simple Dict:", simple)
```

### Task 3.2: Batch Processing

Create functions to process multiple users at once.

```python
def process_user_batch(users: List[Dict[str, Any]], format_type: str = "summary") -> List[str]:
    """
    Process a batch of users into formatted output.

    Args:
        users: List of user dictionaries
        format_type: Output format ("summary", "card", "csv")

    Returns:
        List of formatted strings
    """
    # TODO: Process each user based on format_type
    # TODO: Return list of formatted results
    pass

def save_users_to_file(users: List[Dict[str, Any]], filename: str, format_type: str = "json"):
    """
    Save processed user data to file.

    Args:
        users: List of user dictionaries
        filename: Output filename
        format_type: File format ("json", "csv")
    """
    # TODO: Process users based on format
    # TODO: Save to appropriate file format
    pass
```

**Test your batch processing:**
```python
# Fetch multiple users
users = fetch_multiple_users(3)

# Test different formats
summaries = process_user_batch(users, "summary")
print("Summaries:")
for summary in summaries:
    print(f"  {summary}")

# Save to file
save_users_to_file(users, "users.json", "json")
save_users_to_file(users, "users.csv", "csv")
```

---

## ⚠️ Exercise 4: Error Handling and Validation

**Goal**: Implement robust error handling and data validation

### Task 4.1: Safe API Client

Create a robust API client with error handling.

```python
class SafeAPIClient:
    """API client with comprehensive error handling."""

    def __init__(self, base_url: str = "https://randomuser.me", timeout: int = 10):
        self.base_url = base_url.rstrip('/')
        self.timeout = timeout

    def fetch_users_safe(self, count: int = 1) -> Optional[List[Dict[str, Any]]]:
        """
        Safely fetch users with error handling.

        Args:
            count: Number of users to fetch

        Returns:
            List of users or None if error
        """
        # TODO: Implement safe request with try/catch
        # TODO: Handle network errors, HTTP errors, JSON errors
        # TODO: Return None on any error
        pass

    def validate_user_data(self, user: Dict[str, Any]) -> bool:
        """
        Validate user data structure.

        Args:
            user: User data to validate

        Returns:
            True if valid, False otherwise
        """
        # TODO: Check required fields exist
        # TODO: Validate data types
        # TODO: Check nested structures
        pass
```

**Test your safe client:**
```python
client = SafeAPIClient()

# Test normal operation
users = client.fetch_users_safe(2)
if users:
    print(f"Successfully fetched {len(users)} users")
else:
    print("Failed to fetch users")

# Test validation
user = fetch_single_user()
is_valid = client.validate_user_data(user)
print(f"User data is valid: {is_valid}")
```

### Task 4.2: Data Cleaning Functions

Create functions to clean and normalize API data.

```python
def clean_user_email(email: str) -> str:
    """
    Clean and normalize email addresses.

    Args:
        email: Raw email string

    Returns:
        Cleaned email string
    """
    # TODO: Convert to lowercase
    # TODO: Remove extra whitespace
    # TODO: Basic validation
    pass

def normalize_user_location(location: Dict[str, Any]) -> Dict[str, str]:
    """
    Normalize location data.

    Args:
        location: Raw location dictionary

    Returns:
        Normalized location dictionary
    """
    # TODO: Ensure required fields exist
    # TODO: Standardize country names
    # TODO: Format city names properly
    pass

def clean_user_data(user: Dict[str, Any]) -> Dict[str, Any]:
    """
    Clean and normalize complete user data.

    Args:
        user: Raw user dictionary

    Returns:
        Cleaned user dictionary
    """
    # TODO: Apply all cleaning functions
    # TODO: Handle missing fields
    # TODO: Return cleaned data
    pass
```

**Test your data cleaning:**
```python
# Test individual functions
raw_email = "  JOHN.DOE@EXAMPLE.COM  "
clean_email = clean_user_email(raw_email)
print(f"Cleaned email: '{clean_email}'")

# Test complete cleaning
user = fetch_single_user()
cleaned = clean_user_data(user)
print("Original vs Cleaned:")
print(f"Email: {user['email']} -> {cleaned['email']}")
```

---

## 🚀 Exercise 5: Advanced API Features

**Goal**: Implement advanced API features and caching

### Task 5.1: API Client with Caching

Create an API client that caches responses.

```python
import time
from typing import Dict, Any
import json
from pathlib import Path

class CachedAPIClient:
    """API client with response caching."""

    def __init__(self, cache_dir: str = ".cache", cache_ttl: int = 300):
        self.base_url = "https://randomuser.me"
        self.cache_dir = Path(cache_dir)
        self.cache_ttl = cache_ttl  # seconds
        self.cache_dir.mkdir(exist_ok=True)

    def _get_cache_key(self, params: Dict[str, Any]) -> str:
        """Generate cache key from parameters."""
        # TODO: Create unique key from params
        pass

    def _is_cache_valid(self, cache_file: Path) -> bool:
        """Check if cache file is still valid."""
        # TODO: Check file age vs TTL
        pass

    def _load_from_cache(self, cache_key: str) -> Optional[Dict[str, Any]]:
        """Load data from cache if available and valid."""
        # TODO: Check cache file exists and is valid
        # TODO: Load and return cached data
        pass

    def _save_to_cache(self, cache_key: str, data: Dict[str, Any]):
        """Save data to cache."""
        # TODO: Save data with timestamp
        pass

    def fetch_users_cached(self, count: int = 1) -> List[Dict[str, Any]]:
        """
        Fetch users with caching.

        Args:
            count: Number of users to fetch

        Returns:
            List of user dictionaries
        """
        # TODO: Check cache first
        # TODO: Make API call if no valid cache
        # TODO: Cache the result
        # TODO: Return users
        pass
```

**Test your cached client:**
```python
cached_client = CachedAPIClient()

# First call (should make API request)
print("First call:")
users1 = cached_client.fetch_users_cached(2)
print(f"Fetched {len(users1)} users")

# Second call (should use cache)
print("Second call (should be cached):")
users2 = cached_client.fetch_users_cached(2)
print(f"Fetched {len(users2)} users (from cache)")
```

### Task 5.2: Data Export Utilities

Create utilities to export data in different formats.

```python
def export_to_json(users: List[Dict[str, Any]], filename: str):
    """Export users to JSON file."""
    # TODO: Write users to JSON file with proper formatting
    pass

def export_to_csv(users: List[Dict[str, Any]], filename: str):
    """Export users to CSV file."""
    # TODO: Convert users to CSV format
    # TODO: Write to CSV file
    pass

def export_users(users: List[Dict[str, Any]], filename: str, format_type: str = "json"):
    """
    Export users to file in specified format.

    Args:
        users: List of user dictionaries
        filename: Output filename (without extension)
        format_type: Export format ("json", "csv")
    """
    # TODO: Route to appropriate export function
    # TODO: Add file extension automatically
    pass
```

**Test your export utilities:**
```python
# Fetch users and export
users = fetch_multiple_users(5)

# Export in different formats
export_users(users, "my_users", "json")
export_users(users, "my_users", "csv")

print("Files exported successfully")
```

---

## 🏆 Challenge Exercises

### Challenge 1: Advanced Filtering

Create a flexible filtering system for user data.

```python
def filter_users(users: List[Dict[str, Any]], criteria: Dict[str, Any]) -> List[Dict[str, Any]]:
    """
    Filter users based on complex criteria.

    Args:
        users: List of user dictionaries
        criteria: Filter criteria dictionary

    Returns:
        Filtered list of users

    Example criteria:
    {
        "country": "United States",
        "age_min": 25,
        "age_max": 35,
        "gender": "female"
    }
    """
    # TODO: Implement flexible filtering
    pass
```

### Challenge 2: Statistical Analysis

Create functions to analyze user data statistically.

```python
def analyze_user_demographics(users: List[Dict[str, Any]]) -> Dict[str, Any]:
    """
    Analyze demographic statistics from user data.

    Returns statistics on:
    - Age distribution
    - Gender distribution
    - Country distribution
    - Average age by country
    """
    # TODO: Calculate various statistics
    pass
```

---

## ✅ Solution Code

### Exercise 1 Solutions

```python
import requests
from typing import Dict, List, Any, Optional

def fetch_single_user() -> Dict[str, Any]:
    """Fetch a single random user."""
    response = requests.get("https://randomuser.me/api/")
    response.raise_for_status()
    data = response.json()
    return data["results"][0]

def fetch_multiple_users(count: int = 5, gender: Optional[str] = None) -> List[Dict[str, Any]]:
    """Fetch multiple users with optional gender filter."""
    params = {"results": count}
    if gender:
        params["gender"] = gender

    response = requests.get("https://randomuser.me/api/", params=params)
    response.raise_for_status()
    data = response.json()
    return data["results"]
```

### Exercise 2 Solutions

```python
def extract_basic_info(user: Dict[str, Any]) -> Dict[str, str]:
    """Extract basic user information."""
    return {
        "name": f"{user['name']['first']} {user['name']['last']}",
        "email": user["email"],
        "country": user["location"]["country"]
    }

def extract_contact_info(user: Dict[str, Any]) -> Dict[str, str]:
    """Extract contact information."""
    return {
        "phone": user.get("phone", "N/A"),
        "cell": user.get("cell", "N/A")
    }

def format_user_summary(user: Dict[str, Any]) -> str:
    """Create formatted user summary."""
    name = f"{user['name']['first']} {user['name']['last']}"
    email = user['email']
    country = user['location']['country']
    return f"{name} ({email}) from {country}"

def format_user_card(user: Dict[str, Any]) -> str:
    """Create detailed user card."""
    lines = [
        f"Name: {user['name']['first']} {user['name']['last']}",
        f"Age: {user['dob']['age']}",
        f"Location: {user['location']['city']}, {user['location']['country']}",
        f"Email: {user['email']}",
        f"Phone: {user['phone']}"
    ]
    return "\n".join(lines)
```

### Exercise 3 Solutions

```python
import csv

class UserDataTransformer:
    """Transform user data into various formats."""

    def transform_to_csv_row(self, user: Dict[str, Any]) -> List[str]:
        """Transform to CSV row."""
        name = f"{user['name']['first']} {user['name']['last']}"
        age = str(user['dob']['age'])
        gender = user['gender']
        country = user['location']['country']
        email = user['email']
        return [name, age, gender, country, email]

    def transform_to_simple_dict(self, user: Dict[str, Any]) -> Dict[str, Any]:
        """Transform to simplified format."""
        return {
            "name": f"{user['name']['first']} {user['name']['last']}",
            "age": user['dob']['age'],
            "gender": user['gender'],
            "country": user['location']['country'],
            "email": user['email']
        }

    def get_csv_header(self) -> List[str]:
        """Return CSV header."""
        return ["Name", "Age", "Gender", "Country", "Email"]
```

### Exercise 4 Solutions

```python
class SafeAPIClient:
    """API client with error handling."""

    def fetch_users_safe(self, count: int = 1) -> Optional[List[Dict[str, Any]]]:
        """Safely fetch users."""
        try:
            response = requests.get(
                f"{self.base_url}/api/",
                params={"results": count},
                timeout=self.timeout
            )
            response.raise_for_status()
            data = response.json()
            return data.get("results", [])
        except (requests.RequestException, ValueError) as e:
            print(f"Error fetching users: {e}")
            return None

    def validate_user_data(self, user: Dict[str, Any]) -> bool:
        """Validate user data structure."""
        required_fields = ["name", "email", "location", "dob"]
        for field in required_fields:
            if field not in user:
                return False

        # Check nested structures
        if not isinstance(user.get("name"), dict):
            return False
        if not isinstance(user.get("location"), dict):
            return False

        return True
```

---

## 🧪 Testing Your Solutions

Create a comprehensive test script:

```python
# test_api_integration.py
import api_client
import data_processor

def test_basic_api_calls():
    """Test basic API functionality."""
    # Test single user fetch
    user = api_client.fetch_single_user()
    assert user is not None
    assert "name" in user
    assert "email" in user

    # Test multiple users
    users = api_client.fetch_multiple_users(3)
    assert len(users) == 3

    print("✓ Basic API calls work")

def test_data_processing():
    """Test data processing functions."""
    user = api_client.fetch_single_user()

    # Test extraction functions
    basic = data_processor.extract_basic_info(user)
    assert "name" in basic
    assert "email" in basic

    # Test formatting
    summary = data_processor.format_user_summary(user)
    assert isinstance(summary, str)
    assert "@" in summary  # Should contain email

    print("✓ Data processing works")

def test_error_handling():
    """Test error handling."""
    client = api_client.SafeAPIClient()

    # Test normal operation
    users = client.fetch_users_safe(2)
    assert users is not None
    assert len(users) == 2

    # Test validation
    is_valid = client.validate_user_data(users[0])
    assert is_valid is True

    print("✓ Error handling works")

if __name__ == "__main__":
    test_basic_api_calls()
    test_data_processing()
    test_error_handling()
    print("🎉 All tests passed!")
```

---

## 📝 Key Takeaways

1. **API Integration**: Use requests library for HTTP operations
2. **Error Handling**: Always handle network, HTTP, and JSON errors
3. **Data Validation**: Validate API responses before processing
4. **Caching**: Implement caching to reduce API calls and improve performance
5. **Modular Design**: Create reusable functions and classes
6. **Data Transformation**: Convert API data to application-specific formats

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
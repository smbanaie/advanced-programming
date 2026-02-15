# Homework 1: Virtual Environment Setup

**Topic**: 2 - Python Environment & Dependency Management
**Section**: 1 of 2 sections (90 min workshop + homework)
**Level**: Beginner
**Prerequisites**: Basic Python installation

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Set up Virtual Environments**: Create and manage isolated Python environments
2. **Install Dependencies**: Use pip to install packages in virtual environments
3. **Manage Requirements**: Create and use requirements.txt files
4. **Troubleshoot Environment Issues**: Resolve common virtual environment problems

---

## 📋 Assignment Overview

Create a complete Python project setup with proper virtual environment management and dependency handling.

### Project Requirements

1. **Project Structure**
   ```
   my_python_project/
   ├── venv/                    # Virtual environment
   ├── src/
   │   └── main.py             # Main application
   ├── requirements.txt        # Dependencies list
   ├── .gitignore              # Git ignore file
   └── README.md               # Project documentation
   ```

2. **Virtual Environment Setup**
   - Create a virtual environment named `venv`
   - Activate the virtual environment
   - Verify isolation (check Python path)

3. **Dependency Management**
   - Install `requests` library (version 2.28.0 or later)
   - Install `python-dotenv` for environment variables
   - Create `requirements.txt` with exact versions
   - Test installations work correctly

4. **Application Development**
   - Create a simple script that uses both libraries
   - The script should make an API call and save response to file
   - Include proper error handling

---

## 🛠️ Step-by-Step Instructions

### Step 1: Project Initialization

```bash
# Create project directory
mkdir my_python_project
cd my_python_project

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# macOS/Linux:
# source venv/bin/activate
```

### Step 2: Install Dependencies

```bash
# Install required packages
pip install requests>=2.28.0 python-dotenv

# Generate requirements.txt
pip freeze > requirements.txt
```

### Step 3: Create Application

Create `src/main.py`:

```python
#!/usr/bin/env python3
"""
Simple API client that demonstrates virtual environment usage.
"""

import requests
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

def fetch_user_data(user_id):
    """
    Fetch user data from JSONPlaceholder API.

    Args:
        user_id (int): User ID to fetch

    Returns:
        dict: User data or None if error
    """
    try:
        url = f"https://jsonplaceholder.typicode.com/users/{user_id}"
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        return response.json()
    except requests.RequestException as e:
        print(f"Error fetching user data: {e}")
        return None

def save_to_file(data, filename):
    """
    Save data to JSON file.

    Args:
        data (dict): Data to save
        filename (str): Output filename
    """
    try:
        import json
        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(data, f, indent=2, ensure_ascii=False)
        print(f"Data saved to {filename}")
    except Exception as e:
        print(f"Error saving data: {e}")

def main():
    """Main application function."""
    print("Python Virtual Environment Demo")
    print("=" * 40)

    # Get user ID from environment or use default
    user_id = int(os.getenv('USER_ID', '1'))

    print(f"Fetching data for user ID: {user_id}")

    # Fetch user data
    user_data = fetch_user_data(user_id)

    if user_data:
        print(f"User: {user_data.get('name', 'Unknown')}")
        print(f"Email: {user_data.get('email', 'Unknown')}")

        # Save to file
        save_to_file(user_data, f"user_{user_id}.json")
    else:
        print("Failed to fetch user data")

if __name__ == "__main__":
    main()
```

### Step 4: Create Environment File

Create `.env` file:

```bash
# Environment variables for the application
USER_ID=1
```

### Step 5: Create Documentation

Create `README.md`:

```markdown
# My Python Project

A demonstration of Python virtual environment and dependency management.

## Setup

1. Create virtual environment:
   ```bash
   python -m venv venv
   ```

2. Activate virtual environment:
   ```bash
   # Windows
   venv\Scripts\activate
   
   # macOS/Linux
   source venv/bin/activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the application:
   ```bash
   python src/main.py
   ```

## Dependencies

- requests>=2.28.0
- python-dotenv

## Environment Variables

- `USER_ID`: User ID to fetch from API (default: 1)
```

---

## 📋 Submission Requirements

### Required Files
- [ ] Complete project directory structure
- [ ] Virtual environment (venv folder)
- [ ] requirements.txt with exact versions
- [ ] src/main.py with working application
- [ ] .env file with environment variables
- [ ] README.md with setup instructions
- [ ] .gitignore file

### Verification Steps
1. **Environment Check**:
   ```bash
   # Verify virtual environment
   which python  # Should show venv path
   pip list      # Should show installed packages
   ```

2. **Application Test**:
   ```bash
   python src/main.py
   # Should fetch user data and save to file
   ```

3. **Dependency Verification**:
   ```bash
   pip install -r requirements.txt  # Should work
   ```

### Documentation Requirements
- [ ] Clear setup instructions in README
- [ ] Comments in code explaining functionality
- [ ] Description of virtual environment benefits

---

## 🎯 Evaluation Criteria

### Technical Implementation (60%)
- [ ] Virtual environment properly created and activated
- [ ] All dependencies correctly installed and versioned
- [ ] Application runs without errors
- [ ] Proper error handling implemented
- [ ] Environment variables used correctly

### Code Quality (20%)
- [ ] Clean, readable code with comments
- [ ] Proper project structure
- [ ] Meaningful variable names
- [ ] PEP 8 style compliance

### Documentation (20%)
- [ ] Complete README with setup instructions
- [ ] Clear code comments
- [ ] Proper file organization
- [ ] Working example provided

---

## 🚀 Bonus Challenges

### Advanced Setup
1. **Multiple Environments**: Create separate environments for development and production
2. **Automated Setup**: Create a `setup.py` or shell script for automated environment setup
3. **Testing Environment**: Add pytest and create simple tests for the application

### Dependency Management
1. **Version Pinning**: Research and implement proper version constraints
2. **Security Updates**: Use tools to check for vulnerable dependencies
3. **Lock Files**: Explore using pip-tools or poetry for reproducible builds

---

## 📞 Getting Help

### Common Issues
- **Virtual environment not activating**: Check Python installation and permissions
- **Packages not installing**: Verify internet connection and package names
- **Import errors**: Ensure virtual environment is activated before running

### Resources
- [Python Virtual Environments Guide](https://docs.python.org/3/tutorial/venv.html)
- [pip Documentation](https://pip.pypa.io/en/stable/)
- [Virtual Environment Best Practices](https://realpython.com/python-virtual-environments-a-primer/)

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 2-3 hours
**Total Points**: 50
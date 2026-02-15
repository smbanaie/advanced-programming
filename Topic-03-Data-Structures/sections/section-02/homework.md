# Homework 2: User Data Processor with JSON Operations

**Topic**: 3 - Python Data Structures & JSON API Work
**Section**: 2 of 2 sections (90 min workshop + homework)
**Level**: Intermediate to Advanced
**Prerequisites**: Section 1 (Lists and Sets), Section 2 workshop

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Build Complete Applications**: Create a functional data processing system
2. **Integrate Multiple APIs**: Work with different data sources
3. **Implement Advanced JSON Operations**: Handle complex data transformations
4. **Create Production-Ready Code**: Include proper error handling and logging
5. **Design Modular Architecture**: Build reusable, maintainable components

---

## 📋 Assignment Overview

You will build a comprehensive **User Data Processor** that fetches user information from the randomuser.me API, processes and transforms the data, and provides multiple output formats. The application should demonstrate professional-level Python development practices.

### Project Requirements

**Core Features:**
- Fetch user data from randomuser.me API with configurable parameters
- Process and clean the JSON data
- Transform data into multiple formats (JSON, CSV, custom objects)
- Implement caching to reduce API calls
- Provide command-line interface for easy operation
- Include comprehensive error handling and logging

### Technical Specifications

1. **Project Structure**:
   ```
   user_data_processor/
   ├── src/
   │   ├── __init__.py
   │   ├── api_client.py          # API interaction module
   │   ├── data_processor.py      # Data processing logic
   │   ├── cache_manager.py       # Caching system
   │   ├── file_exporter.py       # Export utilities
   │   └── cli.py                 # Command-line interface
   ├── tests/
   │   ├── test_api_client.py
   │   ├── test_data_processor.py
   │   └── test_cache_manager.py
   ├── config/
   │   └── settings.py            # Configuration management
   ├── logs/                      # Log files directory
   ├── pyproject.toml             # Project configuration
   └── README.md                  # Documentation
   ```

2. **Dependencies**:
   - `requests` for HTTP API calls
   - `click` for command-line interface
   - `pydantic` for data validation
   - `python-dotenv` for environment configuration
   - `pytest` for testing

---

## 🛠️ Implementation Requirements

### 1. Configuration Management

Create `config/settings.py`:

```python
"""Application configuration settings."""

import os
from typing import Optional
from pydantic import BaseSettings


class Settings(BaseSettings):
    """Application settings with validation."""

    # API Configuration
    api_base_url: str = "https://randomuser.me"
    api_timeout: int = 30
    default_user_count: int = 10
    max_user_count: int = 5000

    # Cache Configuration
    cache_enabled: bool = True
    cache_ttl_seconds: int = 600  # 10 minutes
    cache_dir: str = "cache"

    # Export Configuration
    export_dir: str = "exports"
    supported_formats: list = ["json", "csv", "txt"]

    # Logging Configuration
    log_level: str = "INFO"
    log_file: str = "logs/user_processor.log"
    log_max_size: int = 10 * 1024 * 1024  # 10MB
    log_backup_count: int = 5

    class Config:
        env_file = ".env"
        case_sensitive = False


settings = Settings()
```

### 2. API Client Implementation

Create `src/api_client.py`:

```python
"""API client for fetching user data from randomuser.me."""

import requests
import logging
from typing import List, Dict, Any, Optional
from datetime import datetime, timedelta
from .config import settings

logger = logging.getLogger(__name__)


class UserAPIClient:
    """Client for randomuser.me API with caching and error handling."""

    def __init__(self):
        self.base_url = settings.api_base_url
        self.session = requests.Session()
        self.session.timeout = settings.api_timeout

    def fetch_users(
        self,
        count: int = None,
        gender: Optional[str] = None,
        nationality: Optional[str] = None
    ) -> List[Dict[str, Any]]:
        """
        Fetch users from API with optional filtering.

        Args:
            count: Number of users to fetch
            gender: Filter by gender ('male', 'female')
            nationality: Filter by nationality code(s)

        Returns:
            List of user dictionaries

        Raises:
            APIError: If API request fails
        """
        # TODO: Implement API call with parameters
        # TODO: Handle errors appropriately
        # TODO: Return processed user data
        pass

    def close(self):
        """Close the HTTP session."""
        self.session.close()


class APIError(Exception):
    """Custom exception for API-related errors."""
    pass
```

### 3. Data Processor Implementation

Create `src/data_processor.py`:

```python
"""Data processing utilities for user information."""

import logging
from typing import Dict, List, Any, Optional
from datetime import datetime
from .config import settings

logger = logging.getLogger(__name__)


class UserDataProcessor:
    """Process and transform user data."""

    def __init__(self):
        self.processed_count = 0

    def clean_user_data(self, user: Dict[str, Any]) -> Dict[str, Any]:
        """
        Clean and normalize user data.

        Args:
            user: Raw user data from API

        Returns:
            Cleaned and normalized user data
        """
        # TODO: Implement data cleaning logic
        # TODO: Handle missing fields
        # TODO: Normalize data formats
        pass

    def extract_basic_info(self, user: Dict[str, Any]) -> Dict[str, Any]:
        """
        Extract basic user information.

        Args:
            user: Cleaned user data

        Returns:
            Dictionary with essential user information
        """
        # TODO: Extract name, email, age, location, etc.
        pass

    def categorize_users(self, users: List[Dict[str, Any]]) -> Dict[str, List[Dict[str, Any]]]:
        """
        Categorize users by various criteria.

        Args:
            users: List of user dictionaries

        Returns:
            Dictionary with users categorized by country, age group, etc.
        """
        # TODO: Group users by country
        # TODO: Group users by age ranges
        # TODO: Group users by gender
        pass

    def generate_statistics(self, users: List[Dict[str, Any]]) -> Dict[str, Any]:
        """
        Generate statistical analysis of user data.

        Args:
            users: List of user dictionaries

        Returns:
            Dictionary with various statistics
        """
        # TODO: Calculate age statistics
        # TODO: Calculate gender distribution
        # TODO: Calculate country distribution
        pass
```

### 4. Cache Manager Implementation

Create `src/cache_manager.py`:

```python
"""Cache management system for API responses."""

import json
import hashlib
import logging
from pathlib import Path
from typing import Dict, Any, Optional
from datetime import datetime, timedelta
from .config import settings

logger = logging.getLogger(__name__)


class CacheManager:
    """File-based cache manager with TTL support."""

    def __init__(self):
        self.cache_dir = Path(settings.cache_dir)
        self.cache_dir.mkdir(exist_ok=True)
        self.ttl = timedelta(seconds=settings.cache_ttl_seconds)

    def _get_cache_key(self, params: Dict[str, Any]) -> str:
        """Generate cache key from parameters."""
        # TODO: Create unique key from sorted parameters
        pass

    def _get_cache_path(self, cache_key: str) -> Path:
        """Get cache file path for key."""
        return self.cache_dir / f"{cache_key}.json"

    def get(self, params: Dict[str, Any]) -> Optional[List[Dict[str, Any]]]:
        """
        Retrieve data from cache if available and valid.

        Args:
            params: API parameters used for cache key

        Returns:
            Cached data or None if not available/expired
        """
        # TODO: Generate cache key
        # TODO: Check if cache file exists and is valid
        # TODO: Load and return cached data
        pass

    def set(self, params: Dict[str, Any], data: List[Dict[str, Any]]):
        """
        Store data in cache.

        Args:
            params: API parameters used for cache key
            data: Data to cache
        """
        # TODO: Generate cache key
        # TODO: Save data with timestamp
        pass

    def clear_expired(self):
        """Clear expired cache entries."""
        # TODO: Remove cache files older than TTL
        pass

    def clear_all(self):
        """Clear all cached data."""
        # TODO: Remove all cache files
        pass
```

### 5. File Exporter Implementation

Create `src/file_exporter.py`:

```python
"""File export utilities for different formats."""

import csv
import json
import logging
from pathlib import Path
from typing import List, Dict, Any
from .config import settings

logger = logging.getLogger(__name__)


class DataExporter:
    """Export processed data to various file formats."""

    def __init__(self):
        self.export_dir = Path(settings.export_dir)
        self.export_dir.mkdir(exist_ok=True)

    def export_json(self, data: List[Dict[str, Any]], filename: str):
        """
        Export data to JSON format.

        Args:
            data: Data to export
            filename: Output filename (without extension)
        """
        # TODO: Export to JSON with proper formatting
        pass

    def export_csv(self, data: List[Dict[str, Any]], filename: str):
        """
        Export data to CSV format.

        Args:
            data: Data to export (list of dictionaries)
            filename: Output filename (without extension)
        """
        # TODO: Convert to CSV format
        # TODO: Handle different data types appropriately
        pass

    def export_summary_report(self, stats: Dict[str, Any], filename: str):
        """
        Export statistics summary to text file.

        Args:
            stats: Statistics data
            filename: Output filename (without extension)
        """
        # TODO: Create readable summary report
        pass

    def export_all_formats(self, users: List[Dict[str, Any]], stats: Dict[str, Any], base_filename: str):
        """
        Export data in all supported formats.

        Args:
            users: User data
            stats: Statistics data
            base_filename: Base filename for all exports
        """
        # TODO: Export users in all formats
        # TODO: Export statistics as summary
        pass
```

### 6. Command-Line Interface

Create `src/cli.py`:

```python
"""Command-line interface for the User Data Processor."""

import click
import logging
from pathlib import Path
from .api_client import UserAPIClient
from .data_processor import UserDataProcessor
from .cache_manager import CacheManager
from .file_exporter import DataExporter
from .config import settings

# Setup logging
logging.basicConfig(
    level=getattr(logging, settings.log_level),
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler(settings.log_file),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)


@click.group()
@click.option('--debug', is_flag=True, help='Enable debug logging')
@click.pass_context
def cli(ctx, debug):
    """User Data Processor - Fetch and process user information."""
    if debug:
        logging.getLogger().setLevel(logging.DEBUG)

    # Initialize components
    ctx.ensure_object(dict)
    ctx.obj['api_client'] = UserAPIClient()
    ctx.obj['processor'] = UserDataProcessor()
    ctx.obj['cache'] = CacheManager()
    ctx.obj['exporter'] = DataExporter()


@cli.command()
@click.option('--count', default=None, type=int, help='Number of users to fetch')
@click.option('--gender', type=click.Choice(['male', 'female']), help='Filter by gender')
@click.option('--nationality', help='Filter by nationality (comma-separated)')
@click.option('--output', help='Output filename (without extension)')
@click.option('--format', 'output_format', type=click.Choice(['json', 'csv', 'all']),
              default='json', help='Output format')
@click.pass_context
def fetch(ctx, count, gender, nationality, output, output_format):
    """Fetch and process user data."""
    # TODO: Implement fetch command
    # TODO: Use API client to fetch data
    # TODO: Process the data
    # TODO: Export in requested format
    pass


@cli.command()
@click.option('--input', required=True, help='Input file to analyze')
@click.pass_context
def analyze(ctx, input):
    """Analyze existing user data file."""
    # TODO: Load data from file
    # TODO: Generate statistics
    # TODO: Display analysis results
    pass


@cli.command()
@click.pass_context
def cache_clear(ctx):
    """Clear all cached data."""
    # TODO: Clear cache using cache manager
    pass


@cli.command()
@click.option('--input-dir', default=str(settings.export_dir), help='Directory to scan')
@click.pass_context
def list_exports(ctx, input_dir):
    """List all exported files."""
    # TODO: Scan export directory
    # TODO: Display available files
    pass


if __name__ == '__main__':
    try:
        cli()
    except Exception as e:
        logger.error(f"Application error: {e}")
        raise
```

### 7. Project Configuration

Create `pyproject.toml`:

```toml
[project]
name = "user-data-processor"
version = "0.1.0"
description = "A comprehensive user data processing application"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.28.0",
    "click>=8.0.0",
    "pydantic>=1.8.0",
    "python-dotenv>=0.19.0"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project.scripts]
user-processor = "src.cli:cli"

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = "test_*.py"
python_classes = "Test*"
python_functions = "test_*"

[tool.coverage.run]
source = ["src"]
omit = ["tests/*", "config/*"]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "raise AssertionError",
    "raise NotImplementedError"
]
```

---

## 📋 Submission Requirements

### Required Deliverables

1. **Complete Project Structure**:
   - All source files implemented
   - Proper directory structure
   - Configuration files

2. **Working Application**:
   - Functional command-line interface
   - Successful API integration
   - Data processing and export capabilities

3. **Comprehensive Tests**:
   - Unit tests for all major components
   - Integration tests for API functionality
   - Error handling tests

4. **Documentation**:
   - Complete README with usage instructions
   - Code documentation and comments
   - API documentation

### Testing the Application

```bash
# Install dependencies
pip install -e .

# Run basic commands
user-processor fetch --count 5 --output sample_users
user-processor analyze --input sample_users.json
user-processor cache-clear

# Run tests
pytest

# Check test coverage
pytest --cov=src --cov-report=html
```

### Key Features to Verify

- [ ] API calls work without errors
- [ ] Caching reduces redundant API calls
- [ ] All export formats work correctly
- [ ] Error handling prevents crashes
- [ ] Command-line interface is user-friendly
- [ ] Logging provides useful debugging information

---

## 🎯 Evaluation Criteria

### Functionality (40%)
- [ ] API integration works correctly
- [ ] Data processing transforms data properly
- [ ] Caching system functions as expected
- [ ] Export functionality supports all formats
- [ ] CLI provides intuitive user experience
- [ ] Error handling prevents application crashes

### Code Quality (30%)
- [ ] Modular, well-organized code structure
- [ ] Proper use of type hints and docstrings
- [ ] Clean, readable code following PEP 8
- [ ] Effective use of object-oriented principles
- [ ] Proper error handling and logging

### Testing (20%)
- [ ] Comprehensive unit test coverage (80%+)
- [ ] Tests verify both success and failure cases
- [ ] Integration tests for API functionality
- [ ] Proper test organization and naming

### Documentation (10%)
- [ ] Complete README with setup and usage instructions
- [ ] Well-documented code with docstrings
- [ ] Clear project structure documentation
- [ ] Helpful error messages and logging

---

## 🚀 Bonus Challenges

### Advanced Features
1. **Web Dashboard**: Add a simple web interface using Flask/FastAPI
2. **Database Integration**: Store processed data in SQLite/PostgreSQL
3. **Async Processing**: Implement asynchronous API calls for better performance
4. **Data Visualization**: Add charts and graphs using matplotlib
5. **API Rate Limiting**: Implement intelligent rate limiting and retry logic

### Production Enhancements
1. **Docker Containerization**: Package the application in a Docker container
2. **Configuration Management**: Support multiple configuration environments
3. **Monitoring**: Add application metrics and health checks
4. **Security**: Implement API key rotation and secure credential management

---

## 📞 Getting Help

### Common Issues
- **API Rate Limits**: randomuser.me has rate limits; implement caching
- **File Permissions**: Ensure write permissions for cache and export directories
- **Import Errors**: Verify all dependencies are installed correctly
- **Encoding Issues**: Handle UTF-8 encoding properly for international names

### Resources
- [RandomUser.me API Documentation](https://randomuser.me/documentation)
- [Click Documentation](https://click.palletsprojects.com/)
- [Pydantic Documentation](https://pydantic-docs.helpmanual.io/)
- [Python Logging Documentation](https://docs.python.org/3/library/logging.html)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **API Integration**: Building robust HTTP clients
- **Data Processing**: Complex JSON manipulation and transformation
- **Caching Strategies**: Implementing efficient data caching
- **Error Handling**: Comprehensive exception management
- **CLI Development**: Creating user-friendly command-line applications
- **Testing Practices**: Writing maintainable, comprehensive tests
- **Project Structure**: Organizing large Python applications

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 8-12 hours
**Total Points**: 100
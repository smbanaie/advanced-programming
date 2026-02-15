# Homework 1: Extract Data from Multiple Pages

**Topic**: 8 - Web Scraping Fundamentals
**Section**: 1 of 3 sections (90 min workshop + homework)
**Level**: Intermediate to Advanced
**Prerequisites**: Section 1 tutorial and workshop

---

## 🎯 Learning Objectives

After completing this homework, you will demonstrate the ability to:

1. **Build Multi-Page Scrapers**: Extract data across multiple web pages
2. **Implement Pagination Handling**: Navigate through paginated content
3. **Handle Complex Data Structures**: Parse nested and varied HTML structures
4. **Apply Rate Limiting**: Respect server limits with intelligent delays
5. **Manage Large-Scale Scraping**: Handle hundreds of pages efficiently
6. **Implement Error Recovery**: Build resilient scrapers with retry logic
7. **Store Structured Data**: Save large datasets in multiple formats
8. **Monitor Scraping Progress**: Track and report scraping statistics

---

## 📋 Assignment Overview

You will build a comprehensive web scraper that extracts data from multiple pages of a website. The scraper should handle pagination, implement proper rate limiting, include comprehensive error handling, and save the extracted data in multiple formats. This assignment demonstrates production-ready web scraping skills.

### Project Requirements

**Core Features:**
- Scrape data from a multi-page website (using httpbin.org/html as base)
- Handle pagination automatically
- Extract structured data from HTML tables and lists
- Implement rate limiting and respect robots.txt
- Comprehensive error handling with retry logic
- Save data in JSON, CSV, and optionally database formats
- Progress monitoring and statistics reporting
- Configuration management for different environments

### Technical Specifications

1. **Project Structure**:
   ```
   multi_page_scraper/
   ├── src/
   │   ├── scraper.py           # Main scraper class
   │   ├── data_processor.py    # Data processing utilities
   │   ├── storage.py           # Data storage handlers
   │   └── config.py            # Configuration management
   ├── data/
   │   ├── scraped_data.json    # Raw scraped data
   │   ├── processed_data.csv   # Processed CSV data
   │   └── scrape_stats.json    # Scraping statistics
   ├── logs/
   │   └── scraper.log          # Scraping logs
   ├── pyproject.toml           # Project configuration
   └── README.md                # Documentation
   ```

2. **Dependencies**:
   - `requests` - HTTP client
   - `beautifulsoup4` - HTML parsing
   - `lxml` or `html5lib` - Alternative parsers
   - Optional: `pandas` for data processing

---

## 🛠️ Implementation Requirements

### 1. Configuration Management

Create `src/config.py` with scraper configuration:

```python
"""Configuration management for the scraper."""

import os
from typing import Dict, Any

class ScraperConfig:
    """Configuration for web scraping operations."""

    # Base configuration
    BASE_URL = "https://httpbin.org"
    REQUEST_TIMEOUT = int(os.getenv('SCRAPER_TIMEOUT', '10'))
    MAX_RETRIES = int(os.getenv('SCRAPER_MAX_RETRIES', '3'))
    RATE_LIMIT_DELAY = float(os.getenv('SCRAPER_DELAY', '1.0'))

    # Pagination settings
    MAX_PAGES = int(os.getenv('SCRAPER_MAX_PAGES', '10'))
    ITEMS_PER_PAGE = int(os.getenv('SCRAPER_ITEMS_PER_PAGE', '20'))

    # Data storage
    DATA_DIR = os.getenv('SCRAPER_DATA_DIR', 'data')
    LOG_DIR = os.getenv('SCRAPER_LOG_DIR', 'logs')

    # Logging
    LOG_LEVEL = os.getenv('SCRAPER_LOG_LEVEL', 'INFO')
    LOG_FILE = os.path.join(LOG_DIR, 'scraper.log')
    LOG_MAX_SIZE = 10 * 1024 * 1024  # 10MB
    LOG_BACKUP_COUNT = 5

    # Output formats
    SUPPORTED_FORMATS = ['json', 'csv', 'txt']

    @classmethod
    def get_config(cls) -> Dict[str, Any]:
        """Get complete configuration dictionary."""
        # TODO: Return all configuration as dictionary
        pass

    @classmethod
    def validate_config(cls) -> bool:
        """Validate configuration values."""
        # TODO: Check if all required values are valid
        # TODO: Validate URLs, paths, numeric ranges
        pass

# Global config instance
config = ScraperConfig()
```

### 2. Data Models

Create data models for scraped content:

```python
"""Data models for scraped content."""

from pydantic import BaseModel, Field
from typing import List, Optional, Dict, Any
from datetime import datetime

class ScrapedItem(BaseModel):
    """Model for individual scraped items."""
    # TODO: Define fields based on website content
    # - id: unique identifier
    # - title: item title
    # - content: main content
    # - url: source URL
    # - scraped_at: timestamp

class PageData(BaseModel):
    """Model for page-level data."""
    # TODO: Define page metadata
    # - page_number: int
    # - url: str
    # - item_count: int
    # - scraped_at: datetime

class ScrapingResult(BaseModel):
    """Model for complete scraping results."""
    # TODO: Define overall results
    # - pages_scraped: int
    # - total_items: int
    # - start_time: datetime
    # - end_time: datetime
    # - success_rate: float
    # - errors: List[str]

class ScrapingStats(BaseModel):
    """Statistics for scraping operations."""
    # TODO: Define comprehensive statistics
    # - total_requests: int
    # - successful_requests: int
    # - failed_requests: int
    # - total_data_size: int
    # - average_response_time: float
    # - errors_by_type: Dict[str, int]
```

### 3. Main Scraper Class

Create `src/scraper.py` with the core scraping logic:

```python
"""Main web scraper for multi-page data extraction."""

import requests
from bs4 import BeautifulSoup
import time
import logging
from typing import List, Dict, Any, Optional, Iterator
from urllib.parse import urljoin, urlparse
from pathlib import Path
import json

from .config import config
from .data_processor import DataProcessor

class MultiPageScraper:
    """Scraper for extracting data from multiple web pages."""

    def __init__(self):
        """Initialize the scraper."""
        # TODO: Set up logging
        # TODO: Initialize HTTP session
        # TODO: Set up rate limiting
        # TODO: Initialize data processor
        # TODO: Create output directories

    def setup_logging(self):
        """Configure logging for the scraper."""
        # TODO: Set up logging with file and console handlers
        # TODO: Configure log rotation
        # TODO: Set appropriate log level

    def setup_session(self):
        """Configure HTTP session with headers."""
        # TODO: Create requests session
        # TODO: Set user agent and other headers
        # TODO: Configure timeouts and retries

    def check_robots_txt(self) -> bool:
        """Check if scraping is allowed by robots.txt."""
        # TODO: Fetch and parse robots.txt
        # TODO: Check if scraping is allowed
        # TODO: Log the result

    def get_page_urls(self) -> List[str]:
        """Generate list of page URLs to scrape."""
        # TODO: Create URLs for multiple pages
        # TODO: Respect MAX_PAGES limit
        # TODO: Validate URLs are properly formatted

    def fetch_page(self, url: str, retry_count: int = 0) -> Optional[str]:
        """Fetch a single page with error handling and retries."""
        # TODO: Implement rate limiting
        # TODO: Make HTTP request with timeout
        # TODO: Handle various error types
        # TODO: Implement retry logic with exponential backoff
        # TODO: Log all operations and errors

    def parse_page(self, html_content: str, url: str) -> Dict[str, Any]:
        """Parse HTML content and extract structured data."""
        # TODO: Parse HTML with BeautifulSoup
        # TODO: Extract relevant data using CSS selectors
        # TODO: Handle parsing errors gracefully
        # TODO: Return structured data dictionary

    def scrape_single_page(self, url: str) -> Optional[Dict[str, Any]]:
        """Scrape a single page completely."""
        # TODO: Fetch page content
        # TODO: Parse and extract data
        # TODO: Update statistics
        # TODO: Return page data or None on failure

    def scrape_all_pages(self) -> Dict[str, Any]:
        """Scrape all pages and aggregate results."""
        # TODO: Get list of page URLs
        # TODO: Scrape each page
        # TODO: Aggregate results
        # TODO: Handle partial failures
        # TODO: Update comprehensive statistics

    def save_results(self, results: Dict[str, Any], output_formats: List[str] = None):
        """Save scraping results in multiple formats."""
        # TODO: Save to JSON format
        # TODO: Save to CSV format if requested
        # TODO: Save statistics separately
        # TODO: Log save operations

    def generate_report(self, results: Dict[str, Any]) -> str:
        """Generate a human-readable scraping report."""
        # TODO: Create summary of scraping operation
        # TODO: Include statistics and key metrics
        # TODO: List any errors or issues encountered
        # TODO: Provide recommendations for improvement

    def cleanup(self):
        """Clean up resources and close connections."""
        # TODO: Close HTTP session
        # TODO: Flush any pending operations
        # TODO: Log cleanup completion

# Usage example
if __name__ == "__main__":
    scraper = MultiPageScraper()

    # Check if scraping is allowed
    if not scraper.check_robots_txt():
        print("Scraping not allowed by robots.txt")
        exit(1)

    # Perform scraping
    results = scraper.scrape_all_pages()

    # Save results
    scraper.save_results(results, ['json', 'csv'])

    # Generate report
    report = scraper.generate_report(results)
    print("Scraping Report:")
    print(report)

    # Cleanup
    scraper.cleanup()
```

### 4. Data Processing Utilities

Create `src/data_processor.py` for data cleaning and processing:

```python
"""Data processing utilities for scraped content."""

import re
from typing import Dict, List, Any, Optional
from datetime import datetime

class DataProcessor:
    """Process and clean scraped data."""

    @staticmethod
    def clean_text(text: str) -> str:
        """Clean and normalize text data."""
        # TODO: Remove extra whitespace
        # TODO: Handle HTML entities
        # TODO: Normalize unicode characters
        # TODO: Strip unwanted characters

    @staticmethod
    def extract_numbers(text: str) -> List[float]:
        """Extract all numbers from text."""
        # TODO: Find numeric patterns
        # TODO: Handle different formats (integers, decimals, commas)
        # TODO: Return list of extracted numbers

    @staticmethod
    def normalize_url(url: str, base_url: str) -> str:
        """Normalize relative URLs to absolute."""
        # TODO: Convert relative URLs to absolute
        # TODO: Handle different URL formats
        # TODO: Validate final URL

    @staticmethod
    def extract_dates(text: str) -> List[str]:
        """Extract date patterns from text."""
        # TODO: Find common date formats
        # TODO: Standardize date formats
        # TODO: Return list of extracted dates

    @staticmethod
    def categorize_content(content: str) -> Dict[str, Any]:
        """Categorize and analyze content."""
        # TODO: Analyze text length, word count, etc.
        # TODO: Detect content type (article, list, table, etc.)
        # TODO: Extract key metrics

    @classmethod
    def process_item_data(cls, raw_data: Dict[str, Any]) -> Dict[str, Any]:
        """Process a single item's raw data."""
        # TODO: Clean all text fields
        # TODO: Extract and normalize URLs
        # TODO: Extract numbers and dates
        # TODO: Categorize content
        # TODO: Return processed data

    @classmethod
    def validate_item_data(cls, data: Dict[str, Any]) -> bool:
        """Validate processed item data."""
        # TODO: Check required fields exist
        # TODO: Validate data types
        # TODO: Check data quality (non-empty, reasonable lengths)
        # TODO: Return validation result

    @classmethod
    def batch_process_items(cls, items: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
        """Process a batch of items."""
        # TODO: Process each item
        # TODO: Validate processed items
        # TODO: Filter out invalid items
        # TODO: Log processing statistics
```

### 5. Storage Handlers

Create `src/storage.py` for data persistence:

```python
"""Data storage handlers for different formats."""

import json
import csv
from pathlib import Path
from typing import Dict, List, Any
from .config import config

class DataStorage:
    """Handle data storage in multiple formats."""

    def __init__(self):
        """Initialize storage with data directory."""
        # TODO: Create data directory if it doesn't exist
        # TODO: Set up file paths

    def save_json(self, data: Dict[str, Any], filename: str):
        """Save data to JSON format."""
        # TODO: Ensure data directory exists
        # TODO: Save with proper formatting and encoding
        # TODO: Handle file I/O errors

    def save_csv(self, data: List[Dict[str, Any]], filename: str):
        """Save data to CSV format."""
        # TODO: Convert dictionary data to CSV format
        # TODO: Handle different data types appropriately
        # TODO: Save with proper encoding

    def save_text_report(self, content: str, filename: str):
        """Save text content to file."""
        # TODO: Save text content with UTF-8 encoding
        # TODO: Handle file I/O errors

    def load_previous_results(self, filename: str) -> Optional[Dict[str, Any]]:
        """Load previously saved results."""
        # TODO: Load JSON data if file exists
        # TODO: Handle file not found or corrupted data
        # TODO: Return loaded data or None

    def backup_existing_files(self, filename: str):
        """Create backup of existing files before overwriting."""
        # TODO: Check if file exists
        # TODO: Create timestamped backup
        # TODO: Log backup operation
```

### 6. Main Application

Create a complete command-line application:

```python
"""Command-line interface for the multi-page scraper."""

import argparse
import sys
from pathlib import Path
from .scraper import MultiPageScraper
from .config import config

def main():
    """Main application entry point."""
    parser = argparse.ArgumentParser(
        description="Multi-page web scraper",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  python -m multi_page_scraper              # Scrape with default settings
  python -m multi_page_scraper --pages 5    # Scrape 5 pages
  python -m multi_page_scraper --delay 2.0  # 2 second delay between requests
  python -m multi_page_scraper --formats json csv  # Multiple output formats
        """
    )

    parser.add_argument(
        '--pages', '-p',
        type=int,
        default=config.MAX_PAGES,
        help=f'Maximum number of pages to scrape (default: {config.MAX_PAGES})'
    )

    parser.add_argument(
        '--delay', '-d',
        type=float,
        default=config.RATE_LIMIT_DELAY,
        help=f'Delay between requests in seconds (default: {config.RATE_LIMIT_DELAY})'
    )

    parser.add_argument(
        '--formats', '-f',
        nargs='+',
        choices=config.SUPPORTED_FORMATS,
        default=['json'],
        help=f'Output formats (default: json). Choices: {config.SUPPORTED_FORMATS}'
    )

    parser.add_argument(
        '--verbose', '-v',
        action='store_true',
        help='Enable verbose logging'
    )

    parser.add_argument(
        '--dry-run',
        action='store_true',
        help='Show what would be scraped without actually scraping'
    )

    args = parser.parse_args()

    # Validate configuration
    if not config.validate_config():
        print("❌ Invalid configuration. Please check settings.")
        sys.exit(1)

    # Create scraper
    scraper = MultiPageScraper()

    # Configure based on arguments
    scraper.max_pages = args.pages
    scraper.request_delay = args.delay

    if args.verbose:
        # Enable debug logging
        pass

    print(f"🚀 Starting multi-page scraper")
    print(f"📄 Pages to scrape: {args.pages}")
    print(f"⏱️  Request delay: {args.delay}s")
    print(f"💾 Output formats: {', '.join(args.formats)}")

    if args.dry_run:
        print("🔍 Dry run mode - showing what would be scraped")
        # Show URLs that would be scraped
        urls = scraper.get_page_urls()
        for i, url in enumerate(urls[:5], 1):
            print(f"  {i}. {url}")
        if len(urls) > 5:
            print(f"  ... and {len(urls) - 5} more")
        print("✅ Dry run completed")
        return

    try:
        # Perform scraping
        results = scraper.scrape_all_pages()

        # Save results
        scraper.save_results(results, args.formats)

        # Generate and display report
        report = scraper.generate_report(results)
        print("\n📊 Scraping Report:")
        print(report)

        print("✅ Scraping completed successfully!")

    except KeyboardInterrupt:
        print("\n⚠️  Scraping interrupted by user")
    except Exception as e:
        print(f"\n❌ Scraping failed: {e}")
        sys.exit(1)
    finally:
        scraper.cleanup()

if __name__ == "__main__":
    main()
```

---

## 📋 Submission Requirements

### Required Deliverables

1. **Complete Scraper Implementation**:
   - All core scraper functionality implemented
   - Proper error handling and rate limiting
   - Comprehensive logging throughout

2. **Data Processing Pipeline**:
   - Raw data extraction from HTML
   - Data cleaning and validation
   - Multiple output format support

3. **Configuration Management**:
   - Environment-based configuration
   - Command-line argument support
   - Validation of configuration values

4. **Testing and Validation**:
   - Scraper tested with multiple pages
   - Error handling verified
   - Data quality checks implemented

### Output Formats Required

**JSON Output** (`data/scraped_data.json`):
```json
{
  "metadata": {
    "total_pages": 10,
    "total_items": 200,
    "scraped_at": "2023-12-07T10:30:00Z",
    "success_rate": 0.95
  },
  "pages": [
    {
      "page_number": 1,
      "url": "https://example.com/page1",
      "items": [
        {
          "id": "item1",
          "title": "Sample Item",
          "content": "Item content...",
          "scraped_at": "2023-12-07T10:30:05Z"
        }
      ]
    }
  ],
  "statistics": {
    "total_requests": 10,
    "successful_requests": 9,
    "failed_requests": 1,
    "average_response_time": 1.2
  }
}
```

**CSV Output** (`data/processed_data.csv`):
```
id,title,content,page_number,scraped_at
item1,Sample Item,Item content...,1,2023-12-07T10:30:05Z
item2,Another Item,More content...,1,2023-12-07T10:30:06Z
```

### Scraping Statistics Required

**Statistics Output** (`data/scrape_stats.json`):
```json
{
  "scraping_session": {
    "start_time": "2023-12-07T10:30:00Z",
    "end_time": "2023-12-07T10:32:15Z",
    "duration_seconds": 135
  },
  "performance": {
    "pages_per_minute": 4.4,
    "items_per_minute": 88.9,
    "average_response_time": 1.2,
    "success_rate": 0.95
  },
  "data_quality": {
    "total_items": 200,
    "valid_items": 190,
    "invalid_items": 10,
    "duplicate_items": 0
  },
  "errors": {
    "total_errors": 2,
    "errors_by_type": {
      "timeout": 1,
      "http_404": 1
    },
    "error_messages": [
      "Timeout fetching https://example.com/page8",
      "HTTP 404 for https://example.com/page9"
    ]
  }
}
```

---

## 🎯 Evaluation Criteria

### Scraping Implementation (35%)
- [ ] Multi-page scraping functionality works correctly
- [ ] Pagination handling implemented properly
- [ ] Rate limiting and robots.txt compliance
- [ ] Comprehensive error handling and retry logic
- [ ] Progress monitoring and statistics tracking

### Data Processing (25%)
- [ ] HTML parsing extracts correct data
- [ ] Data cleaning and validation implemented
- [ ] Multiple output formats supported
- [ ] Data quality checks and error detection
- [ ] Structured data organization

### Code Quality (20%)
- [ ] Clean, modular, well-documented code
- [ ] Proper error handling and logging
- [ ] Configuration management implemented
- [ ] Type hints and docstrings used
- [ ] PEP 8 compliance

### Robustness and Monitoring (20%)
- [ ] Handles network failures gracefully
- [ ] Comprehensive logging and monitoring
- [ ] Resource cleanup and session management
- [ ] Performance monitoring and optimization
- [ ] Command-line interface works correctly

---

## 🚀 Bonus Challenges

### Advanced Features
1. **Concurrent Scraping**: Implement async scraping for multiple pages simultaneously
2. **Proxy Rotation**: Add proxy support for large-scale scraping
3. **Content Type Detection**: Automatically handle JSON APIs, XML feeds, and HTML pages
4. **Incremental Scraping**: Only scrape changed content based on timestamps
5. **API Integration**: Store scraped data directly in a database

### Production Enhancements
1. **Docker Containerization**: Package the scraper in a Docker container
2. **Monitoring Dashboard**: Create a web interface to monitor scraping progress
3. **Scheduled Scraping**: Add cron job support for regular scraping
4. **Data Validation**: Implement schema validation for scraped data
5. **Resume Capability**: Allow scraping to resume from interruption points

---

## 📞 Getting Help

### Common Issues
- **Rate Limiting Too Aggressive**: Increase delays or implement smarter rate limiting
- **Memory Usage**: Process data in batches rather than loading everything
- **Encoding Issues**: Always specify UTF-8 encoding for file operations
- **Selector Changes**: Websites change structure; implement flexible selectors
- **IP Blocking**: Use longer delays and rotate user agents if needed

### Testing Tips
- Start with small page counts (1-2 pages) for testing
- Use `--dry-run` to verify URL generation without actual scraping
- Check log files for detailed error information
- Validate output data quality before scaling up

### Resources
- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Requests Advanced Usage](https://requests.readthedocs.io/en/master/user/advanced/)
- [Web Scraping Ethics](https://blog.apify.com/web-scraping-ethics/)
- [robots.txt Specification](https://www.robotstxt.org/robotstxt.html)

---

## 📚 Learning Outcomes

This homework demonstrates mastery of:

- **Production Web Scraping**: Building robust, scalable scrapers
- **Multi-Page Handling**: Pagination, URL generation, and navigation
- **Error Recovery**: Retry logic, fallback strategies, and graceful failures
- **Data Pipeline**: Raw data extraction to cleaned, structured output
- **Performance Optimization**: Rate limiting, caching, and monitoring
- **Quality Assurance**: Data validation, testing, and monitoring
- **Ethical Scraping**: Legal compliance and server respect
- **Code Architecture**: Modular design and configuration management

---

**Homework Version**: 1.0
**Last Updated**: February 2026
**Estimated Time**: 8-12 hours
**Total Points**: 100
# Workshop 1: Scraping Simple Website

**Section**: 1 - Web Scraping Basics (90 min)
**Level**: Intermediate
**Prerequisites**: Tutorial 1 (Web Scraping Basics)

---

## 🎯 Workshop Objectives

By the end of this workshop, you will:

1. **Apply HTML Parsing**: Use BeautifulSoup to extract data from real websites
2. **Handle HTTP Requests**: Make robust web requests with error handling
3. **Implement CSS Selectors**: Target specific elements using modern selectors
4. **Extract Structured Data**: Parse and clean website data into usable formats
5. **Handle Edge Cases**: Deal with missing elements and malformed HTML
6. **Build Reusable Scrapers**: Create modular scraping functions and classes
7. **Implement Rate Limiting**: Respect website limits with proper delays

---

## 📋 Workshop Structure

1. [Setup and Environment](#setup-and-environment)
2. [Exercise 1: Basic HTML Parsing](#exercise-1-basic-html-parsing)
3. [Exercise 2: CSS Selector Practice](#exercise-2-css-selector-practice)
4. [Exercise 3: Data Extraction Functions](#exercise-3-data-extraction-functions)
5. [Exercise 4: Complete Website Scraper](#exercise-4-complete-website-scraper)
6. [Exercise 5: Error Handling and Robustness](#exercise-5-error-handling-and-robustness)
7. [Challenge Exercises](#challenge-exercises)
8. [Solution Code](#solution-code)

---

## 🛠️ Setup and Environment

### Create Workshop Directory

```bash
# Create workshop directory
mkdir workshop-scraping-basics
cd workshop-scraping-basics

# Create Python files
touch scraper.py test_scraper.py
touch data_extractor.py test_extractor.py

# Optional: Create virtual environment
python -m venv venv
# Windows: venv\Scripts\activate
# macOS/Linux: source venv/bin/activate

# Install required libraries
pip install requests beautifulsoup4

# Optional: Install additional tools
pip install lxml html5lib  # Alternative parsers
```

### Project Structure

```
workshop-scraping-basics/
├── scraper.py           # Main scraping functions
├── test_scraper.py      # Scraper tests
├── data_extractor.py    # Data extraction utilities
├── test_extractor.py    # Extraction tests
└── README.md           # Documentation (optional)
```

### Test Website Setup

We'll use httpbin.org/html as our test website. It provides consistent HTML content for testing:

```python
# Test the target website
import requests

response = requests.get("https://httpbin.org/html")
print(f"Status: {response.status_code}")
print(f"Content length: {len(response.text)}")
print("Sample HTML:")
print(response.text[:500])
```

---

## 📄 Exercise 1: Basic HTML Parsing

**Goal**: Learn to parse HTML and extract basic information

### Task 1.1: HTML Structure Analysis

Create a function to analyze the basic structure of an HTML page:

```python
from bs4 import BeautifulSoup
import requests

def analyze_html_structure(url):
    """Analyze and print the basic structure of an HTML page."""
    # TODO: Make request to URL
    # TODO: Parse HTML with BeautifulSoup
    # TODO: Print basic information:
    # - Title
    # - Number of headings (h1, h2, h3, etc.)
    # - Number of paragraphs
    # - Number of links
    # - Number of div elements

# Test with httpbin.org/html
analyze_html_structure("https://httpbin.org/html")
```

**Expected Output:**
```
Title: Herman Melville - Moby-Dick
Headings: H1: 1, H2: 2, H3: 0
Paragraphs: 8
Links: 5
Div elements: 12
```

### Task 1.2: Element Extraction

Create functions to extract specific types of elements:

```python
def extract_headings(soup):
    """Extract all headings from the page."""
    # TODO: Find all h1, h2, h3, h4, h5, h6 elements
    # TODO: Return list of dictionaries with level and text

def extract_links(soup):
    """Extract all links from the page."""
    # TODO: Find all <a> elements
    # TODO: Return list of dictionaries with text and href

def extract_paragraphs(soup):
    """Extract all paragraphs from the page."""
    # TODO: Find all <p> elements
    # TODO: Return list of paragraph texts

# Test the functions
response = requests.get("https://httpbin.org/html")
soup = BeautifulSoup(response.text, 'html.parser')

headings = extract_headings(soup)
links = extract_links(soup)
paragraphs = extract_paragraphs(soup)

print(f"Found {len(headings)} headings")
print(f"Found {len(links)} links")
print(f"Found {len(paragraphs)} paragraphs")
```

---

## 🎯 Exercise 2: CSS Selector Practice

**Goal**: Master CSS selectors for precise element targeting

### Task 2.1: Selector Basics

Practice basic CSS selectors on the test page:

```python
def test_css_selectors(soup):
    """Test various CSS selectors."""
    # TODO: Test these selectors and print results:

    # Basic selectors
    # - "h1" (all h1 elements)
    # - ".h1" (elements with class h1)
    # - "#main" (element with id main)

    # Combinators
    # - "div p" (paragraphs inside divs)
    # - "div > p" (direct child paragraphs)
    # - "h1, h2" (both h1 and h2)

    # Attribute selectors
    # - "[href]" (elements with href attribute)
    # - "a[href*='http']" (links containing 'http')

    # Pseudo-classes (limited support in BeautifulSoup)
    # - "p:first-child" (first paragraph)

# Test selectors
response = requests.get("https://httpbin.org/html")
soup = BeautifulSoup(response.text, 'html.parser')
test_css_selectors(soup)
```

### Task 2.2: Advanced Selectors

Create functions using advanced CSS selectors:

```python
def extract_navigation_links(soup):
    """Extract navigation links using selectors."""
    # TODO: Find links that are likely navigation
    # TODO: Look for links in nav, header, or with specific classes

def extract_main_content(soup):
    """Extract main content using selectors."""
    # TODO: Find the main content area
    # TODO: Exclude navigation, footer, sidebar

def extract_data_tables(soup):
    """Extract data from tables."""
    # TODO: Find all tables
    # TODO: Extract headers and data rows
    # TODO: Return structured data

# Test advanced selectors
nav_links = extract_navigation_links(soup)
main_content = extract_main_content(soup)
tables = extract_data_tables(soup)

print(f"Navigation links: {len(nav_links)}")
print(f"Main content length: {len(main_content) if main_content else 0}")
print(f"Tables found: {len(tables)}")
```

### Task 2.3: Selector Comparison

Compare find() vs select() methods:

```python
import time

def compare_selectors(soup, iterations=100):
    """Compare performance of different selection methods."""
    # TODO: Time find() method
    # TODO: Time select() method
    # TODO: Compare for different types of selections
    # TODO: Print performance comparison

# Performance comparison
response = requests.get("https://httpbin.org/html")
soup = BeautifulSoup(response.text, 'html.parser')
compare_selectors(soup)
```

---

## 📊 Exercise 3: Data Extraction Functions

**Goal**: Build reusable functions for extracting structured data

### Task 3.1: Article Extraction

Create a function to extract article-like content:

```python
def extract_article_data(soup):
    """Extract article data from the page."""
    # TODO: Try to identify article content
    # TODO: Extract title, author, publication date, content
    # TODO: Return structured dictionary

def extract_metadata(soup):
    """Extract page metadata."""
    # TODO: Extract from <meta> tags
    # TODO: Look for Open Graph tags
    # TODO: Extract description, keywords, author

# Test data extraction
article = extract_article_data(soup)
metadata = extract_metadata(soup)

print("Article data:", article)
print("Metadata:", metadata)
```

### Task 3.2: List and Table Extraction

Extract structured data from lists and tables:

```python
def extract_lists(soup):
    """Extract all lists from the page."""
    # TODO: Find all <ul> and <ol> elements
    # TODO: Extract list items
    # TODO: Return structured data

def extract_tables(soup):
    """Extract data from all tables."""
    # TODO: Find all <table> elements
    # TODO: Extract headers and rows
    # TODO: Handle colspan/rowspan if present
    # TODO: Return list of table data

# Test list and table extraction
lists = extract_lists(soup)
tables = extract_tables(soup)

print(f"Lists found: {len(lists)}")
print(f"Tables found: {len(tables)}")

# Show sample data
if lists:
    print("Sample list:", lists[0])
if tables:
    print("Sample table:", tables[0])
```

### Task 3.3: Data Cleaning

Create functions to clean and normalize extracted data:

```python
def clean_text(text):
    """Clean and normalize text data."""
    # TODO: Remove extra whitespace
    # TODO: Handle HTML entities
    # TODO: Normalize unicode characters
    # TODO: Return cleaned text

def normalize_url(url, base_url):
    """Normalize relative URLs to absolute."""
    # TODO: Convert relative URLs to absolute
    # TODO: Handle different URL formats
    # TODO: Return normalized URL

def extract_numbers(text):
    """Extract numbers from text."""
    # TODO: Find all numbers in text
    # TODO: Handle different number formats
    # TODO: Return list of numbers

# Test data cleaning
sample_text = "  Price: $1,234.56 & more info  "
cleaned = clean_text(sample_text)
numbers = extract_numbers(sample_text)

print(f"Original: '{sample_text}'")
print(f"Cleaned: '{cleaned}'")
print(f"Numbers found: {numbers}")
```

---

## 🌐 Exercise 4: Complete Website Scraper

**Goal**: Build a complete scraper for a simple website

### Task 4.1: Scraper Class Design

Create a reusable scraper class:

```python
import requests
from bs4 import BeautifulSoup
import time
import logging

class WebScraper:
    """A reusable web scraper class."""

    def __init__(self, base_url="https://httpbin.org"):
        # TODO: Initialize scraper
        # - Set base URL
        # - Configure session with headers
        # - Set up logging
        # - Initialize rate limiting

    def get_page(self, path="/html"):
        """Get a page with error handling."""
        # TODO: Construct full URL
        # TODO: Make request with timeout
        # TODO: Handle errors appropriately
        # TODO: Return response or None

    def parse_html(self, html_content):
        """Parse HTML content."""
        # TODO: Parse with BeautifulSoup
        # TODO: Handle parsing errors
        # TODO: Return soup object

    def extract_quotes(self, soup):
        """Extract quotes from the page."""
        # TODO: Find all blockquote elements
        # TODO: Extract text content
        # TODO: Return list of quotes

    def scrape_quotes(self):
        """Complete quote scraping workflow."""
        # TODO: Get page
        # TODO: Parse HTML
        # TODO: Extract quotes
        # TODO: Handle errors
        # TODO: Return results

# Test the scraper class
scraper = WebScraper()
quotes = scraper.scrape_quotes()

print(f"Scraped {len(quotes)} quotes")
for i, quote in enumerate(quotes[:3], 1):
    print(f"{i}. {quote[:100]}...")
```

### Task 4.2: Data Export

Add data export capabilities:

```python
import json
import csv

class WebScraper:
    # ... (previous methods)

    def save_quotes_json(self, quotes, filename="quotes.json"):
        """Save quotes to JSON file."""
        # TODO: Format data for JSON
        # TODO: Save to file with proper encoding

    def save_quotes_csv(self, quotes, filename="quotes.csv"):
        """Save quotes to CSV file."""
        # TODO: Format data for CSV
        # TODO: Save to file

    def export_quotes(self, quotes, format="json"):
        """Export quotes in specified format."""
        # TODO: Route to appropriate export method
        # TODO: Add timestamp to filename

# Test data export
quotes = scraper.scrape_quotes()
if quotes:
    scraper.export_quotes(quotes, "json")
    scraper.export_quotes(quotes, "csv")
    print("Quotes exported successfully")
```

### Task 4.3: Configuration and Logging

Enhance the scraper with configuration and logging:

```python
class WebScraper:
    def __init__(self, base_url="https://httpbin.org", delay=1.0):
        # TODO: Add configuration options
        # - base_url
        # - request_timeout
        # - rate_limit_delay
        # - user_agent

        # TODO: Set up logging
        # - Create logger
        # - Configure log level
        # - Add console handler

        # TODO: Initialize session
        # - Create requests session
        # - Set headers
        # - Configure timeouts

    def _rate_limit(self):
        """Implement rate limiting."""
        # TODO: Add delay between requests
        # TODO: Track last request time

    def get_page(self, path="/html"):
        """Get page with rate limiting and logging."""
        # TODO: Implement rate limiting
        # TODO: Log request attempt
        # TODO: Make request with error handling
        # TODO: Log response status
        # TODO: Return response

# Test enhanced scraper
enhanced_scraper = WebScraper(delay=0.5)
quotes = enhanced_scraper.scrape_quotes()
print(f"Enhanced scraper found {len(quotes)} quotes")
```

---

## 🛡️ Exercise 5: Error Handling and Robustness

**Goal**: Make scrapers robust against various failure modes

### Task 5.1: Network Error Handling

Add comprehensive error handling:

```python
class RobustScraper(WebScraper):
    """Scraper with robust error handling."""

    def __init__(self, max_retries=3, backoff_factor=2, **kwargs):
        # TODO: Initialize with retry parameters
        super().__init__(**kwargs)

    def get_page_robust(self, path="/html"):
        """Get page with retry logic and comprehensive error handling."""
        # TODO: Implement retry loop
        # TODO: Handle different types of errors
        # - Timeout
        # - Connection errors
        # - HTTP errors
        # - SSL errors
        # TODO: Implement exponential backoff
        # TODO: Log retry attempts
        # TODO: Return response or raise final error

    def parse_html_robust(self, html_content):
        """Parse HTML with fallback parsers."""
        # TODO: Try different parsers: html.parser, lxml, html5lib
        # TODO: Log parsing attempts and failures
        # TODO: Return soup object or None

# Test robust scraper
robust_scraper = RobustScraper(max_retries=2)
try:
    quotes = robust_scraper.scrape_quotes()
    print(f"Robust scraper found {len(quotes)} quotes")
except Exception as e:
    print(f"Scraping failed: {e}")
```

### Task 5.2: Data Validation

Add data validation to extracted content:

```python
class ValidatingScraper(RobustScraper):
    """Scraper with data validation."""

    def validate_quote(self, quote_text):
        """Validate extracted quote."""
        # TODO: Check if quote is not empty
        # TODO: Check minimum length
        # TODO: Check for valid characters
        # TODO: Return True/False

    def clean_quote(self, quote_text):
        """Clean and normalize quote text."""
        # TODO: Remove extra whitespace
        # TODO: Fix encoding issues
        # TODO: Remove unwanted characters
        # TODO: Return cleaned text

    def extract_quotes_validated(self, soup):
        """Extract quotes with validation and cleaning."""
        # TODO: Extract raw quotes
        # TODO: Validate each quote
        # TODO: Clean valid quotes
        # TODO: Log validation results
        # TODO: Return cleaned quotes

# Test validating scraper
validating_scraper = ValidatingScraper()
quotes = validating_scraper.scrape_quotes()

valid_quotes = [q for q in quotes if validating_scraper.validate_quote(q)]
print(f"Found {len(valid_quotes)} valid quotes out of {len(quotes)} total")
```

### Task 5.3: Monitoring and Statistics

Add monitoring capabilities:

```python
class MonitoredScraper(ValidatingScraper):
    """Scraper with monitoring and statistics."""

    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        # TODO: Initialize statistics
        # - requests_made
        # - successful_requests
        # - failed_requests
        # - bytes_downloaded
        # - parsing_errors
        # - start_time

    def update_stats(self, success=True, bytes_downloaded=0, parsing_errors=0):
        """Update scraping statistics."""
        # TODO: Update counters
        # TODO: Track timing information

    def get_stats(self):
        """Get current statistics."""
        # TODO: Calculate rates and percentages
        # TODO: Return comprehensive stats dictionary

    def log_stats(self):
        """Log current statistics."""
        # TODO: Log statistics at appropriate level

    def reset_stats(self):
        """Reset all statistics."""
        # TODO: Reset counters and timing

# Test monitored scraper
monitored_scraper = MonitoredScraper()
quotes = monitored_scraper.scrape_quotes()

stats = monitored_scraper.get_stats()
print("Scraping Statistics:")
for key, value in stats.items():
    print(f"  {key}: {value}")
```

---

## 🏆 Challenge Exercises

### Challenge 1: Multi-Page Scraper

Create a scraper that follows links and scrapes multiple pages:

```python
class MultiPageScraper(MonitoredScraper):
    """Scraper that can follow links and scrape multiple pages."""

    def __init__(self, max_pages=5, **kwargs):
        # TODO: Initialize with page limits
        super().__init__(**kwargs)
        self.visited_urls = set()
        self.max_pages = max_pages

    def should_visit_url(self, url):
        """Check if URL should be visited."""
        # TODO: Check if URL is valid
        # TODO: Check if already visited
        # TODO: Check page limit
        # TODO: Check if same domain
        pass

    def extract_links(self, soup, base_url):
        """Extract links from page."""
        # TODO: Find all <a> tags
        # TODO: Convert relative to absolute URLs
        # TODO: Filter valid links
        pass

    def scrape_recursive(self, start_url):
        """Recursively scrape pages following links."""
        # TODO: Implement breadth-first or depth-first scraping
        # TODO: Respect robots.txt
        # TODO: Implement rate limiting
        # TODO: Track visited pages
        pass

# TODO: Implement multi-page scraping logic
```

### Challenge 2: Content Type Detection

Add content type detection and appropriate parsing:

```python
class ContentAwareScraper(MonitoredScraper):
    """Scraper that adapts to different content types."""

    def detect_content_type(self, response):
        """Detect content type of response."""
        # TODO: Check Content-Type header
        # TODO: Analyze content for clues
        # TODO: Return content type
        pass

    def parse_json_content(self, response):
        """Parse JSON API responses."""
        # TODO: Parse JSON data
        # TODO: Extract relevant information
        pass

    def parse_xml_content(self, response):
        """Parse XML content."""
        # TODO: Parse XML data
        # TODO: Extract information
        pass

    def scrape_adaptive(self, url):
        """Scrape content adapting to its type."""
        # TODO: Make request
        # TODO: Detect content type
        # TODO: Use appropriate parsing method
        # TODO: Return structured data
        pass

# TODO: Implement content-type aware scraping
```

### Challenge 3: Caching System

Implement response caching to avoid redundant requests:

```python
import hashlib
import os
from pathlib import Path

class CachingScraper(MonitoredScraper):
    """Scraper with response caching."""

    def __init__(self, cache_dir="cache", cache_ttl=3600, **kwargs):
        # TODO: Initialize cache parameters
        super().__init__(**kwargs)
        self.cache_dir = Path(cache_dir)
        self.cache_ttl = cache_ttl
        self.cache_dir.mkdir(exist_ok=True)

    def _get_cache_key(self, url):
        """Generate cache key from URL."""
        # TODO: Create hash of URL
        pass

    def _is_cache_valid(self, cache_file):
        """Check if cache file is still valid."""
        # TODO: Check file modification time
        # TODO: Compare with TTL
        pass

    def get_page_cached(self, url):
        """Get page with caching."""
        # TODO: Check cache first
        # TODO: Return cached content if valid
        # TODO: Make request if not cached or expired
        # TODO: Save to cache
        pass

    def clear_cache(self):
        """Clear all cached responses."""
        # TODO: Remove all cache files
        pass

# TODO: Implement response caching
```

---

## ✅ Solution Code

### WebScraper Base Class

```python
import requests
from bs4 import BeautifulSoup
import time
import logging
import json
import csv
from typing import List, Dict, Optional
from urllib.parse import urljoin, urlparse

class WebScraper:
    """A reusable web scraper with comprehensive features."""

    def __init__(self, base_url="https://httpbin.org", request_delay=1.0):
        self.base_url = base_url.rstrip('/')
        self.request_delay = request_delay
        self.last_request_time = 0

        # Set up logging
        self.logger = logging.getLogger(f"{__name__}.{self.__class__.__name__}")
        if not self.logger.handlers:
            handler = logging.StreamHandler()
            formatter = logging.Formatter(
                '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
            )
            handler.setFormatter(formatter)
            self.logger.addHandler(handler)
            self.logger.setLevel(logging.INFO)

        # Set up session
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'WebScraperWorkshop/1.0 (educational@example.com)',
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
            'Accept-Language': 'en-US,en;q=0.9,en;q=0.8',
        })

    def _rate_limit(self):
        """Implement rate limiting between requests."""
        current_time = time.time()
        time_since_last = current_time - self.last_request_time

        if time_since_last < self.request_delay:
            sleep_time = self.request_delay - time_since_last
            self.logger.debug(f"Rate limiting: sleeping {sleep_time:.2f}s")
            time.sleep(sleep_time)

        self.last_request_time = time.time()

    def get_page(self, path="/html", timeout=10):
        """Get a page with error handling and rate limiting."""
        self._rate_limit()

        url = f"{self.base_url}{path}"
        self.logger.info(f"Fetching: {url}")

        try:
            response = self.session.get(url, timeout=timeout)
            response.raise_for_status()

            self.logger.info(f"Successfully fetched {len(response.text)} bytes")
            return response

        except requests.exceptions.Timeout:
            self.logger.error(f"Timeout fetching {url}")
        except requests.exceptions.ConnectionError:
            self.logger.error(f"Connection error for {url}")
        except requests.exceptions.HTTPError as e:
            self.logger.error(f"HTTP error {e.response.status_code} for {url}")
        except requests.exceptions.RequestException as e:
            self.logger.error(f"Request error for {url}: {e}")

        return None

    def parse_html(self, html_content):
        """Parse HTML content with error handling."""
        try:
            soup = BeautifulSoup(html_content, 'html.parser')
            self.logger.debug("HTML parsed successfully")
            return soup
        except Exception as e:
            self.logger.error(f"HTML parsing error: {e}")
            return None

    def extract_quotes(self, soup):
        """Extract quotes from BeautifulSoup object."""
        if not soup:
            return []

        quotes = soup.find_all('blockquote')
        extracted_quotes = []

        for quote in quotes:
            text = quote.text.strip()
            if text:
                extracted_quotes.append(text)

        self.logger.info(f"Extracted {len(extracted_quotes)} quotes")
        return extracted_quotes

    def scrape_quotes(self):
        """Complete quote scraping workflow."""
        response = self.get_page("/html")
        if not response:
            return []

        soup = self.parse_html(response.text)
        if not soup:
            return []

        quotes = self.extract_quotes(soup)
        return quotes

    def save_quotes_json(self, quotes, filename="quotes.json"):
        """Save quotes to JSON file."""
        data = {
            'quotes': quotes,
            'count': len(quotes),
            'scraped_at': time.time(),
            'source': self.base_url
        }

        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(data, f, indent=2, ensure_ascii=False)

        self.logger.info(f"Saved {len(quotes)} quotes to {filename}")

    def save_quotes_csv(self, quotes, filename="quotes.csv"):
        """Save quotes to CSV file."""
        with open(filename, 'w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(['Quote'])

            for quote in quotes:
                writer.writerow([quote])

        self.logger.info(f"Saved {len(quotes)} quotes to {filename}")

def main():
    """Test the scraper."""
    scraper = WebScraper()

    # Scrape quotes
    quotes = scraper.scrape_quotes()

    if quotes:
        print(f"Successfully scraped {len(quotes)} quotes!")

        # Save in different formats
        scraper.save_quotes_json(quotes)
        scraper.save_quotes_csv(quotes)

        # Show sample quotes
        for i, quote in enumerate(quotes[:3], 1):
            print(f"{i}. {quote[:100]}...")
    else:
        print("Failed to scrape quotes")

if __name__ == "__main__":
    main()
```

### Additional Utility Functions

```python
# data_extractor.py
"""Data extraction utilities for web scraping."""

from bs4 import BeautifulSoup
from typing import List, Dict, Any, Optional
import re

def extract_headings(soup: BeautifulSoup) -> List[Dict[str, str]]:
    """Extract all headings from soup."""
    headings = []
    for level in range(1, 7):
        for heading in soup.find_all(f'h{level}'):
            headings.append({
                'level': level,
                'text': heading.text.strip(),
                'id': heading.get('id', '')
            })
    return headings

def extract_links(soup: BeautifulSoup, base_url: str = "") -> List[Dict[str, str]]:
    """Extract all links from soup."""
    links = []
    for link in soup.find_all('a', href=True):
        href = link['href']
        if base_url and not href.startswith(('http://', 'https://')):
            href = urljoin(base_url, href)

        links.append({
            'text': link.text.strip(),
            'url': href,
            'title': link.get('title', '')
        })
    return links

def extract_paragraphs(soup: BeautifulSoup) -> List[str]:
    """Extract all paragraph texts."""
    return [p.text.strip() for p in soup.find_all('p') if p.text.strip()]

def extract_tables(soup: BeautifulSoup) -> List[List[List[str]]]:
    """Extract data from all tables."""
    tables = []

    for table in soup.find_all('table'):
        table_data = []

        # Extract headers
        headers = []
        header_row = table.find('thead')
        if header_row:
            headers = [th.text.strip() for th in header_row.find_all('th')]

        if not headers:
            # Try first row as headers
            first_row = table.find('tr')
            if first_row:
                headers = [td.text.strip() for td in first_row.find_all(['td', 'th'])]

        table_data.append(headers)

        # Extract data rows
        rows = table.find_all('tr')[1:] if headers else table.find_all('tr')
        for row in rows:
            cells = [cell.text.strip() for cell in row.find_all(['td', 'th'])]
            if cells:  # Skip empty rows
                table_data.append(cells)

        if len(table_data) > 1:  # Has data beyond headers
            tables.append(table_data)

    return tables

def clean_text(text: str) -> str:
    """Clean and normalize text."""
    if not text:
        return ""

    # Remove extra whitespace
    text = re.sub(r'\s+', ' ', text.strip())

    # Handle common HTML entities
    entities = {
        '&nbsp;': ' ',
        '&amp;': '&',
        '&lt;': '<',
        '&gt;': '>',
        '&quot;': '"',
        '&#39;': "'"
    }

    for entity, replacement in entities.items():
        text = text.replace(entity, replacement)

    return text

def extract_numbers(text: str) -> List[float]:
    """Extract all numbers from text."""
    # Find all number patterns (including decimals, commas)
    pattern = r'-?\d+(?:,\d{3})*(?:\.\d+)?'
    matches = re.findall(pattern, text)

    numbers = []
    for match in matches:
        # Remove commas from formatted numbers
        clean_match = match.replace(',', '')
        try:
            numbers.append(float(clean_match))
        except ValueError:
            continue

    return numbers

def validate_url(url: str) -> bool:
    """Basic URL validation."""
    if not url or not isinstance(url, str):
        return False

    url = url.strip()
    if not url:
        return False

    # Basic pattern check
    pattern = r'^https?://[^\s/$.?#].[^\s]*$'
    return bool(re.match(pattern, url, re.IGNORECASE))
```

---

## 📝 Key Takeaways

1. **BeautifulSoup**: Use `find()`, `find_all()`, and `select()` for element extraction
2. **CSS Selectors**: Leverage modern selectors for precise element targeting
3. **Error Handling**: Always handle network errors, timeouts, and HTTP errors
4. **Rate Limiting**: Implement delays between requests to respect servers
5. **Data Cleaning**: Clean and validate extracted data before storage
6. **Logging**: Use logging throughout scraping for debugging and monitoring
7. **Session Management**: Reuse HTTP sessions for better performance
8. **Structured Data**: Convert scraped data to dictionaries and lists for easy processing
9. **File Operations**: Save data in JSON, CSV, and other formats
10. **Robust Parsing**: Handle malformed HTML and missing elements gracefully

---

**Workshop Version**: 1.0
**Last Updated**: February 2026
**Estimated Completion Time**: 90 minutes
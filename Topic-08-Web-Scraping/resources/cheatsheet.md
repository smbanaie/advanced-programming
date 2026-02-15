# Web Scraping Cheatsheet

**Topic**: 8 - Web Scraping Fundamentals
**Level**: Intermediate to Advanced
**Last Updated**: February 2026

---

## 📋 Table of Contents

1. [HTTP Requests](#http-requests)
2. [BeautifulSoup Selectors](#beautifulsoup-selectors)
3. [CSS Selectors](#css-selectors)
4. [Error Handling](#error-handling)
5. [Rate Limiting](#rate-limiting)
6. [Data Storage](#data-storage)
7. [Legal & Ethical](#legal--ethical)
8. [Common Patterns](#common-patterns)
9. [Debugging](#debugging)
10. [Performance](#performance)

---

## 🌐 HTTP Requests

### Basic Requests

```python
import requests

# GET request
response = requests.get('https://api.example.com/data')

# POST request with data
data = {'key': 'value'}
response = requests.post('https://api.example.com/submit', data=data)

# JSON POST
response = requests.post('https://api.example.com/submit',
                        json={'key': 'value'})

# With headers
headers = {'Authorization': 'Bearer token123'}
response = requests.get('https://api.example.com/data', headers=headers)
```

### Session Management

```python
# Persistent session
session = requests.Session()
session.headers.update({'User-Agent': 'MyScraper/1.0'})

# Login and maintain session
login_data = {'username': 'user', 'password': 'pass'}
session.post('https://example.com/login', data=login_data)

# Subsequent requests maintain cookies
response = session.get('https://example.com/dashboard')
```

### Request Configuration

```python
# Timeout and retries
response = requests.get('https://example.com',
                       timeout=10,
                       allow_redirects=True)

# Custom User-Agent
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
}

# Proxies
proxies = {
    'http': 'http://proxy.example.com:8080',
    'https': 'https://proxy.example.com:8080'
}
response = requests.get('https://example.com', proxies=proxies)
```

---

## 🍲 BeautifulSoup Selectors

### Basic Selection

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html_content, 'html.parser')

# Find single element
title = soup.find('title')
heading = soup.find('h1', class_='main-title')

# Find multiple elements
paragraphs = soup.find_all('p')
links = soup.find_all('a', href=True)
items = soup.find_all('div', class_='item')

# Get text content
title_text = soup.title.text
title_text = soup.title.get_text()  # Same result
clean_text = soup.title.get_text(strip=True)

# Get attributes
href = link['href']  # Direct access
href = link.get('href')  # Safe access
classes = div.get('class', [])
```

### Advanced Selection

```python
# Find by attribute patterns
external_links = soup.find_all('a', href=lambda x: x and x.startswith('http'))

# Find by text content
important_p = soup.find_all('p', text=lambda t: t and 'important' in t.lower())

# Custom filters
def has_data_attr(tag):
    return tag.has_attr('data-product-id')

products = soup.find_all(has_data_attr)

# Limit results
first_5_links = soup.find_all('a', limit=5)

# Recursive search control
# Only immediate children
children = soup.div.find_all(recursive=False)
```

### Navigation

```python
# Parent relationships
parent = element.parent
grandparent = element.parent.parent

# Child relationships
children = element.children  # Generator
first_child = element.contents[0]  # List

# Sibling relationships
next_sib = element.next_sibling
prev_sib = element.previous_sibling
all_sibs = element.next_siblings

# Tree traversal
# All descendants
descendants = element.descendants

# Find next/previous elements
next_p = element.find_next('p')
prev_h2 = element.find_previous('h2')
```

---

## 🎯 CSS Selectors

### Basic Selectors

```python
# Element selectors
soup.select('div')           # All div elements
soup.select('p')             # All paragraphs

# Class selectors
soup.select('.container')    # Class = container
soup.select('.item.active')  # Multiple classes

# ID selectors
soup.select('#main')         # ID = main

# Universal selector
soup.select('*')             # All elements
```

### Combinators

```python
# Descendant combinator
soup.select('div p')         # p elements inside div
soup.select('ul li a')       # links inside list items

# Child combinator
soup.select('div > p')       # Direct child paragraphs
soup.select('ul > li')       # Direct list items

# Adjacent sibling
soup.select('h1 + p')        # p immediately after h1

# General sibling
soup.select('h1 ~ p')        # All p after h1 (same parent)
```

### Attribute Selectors

```python
# Has attribute
soup.select('[href]')        # Elements with href

# Exact match
soup.select('[class="button"]')  # Exact class match

# Contains word
soup.select('[class~="primary"]')  # Class contains "primary"

# Starts with
soup.select('[href^="http"]')  # href starts with http

# Ends with
soup.select('[href$=".pdf"]')  # href ends with .pdf

# Contains substring
soup.select('[title*="click"]')  # title contains "click"
```

### Pseudo-Selectors

```python
# First/last child
soup.select('li:first-child')  # First list item
soup.select('li:last-child')   # Last list item

# Nth child
soup.select('li:nth-child(2)')  # Second list item
soup.select('tr:nth-child(odd)')  # Odd table rows

# Not selector
soup.select('a:not([href^="http"])')  # Internal links only
```

---

## ⚠️ Error Handling

### Network Errors

```python
try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()  # Check HTTP status

    data = response.json()
    print("Success:", data)

except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection error")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e.response.status_code}")
except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")
```

### Parsing Errors

```python
def safe_parse_html(html_content):
    """Parse HTML with fallback parsers."""
    parsers = ['html.parser', 'lxml', 'html5lib']

    for parser in parsers:
        try:
            soup = BeautifulSoup(html_content, parser)
            if soup.title:  # Basic validation
                return soup
        except Exception:
            continue

    # Fallback to basic parser
    return BeautifulSoup(html_content, 'html.parser')

# Usage
soup = safe_parse_html(response.text)
if not soup:
    print("Failed to parse HTML")
```

### Data Validation

```python
def validate_scraped_data(data):
    """Validate scraped data structure."""
    required_fields = ['title', 'url', 'content']

    if not isinstance(data, dict):
        return False, "Data must be dictionary"

    for field in required_fields:
        if field not in data:
            return False, f"Missing required field: {field}"

        if not data[field] or not data[field].strip():
            return False, f"Empty field: {field}"

    return True, "Valid data"

# Usage
is_valid, message = validate_scraped_data(item)
if not is_valid:
    print(f"Invalid data: {message}")
```

---

## ⏱️ Rate Limiting

### Simple Rate Limiting

```python
import time

class RateLimiter:
    """Simple rate limiter."""

    def __init__(self, requests_per_minute=30):
        self.requests_per_minute = requests_per_minute
        self.min_delay = 60 / requests_per_minute
        self.last_request = 0

    def wait_if_needed(self):
        """Wait if request would violate rate limit."""
        current_time = time.time()
        time_since_last = current_time - self.last_request

        if time_since_last < self.min_delay:
            sleep_time = self.min_delay - time_since_last
            time.sleep(sleep_time)

        self.last_request = time.time()

# Usage
limiter = RateLimiter(requests_per_minute=20)

def rate_limited_request(url):
    limiter.wait_if_needed()
    return requests.get(url)
```

### Exponential Backoff

```python
import random

def retry_with_backoff(func, max_retries=3, base_delay=1):
    """Retry function with exponential backoff."""
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            if attempt == max_retries - 1:
                raise e

            delay = base_delay * (2 ** attempt) + random.uniform(0, 1)
            print(f"Attempt {attempt + 1} failed, retrying in {delay:.1f}s")
            time.sleep(delay)

# Usage
def fetch_data():
    response = requests.get('https://api.example.com/data')
    response.raise_for_status()
    return response.json()

data = retry_with_backoff(fetch_data)
```

### Adaptive Rate Limiting

```python
class AdaptiveRateLimiter:
    """Adaptive rate limiter based on response times."""

    def __init__(self, min_delay=1.0, max_delay=60.0):
        self.min_delay = min_delay
        self.max_delay = max_delay
        self.current_delay = min_delay
        self.last_request = 0
        self.response_times = []

    def update_delay(self, response_time):
        """Update delay based on response time."""
        self.response_times.append(response_time)

        # Keep last 10 response times
        if len(self.response_times) > 10:
            self.response_times.pop(0)

        avg_response_time = sum(self.response_times) / len(self.response_times)

        # Increase delay if responses are slow
        if avg_response_time > 2.0:
            self.current_delay = min(self.current_delay * 1.5, self.max_delay)
        # Decrease delay if responses are fast
        elif avg_response_time < 0.5 and self.current_delay > self.min_delay:
            self.current_delay = max(self.current_delay * 0.8, self.min_delay)

    def wait_if_needed(self):
        """Wait with adaptive delay."""
        current_time = time.time()
        time_since_last = current_time - self.last_request

        if time_since_last < self.current_delay:
            sleep_time = self.current_delay - time_since_last
            time.sleep(sleep_time)

        self.last_request = time.time()
```

---

## 💾 Data Storage

### JSON Storage

```python
import json
from datetime import datetime

def save_to_json(data, filename):
    """Save data to JSON with metadata."""
    output = {
        'data': data,
        'metadata': {
            'saved_at': datetime.now().isoformat(),
            'count': len(data) if isinstance(data, list) else 1,
            'source': 'web_scraper'
        }
    }

    with open(filename, 'w', encoding='utf-8') as f:
        json.dump(output, f, indent=2, ensure_ascii=False)

# Usage
scraped_data = [{'title': 'Item 1', 'url': 'http://example.com/1'}]
save_to_json(scraped_data, 'data.json')
```

### CSV Storage

```python
import csv

def save_to_csv(data, filename, fieldnames=None):
    """Save data to CSV format."""
    if not data:
        return

    if fieldnames is None:
        fieldnames = data[0].keys()

    with open(filename, 'w', newline='', encoding='utf-8') as f:
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(data)

# Usage
products = [
    {'name': 'Laptop', 'price': 999.99, 'category': 'Electronics'},
    {'name': 'Book', 'price': 19.99, 'category': 'Education'}
]
save_to_csv(products, 'products.csv')
```

### Database Storage

```python
import sqlite3
from contextlib import contextmanager

@contextmanager
def get_db_connection(db_file):
    """Context manager for database connections."""
    conn = sqlite3.connect(db_file)
    try:
        yield conn
    finally:
        conn.close()

def create_products_table(conn):
    """Create products table."""
    conn.execute('''
        CREATE TABLE IF NOT EXISTS products (
            id INTEGER PRIMARY KEY,
            name TEXT NOT NULL,
            price REAL,
            category TEXT,
            scraped_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')

def save_to_database(data, db_file='scraped_data.db'):
    """Save data to SQLite database."""
    with get_db_connection(db_file) as conn:
        create_products_table(conn)

        conn.executemany('''
            INSERT INTO products (name, price, category)
            VALUES (?, ?, ?)
        ''', [(item['name'], item['price'], item['category']) for item in data])

        conn.commit()

# Usage
save_to_database(products)
```

---

## ⚖️ Legal & Ethical

### Robots.txt Checking

```python
from urllib.robotparser import RobotFileParser

def can_scrape(url, user_agent='*'):
    """Check if scraping is allowed by robots.txt."""
    from urllib.parse import urlparse

    parsed = urlparse(url)
    robots_url = f"{parsed.scheme}://{parsed.netloc}/robots.txt"

    rp = RobotFileParser()
    rp.set_url(robots_url)
    rp.read()

    return rp.can_fetch(user_agent, parsed.path)

# Usage
if can_scrape('https://example.com/products'):
    print("Scraping allowed")
else:
    print("Scraping not allowed by robots.txt")
```

### Respectful Headers

```python
# Always identify yourself
headers = {
    'User-Agent': 'MyResearchBot/1.0 (https://example.com/bot-info)',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'en-US,en;q=0.9,en;q=0.8',
    'Accept-Encoding': 'gzip, deflate, br',
    'DNT': '1',  # Do Not Track
    'Connection': 'keep-alive',
    'Referer': 'https://example.com/',  # Appropriate referer
}

response = requests.get('https://example.com/page', headers=headers)
```

### Terms of Service Checking

```python
def check_tos_compliance(url):
    """Check basic terms of service compliance."""
    # Check robots.txt
    if not can_scrape(url):
        return False, "Blocked by robots.txt"

    # Check for common blocking indicators
    response = requests.get(url, headers={
        'User-Agent': 'Mozilla/5.0 (compatible; ResearchBot/1.0)'
    })

    if 'captcha' in response.text.lower():
        return False, "CAPTCHA detected"

    if response.status_code == 429:
        return False, "Rate limited"

    return True, "Appears compliant"
```

---

## 🔄 Common Patterns

### Article Scraping

```python
def scrape_article(soup):
    """Extract article data."""
    article = {}

    # Title
    title_elem = soup.select_one('h1, .article-title, .post-title')
    article['title'] = title_elem.text.strip() if title_elem else None

    # Author
    author_elem = soup.select_one('.author, [rel="author"], .byline')
    article['author'] = author_elem.text.strip() if author_elem else None

    # Publish date
    date_elem = soup.select_one('time, .published, .date')
    article['date'] = date_elem.get('datetime') or date_elem.text.strip() if date_elem else None

    # Content
    content_elem = soup.select_one('.content, .article-body, .post-content')
    article['content'] = content_elem.get_text(separator='\n', strip=True) if content_elem else None

    return article
```

### Product Scraping

```python
def scrape_product(soup):
    """Extract product data."""
    product = {}

    # Basic info
    product['name'] = soup.select_one('.product-name, h1').text.strip()
    product['price'] = soup.select_one('.price, .product-price').text.strip()
    product['sku'] = soup.select_one('[data-sku], .sku').get('data-sku')

    # Images
    images = soup.select('.product-image img')
    product['images'] = [img['src'] for img in images if img.get('src')]

    # Specifications
    specs = {}
    spec_rows = soup.select('.specs tr, .specifications tr')
    for row in spec_rows:
        key = row.select_one('th, td:first-child').text.strip()
        value = row.select_one('td:last-child').text.strip()
        specs[key] = value
    product['specifications'] = specs

    # Reviews
    reviews = soup.select('.review, .review-item')
    product['reviews'] = [review.text.strip() for review in reviews]

    return product
```

### Table Scraping

```python
def scrape_table(soup, table_selector='table'):
    """Extract data from HTML tables."""
    tables = []

    for table in soup.select(table_selector):
        table_data = []

        # Get headers
        headers = []
        header_row = table.select_one('thead tr')
        if header_row:
            headers = [th.text.strip() for th in header_row.select('th, td')]
        else:
            # Use first row as headers
            first_row = table.select_one('tr')
            if first_row:
                headers = [cell.text.strip() for cell in first_row.select('td, th')]

        # Get data rows
        rows = table.select('tbody tr') if table.select_one('tbody') else table.select('tr')[1:]

        for row in rows:
            cells = row.select('td, th')
            if len(cells) == len(headers):
                row_data = {headers[i]: cell.text.strip() for i, cell in enumerate(cells)}
                table_data.append(row_data)

        tables.append({
            'headers': headers,
            'data': table_data,
            'row_count': len(table_data)
        })

    return tables
```

### Pagination Handling

```python
def get_paginated_urls(base_url, max_pages=10):
    """Generate paginated URLs."""
    urls = [base_url]

    for page in range(2, max_pages + 1):
        # Try different pagination patterns
        patterns = [
            f"{base_url}/page/{page}",
            f"{base_url}?page={page}",
            f"{base_url}&page={page}",
            f"{base_url}/p{page}",
        ]

        for url in patterns:
            # Test if URL exists (basic check)
            try:
                response = requests.head(url, timeout=5)
                if response.status_code == 200:
                    urls.append(url)
                    break
            except:
                continue

    return urls
```

---

## 🐛 Debugging

### Logging Setup

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('scraper.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

# Usage in scraper
logger.info(f"Starting scrape of {url}")
logger.debug(f"Found {len(items)} items")
logger.error(f"Failed to parse {url}: {error}")
```

### Debug Selectors

```python
def debug_selectors(soup, selectors):
    """Test and debug CSS selectors."""
    for selector in selectors:
        elements = soup.select(selector)
        print(f"{selector}: {len(elements)} matches")
        if elements:
            print(f"  Sample: {elements[0].text[:50]}...")

# Usage
selectors_to_test = [
    'h1', '.title', '#main-title',
    'article h2', '.content p', 'table tr'
]
debug_selectors(soup, selectors_to_test)
```

### Response Inspection

```python
def inspect_response(response):
    """Inspect HTTP response for debugging."""
    print(f"URL: {response.url}")
    print(f"Status: {response.status_code}")
    print(f"Headers: {dict(response.headers)}")
    print(f"Content-Type: {response.headers.get('content-type', 'Unknown')}")
    print(f"Content-Length: {len(response.text)}")

    # Check for common issues
    if 'text/html' not in response.headers.get('content-type', ''):
        print("⚠️  Warning: Not HTML content")

    if response.status_code >= 400:
        print(f"⚠️  Warning: HTTP error {response.status_code}")

    if len(response.text) < 100:
        print("⚠️  Warning: Very short response")

    # Show start of content
    print(f"Content preview: {response.text[:200]}...")

# Usage
inspect_response(response)
```

---

## ⚡ Performance

### Concurrent Scraping

```python
import concurrent.futures
import threading

def scrape_urls_concurrent(urls, max_workers=5):
    """Scrape multiple URLs concurrently."""
    results = {}

    def scrape_single(url):
        try:
            return url, scrape_page(url)
        except Exception as e:
            logger.error(f"Failed to scrape {url}: {e}")
            return url, None

    with concurrent.futures.ThreadPoolExecutor(max_workers=max_workers) as executor:
        future_to_url = {executor.submit(scrape_single, url): url for url in urls}

        for future in concurrent.futures.as_completed(future_to_url):
            url, result = future.result()
            results[url] = result

    return results

# Usage
urls = ['https://example.com/page1', 'https://example.com/page2']
results = scrape_urls_concurrent(urls, max_workers=3)
```

### Caching

```python
import hashlib
import os
from pathlib import Path

class ResponseCache:
    """Simple response cache."""

    def __init__(self, cache_dir='cache', ttl_hours=24):
        self.cache_dir = Path(cache_dir)
        self.cache_dir.mkdir(exist_ok=True)
        self.ttl_seconds = ttl_hours * 3600

    def _get_cache_path(self, url):
        """Get cache file path for URL."""
        url_hash = hashlib.md5(url.encode()).hexdigest()
        return self.cache_dir / f"{url_hash}.html"

    def is_cached(self, url):
        """Check if URL is cached and valid."""
        cache_path = self._get_cache_path(url)
        if not cache_path.exists():
            return False

        # Check TTL
        mtime = cache_path.stat().st_mtime
        age_seconds = time.time() - mtime

        return age_seconds < self.ttl_seconds

    def get_cached(self, url):
        """Get cached content."""
        cache_path = self._get_cache_path(url)
        with open(cache_path, 'r', encoding='utf-8') as f:
            return f.read()

    def cache_response(self, url, content):
        """Cache response content."""
        cache_path = self._get_cache_path(url)
        with open(cache_path, 'w', encoding='utf-8') as f:
            f.write(content)

# Usage
cache = ResponseCache()

def cached_get(url):
    if cache.is_cached(url):
        print(f"Using cached content for {url}")
        return cache.get_cached(url)

    response = requests.get(url)
    cache.cache_response(url, response.text)
    return response.text
```

### Memory Optimization

```python
def process_large_dataset_in_batches(data, batch_size=100):
    """Process large datasets in batches."""
    for i in range(0, len(data), batch_size):
        batch = data[i:i + batch_size]
        process_batch(batch)

        # Force garbage collection
        if i % 1000 == 0:
            import gc
            gc.collect()

def stream_large_html(html_content):
    """Process large HTML without loading entire DOM."""
    # For very large pages, consider streaming parsers
    # or processing in chunks

    chunk_size = 1024 * 1024  # 1MB chunks
    soup = BeautifulSoup('', 'html.parser')

    for i in range(0, len(html_content), chunk_size):
        chunk = html_content[i:i + chunk_size]
        soup.append(BeautifulSoup(chunk, 'html.parser'))

    return soup
```

---

## 📚 Additional Resources

- **BeautifulSoup**: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- **Requests**: https://requests.readthedocs.io/
- **CSS Selectors**: https://www.w3schools.com/cssref/css_selectors.asp
- **Web Scraping Ethics**: https://blog.apify.com/web-scraping-ethics/
- **robots.txt**: https://www.robotstxt.org/robotstxt.html

---

**Cheatsheet Version**: 1.0
**Last Updated**: February 2026
**Pages**: 12
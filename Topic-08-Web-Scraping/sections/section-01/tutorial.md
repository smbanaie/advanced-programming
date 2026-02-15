# Tutorial 1: Web Scraping Basics

**Section**: 1 - Web Scraping Basics (90 min)
**Level**: Intermediate
**Prerequisites**: Topics 1-7 (Basic Python, APIs, Logging)

---

## 📋 Learning Objectives

By the end of this tutorial, you will be able to:

1. **Understand HTML Structure**: Navigate DOM elements and CSS selectors
2. **Make HTTP Requests**: Use requests library with proper headers and error handling
3. **Parse HTML Content**: Extract data using BeautifulSoup and CSS selectors
4. **Handle Ethical Scraping**: Implement rate limiting and respect robots.txt
5. **Store Scraped Data**: Save extracted data in structured formats
6. **Debug Scraping Issues**: Troubleshoot common scraping problems
7. **Build Basic Scrapers**: Create simple, functional web scraping scripts

---

## 📚 Table of Contents

1. [What is Web Scraping?](#what-is-web-scraping)
2. [HTML Structure and CSS Selectors](#html-structure-and-css-selectors)
3. [Making HTTP Requests](#making-http-requests)
4. [BeautifulSoup for HTML Parsing](#beautifulsoup-for-html-parsing)
5. [Data Extraction Techniques](#data-extraction-techniques)
6. [Ethical Scraping and Legal Considerations](#ethical-scraping-and-legal-considerations)
7. [Error Handling and Debugging](#error-handling-and-debugging)
8. [Data Storage and Export](#data-storage-and-export)
9. [Building Your First Scraper](#building-your-first-scraper)
10. [Best Practices](#best-practices)

---

## 🤖 What is Web Scraping?

Web scraping is the process of automatically extracting data from websites. It involves:

1. **Making HTTP requests** to web servers
2. **Parsing HTML content** to extract structured data
3. **Storing the extracted data** in usable formats
4. **Respecting website policies** and legal boundaries

### Why Web Scraping?
- **Data Collection**: Gather information from websites at scale
- **Research**: Academic and market research data gathering
- **Monitoring**: Track changes on websites over time
- **Integration**: Pull data from websites into applications
- **Automation**: Replace manual data copying with automated scripts

### Web Scraping vs APIs
- **APIs**: Structured, official data access (preferred when available)
- **Scraping**: Extract data from HTML when APIs don't exist
- **Hybrid**: Some sites offer both APIs and HTML interfaces

---

## 🌐 HTML Structure and CSS Selectors

### Understanding HTML Structure

HTML documents have a tree-like structure called the DOM (Document Object Model):

```html
<!DOCTYPE html>
<html>
<head>
    <title>Sample Page</title>
</head>
<body>
    <div class="container">
        <h1 id="main-title">Welcome</h1>
        <div class="content">
            <p class="text">This is a paragraph.</p>
            <ul class="list">
                <li class="item">Item 1</li>
                <li class="item">Item 2</li>
            </ul>
        </div>
    </div>
</body>
</html>
```

**Key HTML Elements:**
- **Tags**: `<div>`, `<p>`, `<h1>`, `<ul>`, `<li>`
- **Attributes**: `class="container"`, `id="main-title"`
- **Text Content**: The actual text between tags
- **Hierarchy**: Parent-child relationships between elements

### CSS Selectors

CSS selectors are patterns used to select HTML elements:

```python
# Element selectors
"div"                    # All <div> elements
"p"                      # All <p> elements

# Class selectors
".container"             # Elements with class="container"
".text"                  # Elements with class="text"

# ID selectors
"#main-title"            # Element with id="main-title"

# Attribute selectors
"[href]"                 # Elements with href attribute
"[class='item']"         # Elements with class exactly "item"

# Combinators
"div p"                  # <p> elements inside <div> elements
"div > p"                # Direct child <p> elements of <div>
"ul li.item"             # <li> elements with class "item" inside <ul>
```

### Common CSS Selector Patterns for Scraping

```python
# Articles or posts
"article"
".post"
".article"

# Product listings
".product"
".item"
"[data-product-id]"

# Tables
"table tbody tr"

# Navigation
"nav a"
".menu li"

# Content areas
".content"
"#main-content"
"main"
```

---

## 🌐 Making HTTP Requests

The `requests` library is Python's most popular HTTP library.

### Installation

```bash
pip install requests
```

### Basic GET Request

```python
import requests

# Simple GET request
response = requests.get("https://httpbin.org/html")

print(f"Status Code: {response.status_code}")
print(f"Content Type: {response.headers['content-type']}")
print(f"Content Length: {len(response.text)}")
```

### Response Object

```python
response = requests.get("https://httpbin.org/json")

# Response attributes
print(f"URL: {response.url}")
print(f"Status: {response.status_code}")
print(f"Headers: {dict(response.headers)}")
print(f"Raw Content: {response.content[:100]}...")
print(f"Text Content: {response.text[:100]}...")

# For JSON responses
if response.headers['content-type'] == 'application/json':
    data = response.json()
    print(f"JSON Data: {data}")
```

### Request Headers

```python
# Basic headers
headers = {
    'User-Agent': 'MyScraper/1.0',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'en-US,en;q=0.5',
    'Accept-Encoding': 'gzip, deflate',
    'Connection': 'keep-alive',
}

response = requests.get("https://example.com", headers=headers)
```

### Handling Different Response Types

```python
# HTML content
html_response = requests.get("https://httpbin.org/html")
html_content = html_response.text

# JSON API
json_response = requests.get("https://httpbin.org/json")
json_data = json_response.json()

# Binary content (images, PDFs)
image_response = requests.get("https://httpbin.org/image/png")
with open('image.png', 'wb') as f:
    f.write(image_response.content)
```

### Error Handling

```python
try:
    response = requests.get("https://httpbin.org/status/404", timeout=10)
    response.raise_for_status()  # Raises HTTPError for bad status codes

    data = response.json()
    print("Success:", data)

except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection error")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e.response.status_code}")
except requests.exceptions.RequestException as e:
    print(f"Request error: {e}")
```

---

## 🍲 BeautifulSoup for HTML Parsing

BeautifulSoup is a Python library for parsing HTML and XML documents.

### Installation

```bash
pip install beautifulsoup4
```

### Basic Parsing

```python
from bs4 import BeautifulSoup
import requests

# Get HTML content
response = requests.get("https://httpbin.org/html")
html_content = response.text

# Parse HTML
soup = BeautifulSoup(html_content, 'html.parser')

print(f"Title: {soup.title.text if soup.title else 'No title'}")
print(f"First H1: {soup.h1.text if soup.h1 else 'No H1'}")
```

### Finding Elements

```python
# Find single element
title = soup.find('title')  # First <title> tag
heading = soup.find('h1', class_='main-title')  # H1 with specific class

# Find multiple elements
paragraphs = soup.find_all('p')  # All <p> tags
links = soup.find_all('a', href=True)  # All links with href

# Using CSS selectors
content_div = soup.select_one('.content')  # First element with class 'content'
menu_items = soup.select('nav li')  # All li elements inside nav
```

### Navigating the DOM

```python
# Get text content
title_text = soup.title.text
title_text = soup.title.get_text()  # Same result

# Get attributes
link_href = soup.a['href']  # Direct access
link_href = soup.a.get('href')  # Safe access

# Navigate relationships
parent = soup.p.parent  # Parent element
children = soup.div.children  # Child elements
siblings = soup.p.next_siblings  # Sibling elements

# Get all text from element
div_text = soup.div.get_text(separator=' ', strip=True)
```

### Extracting Data

```python
# Extract article titles
articles = soup.find_all('article')
for article in articles:
    title = article.find('h2')
    if title:
        print(f"Article: {title.text.strip()}")

# Extract product information
products = soup.select('.product')
for product in products:
    name = product.select_one('.product-name')
    price = product.select_one('.product-price')

    if name and price:
        print(f"{name.text.strip()}: {price.text.strip()}")

# Extract table data
rows = soup.select('table tr')
for row in rows:
    cells = row.find_all('td')
    if cells:
        data = [cell.text.strip() for cell in cells]
        print(f"Row data: {data}")
```

### Advanced Selection

```python
# Complex selectors
featured_articles = soup.select('article.featured h2')
product_links = soup.select('div.product a[href*="product"]')

# Attribute-based selection
external_links = soup.find_all('a', href=lambda x: x and x.startswith('http'))

# Text-based selection
important_paragraphs = soup.find_all('p', text=lambda t: t and 'important' in t.lower())

# Custom filters
def has_data_attr(tag):
    return tag.has_attr('data-product-id')

products_with_ids = soup.find_all(has_data_attr)
```

---

## 🔍 Data Extraction Techniques

### Structured Data Extraction

```python
def extract_product_data(soup):
    """Extract product information from HTML."""
    products = []

    product_elements = soup.select('.product-card')
    for product_elem in product_elements:
        product = {
            'name': product_elem.select_one('.product-name'),
            'price': product_elem.select_one('.product-price'),
            'rating': product_elem.select_one('.rating'),
            'url': product_elem.select_one('a')['href'] if product_elem.select_one('a') else None
        }

        # Clean and validate data
        if product['name']:
            product['name'] = product['name'].text.strip()
        if product['price']:
            product['price'] = product['price'].text.strip()
        if product['rating']:
            product['rating'] = float(product['rating'].text.strip())

        # Only add complete products
        if all([product['name'], product['price']]):
            products.append(product)

    return products
```

### Handling Missing Data

```python
def safe_extract_text(element, default="N/A"):
    """Safely extract text from an element."""
    return element.text.strip() if element else default

def safe_extract_attr(element, attr, default="N/A"):
    """Safely extract attribute from an element."""
    return element.get(attr, default) if element else default

# Usage
title = safe_extract_text(soup.select_one('h1'), "No title")
image_url = safe_extract_attr(soup.select_one('img'), 'src', "/default.jpg")
```

### Data Cleaning

```python
import re

def clean_price(price_text):
    """Clean and parse price text."""
    # Remove currency symbols and extra whitespace
    cleaned = re.sub(r'[^\d.,]', '', price_text.strip())

    # Handle different decimal separators
    if ',' in cleaned and '.' in cleaned:
        # European format (1.234,56)
        if cleaned.rfind(',') > cleaned.rfind('.'):
            cleaned = cleaned.replace('.', '').replace(',', '.')
        else:
            # US format (1,234.56)
            cleaned = cleaned.replace(',', '')

    try:
        return float(cleaned)
    except ValueError:
        return None

# Usage
prices = ["$1,234.56", "€1.234,56", "$123", "Price: $45.67"]
for price_text in prices:
    price = clean_price(price_text)
    print(f"{price_text} -> {price}")
```

---

## ⚖️ Ethical Scraping and Legal Considerations

### Legal Considerations

```python
# Check robots.txt before scraping
def check_robots_txt(domain):
    """Check if scraping is allowed by robots.txt."""
    import urllib.robotparser

    rp = urllib.robotparser.RobotFileParser()
    rp.set_url(f"https://{domain}/robots.txt")
    rp.read()

    return rp.can_fetch('*', '/')

# Usage
if check_robots_txt('example.com'):
    print("Scraping allowed")
else:
    print("Scraping not allowed by robots.txt")
```

### Rate Limiting

```python
import time
import random

class PoliteScraper:
    """Scraper that respects rate limits."""

    def __init__(self, requests_per_minute=30):
        self.requests_per_minute = requests_per_minute
        self.last_request_time = 0
        self.min_delay = 60 / requests_per_minute  # Minimum delay between requests

    def wait_before_request(self):
        """Wait appropriate time before making request."""
        current_time = time.time()
        time_since_last = current_time - self.last_request_time

        if time_since_last < self.min_delay:
            sleep_time = self.min_delay - time_since_last + random.uniform(0.1, 1.0)
            time.sleep(sleep_time)

        self.last_request_time = time.time()

    def get(self, url, **kwargs):
        """Make request with rate limiting."""
        self.wait_before_request()
        return requests.get(url, **kwargs)
```

### Respectful Headers

```python
# Always identify yourself
headers = {
    'User-Agent': 'MyScraper/1.0 (https://example.com/contact)',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Language': 'en-US,en;q=0.9,en;q=0.8',
    'Accept-Encoding': 'gzip, deflate, br',
    'DNT': '1',  # Do Not Track
    'Connection': 'keep-alive',
    'Upgrade-Insecure-Requests': '1',
}

response = requests.get('https://example.com', headers=headers)
```

---

## 🐛 Error Handling and Debugging

### Network Error Handling

```python
def robust_request(url, max_retries=3, timeout=10):
    """Make request with comprehensive error handling."""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=timeout)
            response.raise_for_status()  # Check for HTTP errors
            return response

        except requests.exceptions.Timeout:
            print(f"Timeout on attempt {attempt + 1}")
        except requests.exceptions.ConnectionError:
            print(f"Connection error on attempt {attempt + 1}")
        except requests.exceptions.HTTPError as e:
            print(f"HTTP error {e.response.status_code} on attempt {attempt + 1}")
            if e.response.status_code == 429:  # Too Many Requests
                time.sleep(60)  # Wait longer for rate limits
        except requests.exceptions.RequestException as e:
            print(f"Request error on attempt {attempt + 1}: {e}")

        if attempt < max_retries - 1:
            time.sleep(2 ** attempt)  # Exponential backoff

    raise Exception(f"Failed to fetch {url} after {max_retries} attempts")
```

### HTML Parsing Error Handling

```python
def safe_html_parsing(html_content):
    """Parse HTML with error handling."""
    try:
        soup = BeautifulSoup(html_content, 'html.parser')

        # Check if parsing was successful
        if soup.title is None:
            print("Warning: No title found in HTML")
        if len(soup.find_all()) == 0:
            print("Warning: No elements found in HTML")

        return soup

    except Exception as e:
        print(f"HTML parsing error: {e}")
        return None
```

### Debugging Scrapers

```python
import logging

# Set up logging for debugging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def debug_scraper(url):
    """Scraper with comprehensive debugging."""
    logger = logging.getLogger(__name__)

    logger.info(f"Starting scrape of {url}")

    try:
        logger.debug("Making HTTP request")
        response = requests.get(url, timeout=10)
        logger.debug(f"Response status: {response.status_code}")
        logger.debug(f"Content length: {len(response.text)}")

        response.raise_for_status()

        logger.debug("Parsing HTML")
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extract data with logging
        titles = soup.find_all('h2')
        logger.info(f"Found {len(titles)} title elements")

        data = []
        for i, title in enumerate(titles):
            text = title.text.strip()
            logger.debug(f"Title {i+1}: {text[:50]}...")
            data.append(text)

        logger.info(f"Successfully extracted {len(data)} items")
        return data

    except Exception as e:
        logger.error(f"Scraping failed: {e}")
        raise
```

---

## 💾 Data Storage and Export

### JSON Export

```python
import json

def save_to_json(data, filename):
    """Save scraped data to JSON file."""
    with open(filename, 'w', encoding='utf-8') as f:
        json.dump(data, f, indent=2, ensure_ascii=False)

# Usage
scraped_data = [
    {"title": "Article 1", "url": "https://example.com/1"},
    {"title": "Article 2", "url": "https://example.com/2"}
]

save_to_json(scraped_data, 'articles.json')
```

### CSV Export

```python
import csv

def save_to_csv(data, filename, fieldnames=None):
    """Save scraped data to CSV file."""
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
    {"name": "Laptop", "price": 999.99, "category": "Electronics"},
    {"name": "Book", "price": 19.99, "category": "Education"}
]

save_to_csv(products, 'products.csv')
```

### Structured Data Storage

```python
from datetime import datetime
import json

class ScrapingSession:
    """Manage scraping sessions with metadata."""

    def __init__(self, scraper_name, target_url):
        self.session_data = {
            'scraper_name': scraper_name,
            'target_url': target_url,
            'start_time': datetime.now().isoformat(),
            'requests_made': 0,
            'data_extracted': [],
            'errors': []
        }

    def add_request(self):
        """Record a request made."""
        self.session_data['requests_made'] += 1

    def add_data(self, data):
        """Add extracted data."""
        self.session_data['data_extracted'].append({
            'timestamp': datetime.now().isoformat(),
            'data': data
        })

    def add_error(self, error):
        """Record an error."""
        self.session_data['errors'].append({
            'timestamp': datetime.now().isoformat(),
            'error': str(error)
        })

    def save_session(self, filename):
        """Save session data to file."""
        self.session_data['end_time'] = datetime.now().isoformat()

        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(self.session_data, f, indent=2, ensure_ascii=False)
```

---

## 🏗️ Building Your First Scraper

### Complete Scraper Example

```python
import requests
from bs4 import BeautifulSoup
import json
import time
import logging

# Set up logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class QuoteScraper:
    """Scraper for quotes from httpbin.org/html."""

    def __init__(self):
        self.base_url = "https://httpbin.org"
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'QuoteScraper/1.0'
        })

    def scrape_quotes(self):
        """Scrape quotes from the HTML page."""
        logger.info("Starting quote scraping")

        try:
            # Make request
            response = self.session.get(f"{self.base_url}/html")
            response.raise_for_status()

            # Parse HTML
            soup = BeautifulSoup(response.text, 'html.parser')

            # Find all blockquote elements (quotes)
            quotes = soup.find_all('blockquote')

            scraped_quotes = []
            for i, quote in enumerate(quotes, 1):
                text = quote.text.strip()
                if text:
                    scraped_quotes.append({
                        'id': i,
                        'text': text,
                        'length': len(text)
                    })
                    logger.debug(f"Found quote {i}: {text[:50]}...")

            logger.info(f"Successfully scraped {len(scraped_quotes)} quotes")
            return scraped_quotes

        except Exception as e:
            logger.error(f"Scraping failed: {e}")
            return []

    def save_quotes(self, quotes, filename="quotes.json"):
        """Save quotes to JSON file."""
        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(quotes, f, indent=2, ensure_ascii=False)

        logger.info(f"Saved {len(quotes)} quotes to {filename}")

def main():
    """Main scraping function."""
    scraper = QuoteScraper()

    # Scrape quotes
    quotes = scraper.scrape_quotes()

    if quotes:
        # Save to file
        scraper.save_quotes(quotes)

        # Print summary
        print(f"Scraped {len(quotes)} quotes:")
        for quote in quotes[:3]:  # Show first 3
            print(f"  {quote['id']}: {quote['text'][:60]}...")
    else:
        print("No quotes found")

if __name__ == "__main__":
    main()
```

---

## 🎯 Best Practices

### Respectful Scraping

```python
# Always check robots.txt
def can_scrape(url):
    """Check if scraping is allowed."""
    from urllib.parse import urlparse
    from urllib.robotparser import RobotFileParser

    parsed = urlparse(url)
    robots_url = f"{parsed.scheme}://{parsed.netloc}/robots.txt"

    rp = RobotFileParser()
    rp.set_url(robots_url)
    rp.read()

    return rp.can_fetch('*', parsed.path)

# Implement delays
def polite_request(url, min_delay=1.0):
    """Make request with minimum delay."""
    time.sleep(min_delay)
    return requests.get(url)
```

### Robust Parsing

```python
def robust_parsing(html_content):
    """Parse HTML with multiple fallback strategies."""
    parsers = ['html.parser', 'lxml', 'html5lib']

    for parser in parsers:
        try:
            soup = BeautifulSoup(html_content, parser)
            # Test if parsing worked
            if soup.title:
                return soup
        except Exception:
            continue

    # If all parsers fail, return basic soup
    return BeautifulSoup(html_content, 'html.parser')
```

### Error Recovery

```python
def scrape_with_recovery(url, max_retries=3):
    """Scrape with automatic error recovery."""
    for attempt in range(max_retries):
        try:
            response = requests.get(url, timeout=10)
            response.raise_for_status()

            soup = BeautifulSoup(response.text, 'html.parser')

            # Try to extract data
            data = extract_data(soup)
            if data:
                return data

        except Exception as e:
            logger.warning(f"Attempt {attempt + 1} failed: {e}")
            time.sleep(2 ** attempt)  # Exponential backoff

    logger.error(f"Failed to scrape {url} after {max_retries} attempts")
    return None
```

### Configuration Management

```python
class ScraperConfig:
    """Configuration for scraping operations."""

    DEFAULT_HEADERS = {
        'User-Agent': 'EthicalScraper/1.0 (contact@example.com)',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    }

    def __init__(self):
        self.request_timeout = 10
        self.max_retries = 3
        self.rate_limit_delay = 1.0
        self.respect_robots = True

config = ScraperConfig()
```

---

## 🎯 Key Takeaways

1. **Legal & Ethical**: Always check robots.txt and terms of service
2. **Requests Library**: Use for HTTP requests with proper headers and error handling
3. **BeautifulSoup**: Parse HTML with find(), find_all(), and select() methods
4. **CSS Selectors**: Use modern selectors for element targeting
5. **Error Handling**: Comprehensive exception handling for network and parsing errors
6. **Rate Limiting**: Implement delays to respect server resources
7. **Data Storage**: Save scraped data in JSON, CSV, or database formats
8. **Logging**: Use logging for debugging and monitoring scraper activity
9. **Robust Parsing**: Handle malformed HTML and missing elements gracefully
10. **Testing**: Test scrapers on small samples before full-scale scraping

---

## 🔗 Further Reading

- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Requests Documentation](https://requests.readthedocs.io/)
- [CSS Selector Reference](https://www.w3schools.com/cssref/css_selectors.asp)
- [Web Scraping Ethics](https://blog.apify.com/web-scraping-ethics/)

---

## 📝 Practice Exercises

1. **HTML Parser**: Build a function that extracts all links and their text from an HTML page
2. **Table Scraper**: Extract data from HTML tables and convert to CSV format
3. **Product Scraper**: Scrape product information from a simple e-commerce page
4. **News Scraper**: Extract article titles, dates, and summaries from a news website
5. **Rate Limited Scraper**: Implement a scraper that respects rate limits and handles 429 errors

---

**Tutorial Version**: 1.0
**Last Updated**: February 2026
**Estimated Reading Time**: 60 minutes
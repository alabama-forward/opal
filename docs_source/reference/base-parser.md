---
layout: default
title: "BaseParser API Reference"
---

# BaseParser API Reference

The `BaseParser` class is an **abstract base class (ABC)** that provides the foundation for all OPAL parsers. It defines the interface that all parser implementations must follow.

!!! note "Abstract Base Class"
    `BaseParser` cannot be instantiated directly. You must create a subclass and implement the required abstract methods.

## Class Definition

```python
from abc import ABC, abstractmethod

class BaseParser(ABC):
    """Base class defining the interface for all parsers (news, court cases, etc.)"""
```

**Location:** `opal/parser_module.py`

## Methods

### make_request()

```python
def make_request(self, urls: List[str]) -> Tuple[List[str], List[str]]
```

Shared request functionality for all parsers. Makes HTTP requests to the provided URLs and returns both the response HTML and the successfully processed URLs.

**Parameters:**
- `urls` (List[str]): List of URLs to request

**Returns:**
- Tuple containing:
  - `responses` (List[str]): List of HTML response text from successful requests
  - `successful_urls` (List[str]): List of URLs that were successfully processed

**Behavior:**
- Handles request exceptions gracefully by skipping failed URLs
- Includes a 5-second timeout per request
- Prints progress information during processing
- Raises `ValueError` if all URLs fail to process

**Example:**
```python
parser = Parser1819()
responses, urls = parser.make_request([
    "https://1819news.com/article-1",
    "https://1819news.com/article-2"
])
```

### parse_article() *(Abstract)*

```python
@abstractmethod
def parse_article(self, html: str, url: str) -> Dict[str, Any]
```

**Abstract method that must be implemented by all subclasses.** Parses a single article or data item from HTML content.

**Parameters:**
- `html` (str): HTML content of the page
- `url` (str): URL of the page being parsed

**Returns:**
- `Dict[str, Any]`: Dictionary containing parsed data (structure varies by parser)

**Implementation Required:** Every subclass must provide its own implementation of this method tailored to the specific website structure it's parsing.

### parse_articles()

```python
def parse_articles(self, urls: List[str]) -> str
```

Orchestrates the parsing of multiple articles. Uses `make_request()` to fetch content and calls `parse_article()` for each successful response.

**Parameters:**
- `urls` (List[str]): List of article URLs to parse

**Returns:**
- `str`: JSON-formatted string containing all parsed articles

**Example Output Structure:**
```json
[
    {
        "url": "https://example.com/article-1",
        "title": "Article Title",
        "content": "..."
    },
    {
        "url": "https://example.com/article-2",
        "title": "Another Article",
        "content": "..."
    }
]
```

## Creating a Custom Parser

To create a new parser, subclass `BaseParser` and implement the `parse_article()` method:

### Basic Pattern

```python
from opal.parser_module import BaseParser
from bs4 import BeautifulSoup
from typing import Dict, Any

class MyCustomParser(BaseParser):
    """Parser for MyWebsite.com"""

    def parse_article(self, html: str, url: str) -> Dict[str, Any]:
        """
        Parse an article from MyWebsite.com

        Args:
            html: HTML content of the page
            url: URL of the article

        Returns:
            Dictionary with parsed article data
        """
        soup = BeautifulSoup(html, 'html.parser')

        # Create result dictionary
        article = {
            'url': url,
            'title': '',
            'author': '',
            'date': '',
            'content': ''
        }

        # Extract data using BeautifulSoup
        title_tag = soup.find('h1', class_='article-title')
        if title_tag:
            article['title'] = title_tag.text.strip()

        author_tag = soup.find('span', class_='author-name')
        if author_tag:
            article['author'] = author_tag.text.strip()

        # Add more extraction logic...

        return article
```

### Using Your Custom Parser

```python
from opal.integrated_parser import IntegratedParser

# Create parser instance
parser = IntegratedParser(MyCustomParser)

# Process articles
results = parser.process_site(
    base_url="https://mywebsite.com",
    suffix="/articles",
    max_pages=5
)

# Results are returned as JSON string
import json
data = json.loads(results)
print(f"Parsed {len(data)} articles")
```

## Built-in Parser Implementations

OPAL includes several built-in parsers that extend `BaseParser`:

### Parser1819
Parses articles from 1819 News (Alabama conservative news outlet).

**Output Structure:**
```python
{
    'url': str,           # Article URL
    'title': str,         # Article title
    'author': str,        # Author name
    'date': str,          # Publication date
    'line_count': int,    # Number of content lines
    'line_content': {     # Article text by line
        'line 1': str,
        'line 2': str,
        ...
    }
}
```

### ParserDailyNews
Parses articles from Alabama Daily News.

**Output Structure:** Same as Parser1819

### ParserAppealsAL
Parses court case data from Alabama Appeals Court portal. Uses Selenium for JavaScript-rendered content.

**Output Structure:**
```python
{
    'court': str,                    # Court name
    'case_number': {                 # Case number with link
        'text': str,
        'link': str
    },
    'case_title': str,               # Case title
    'classification': str,           # Case type
    'filed_date': str,               # Filing date
    'status': str                    # Case status
}
```

## Error Handling

`BaseParser` includes built-in error handling:

- **Failed Requests:** Individual URL failures are logged but don't stop processing
- **Empty Results:** If all URLs fail, raises `ValueError`
- **Parsing Errors:** Subclasses should handle parsing errors gracefully

### Best Practices

1. **Handle Missing Elements:** Always check if HTML elements exist before accessing them
2. **Provide Defaults:** Return default values (e.g., "Unknown Author") for missing data
3. **Strip Whitespace:** Use `.strip()` on extracted text
4. **Validate Data:** Ensure required fields are present in return dictionary

## See Also

- [Creating Custom Parsers Guide](../developer/creating-custom-parsers.md) - Detailed tutorial
- [Parser1819 Documentation](parser-1819.md) - Example implementation
- [IntegratedParser](parser-classes-overview.md) - Orchestration layer

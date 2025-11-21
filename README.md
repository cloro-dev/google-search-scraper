# Google Search scraper

[![Google Search scraper by cloro](https://github.com/cloro-dev/google-search-scraper/blob/main/google-scraper-hero-image.png)](https://cloro.dev/google/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google Search scraper](https://cloro.dev/google-search/) by cloro enables developers to programmatically interact with Google Search and automatically collect search results along with structured metadata. Instead of manual data collection, you can retrieve results as parsed JSON, raw HTML, or other formats for seamless integration into your workflows.

You can use cloro's Google Search scraper for SEO monitoring, rank tracking, market research, and content idea generation. It handles dynamic content, supports real-time extraction, and eliminates the need to manage authentication, sessions, or anti-bot systems.

## How it works

The Google Search scraper handles the rendering, parsing, and delivery of results in your requested format. You provide your search query, API credentials, and optional parameters as shown below.

### Request sample (Python)

```python
import json
import requests

# API parameters
payload = {
    'query': 'best laptops for programming 2024',
    'country': 'US',
    'include': {
        'aioverview': {
            'markdown': True
        }
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/google',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())

# Save response to a JSON file
with open('response.json', 'w') as file:
    json.dump(response.json(), file, indent=2)
```

### Request sample (cURL)

````bash
curl -X POST https://api.cloro.dev/v1/monitor/google \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d
```json
{
    "query": "best laptops for programming 2024",
    "country": "US",
    "include": {
      "aioverview": {
        "markdown": true
      }
    }
  }
````

### Request sample (Node.js)

```javascript
const axios = require("axios");

const payload = {
  query: "best laptops for programming 2024",
  country: "US",
  include: {
    aioverview: {
      markdown: true,
    },
  },
};

axios
  .post("https://api.cloro.dev/v1/monitor/google", payload, {
    headers: {
      Authorization: "Bearer YOUR_API_KEY",
      "Content-Type": "application/json",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Request parameters

| Parameter            | Description                                                                 | Default value |
| -------------------- | --------------------------------------------------------------------------- | ------------- |
| `query`\*            | The search query (1-10,000 characters)                                      | –             |
| `country`            | Optional country/region code for localized results (e.g., `US`, `GB`, `DE`) | `US`          |
| `city`               | Canonical city name for hyperlocal results (auto-converted to uule)         | –             |
| `pages`              | Number of search results pages to scrape (1-20)                             | `1`           |
| `include.html`       | Include raw HTML response when set to true                                  | `false`       |
| `include.aioverview` | Include AI Overview (use `{"markdown": true}` for markdown format)          | `false`       |

- Mandatory parameters

---

### Output samples

The Google Search scraper API returns a structured JSON object containing Google Search results and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "organicResults": [
      {
        "position": 1,
        "title": "Best Laptops for Programming in 2024",
        "link": "https://example.com/best-laptops",
        "displayedLink": "https://example.com",
        "snippet": "Comprehensive guide to choosing the perfect laptop for software development..."
      }
    ],
    "peopleAlsoAsk": [
      {
        "question": "What specs should I look for in a programming laptop?",
        "type": "LINK",
        "snippet": "Key specifications include RAM, processor, storage...",
        "title": "Essential laptop specs for developers",
        "link": "https://example.com/laptop-specs"
      }
    ],
    "relatedSearches": [
      {
        "query": "best budget laptop for coding",
        "link": "https://google.com/search?q=best+budget+laptop+for+coding"
      }
    ],
    "aioverview": {
      "sources": [
        {
          "position": 1,
          "label": "Programming Laptop Guide",
          "url": "https://example.com/guide",
          "description": "Complete guide to development laptops"
        }
      ],
      "text": "Based on current information, the best laptops for programming...",
      "markdown": "**Based on current information**, the best laptops..."
    },
    "html": "<!DOCTYPE html>..."
  }
}
```

## Advanced features

### AI Overview extraction

Get Google's AI-generated summaries with source attribution by setting `include.aioverview` to `true` (or an object with `markdown: true`).

### Country-specific searches

Get localized search results for different countries by specifying the `country` parameter (e.g., "US", "GB", "DE", "FR", "JP").

### Hyperlocal search

Get city-specific search results using the `city` parameter with canonical city names (e.g., "New York", "Paris,Île-de-France", "Tokyo,Tokyo,Japan"). The backend automatically converts these to Google's location parameter.

### Multiple page scraping

Scrape multiple pages of search results in a single request using the `pages` parameter (up to 20 pages).

### Raw HTML access

For advanced use cases, get the complete HTML by setting `include.html` to `true`.

## Practical Google Search scraper use cases

1. **SEO monitoring:** Track keyword rankings and monitor search performance.
2. **Competitor analysis:** Analyze competitor presence and search visibility.
3. **Content ideas:** Generate content ideas from "People Also Ask" and related searches.
4. **Market research:** Gather insights on market trends and consumer interests.
5. **Brand monitoring:** Track brand mentions and sentiment in search results.
6. **Ad verification:** Verify ad placements and targeting (using specific location parameters).

## Why choose cloro?

- **Simple integration:** Clean API design with comprehensive documentation and examples.
- **Reliable performance:** >99% uptime and low latencies.
- **No infrastructure hassle:** We handle rate limiting, proxies, and browser management.
- **Flexible pricing:** Low-cost subscription model with transparent pricing.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping Google allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's Google Search scraper unique?

cloro's Google endpoint provides reliable access to Google Search with:

- **AI Overview extraction** with source attribution
- **Hyperlocal targeting** down to the city level
- **Built-in anti-detection** to ensure consistent results

### What's the recommended timeout for requests?

We recommend setting a timeout of 30-60 seconds. Our system handles automatic retries, but implementing your own retry logic provides the best reliability.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP` and more to get localized results relevant to specific regions.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [docs.cloro.dev](https://docs.cloro.dev)
- **Google Search scraper page:** [cloro.dev/google-search](https://cloro.dev/google-search/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Google](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us on [our contact page](https://cloro.dev/contact).

---

Built with ❤️ by the cloro team

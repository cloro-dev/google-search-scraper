# Google Search scraper

[![Google Search scraper by cloro](https://github.com/cloro-dev/google-search-scraper/blob/main/google-scraper-hero-image.png)](https://cloro.dev/google-search/?utm_source=github)

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

```bash
curl -X POST https://api.cloro.dev/v1/monitor/google \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "best laptops for programming 2024",
    "country": "US",
    "include": {
      "aioverview": {
        "markdown": true
      }
    }
  }'
```

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
| `device`             | Device type for search results (`desktop` or `mobile`)                      | `desktop`     |
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
    "ads": [
      {
        "position": 1,
        "blockPosition": "top",
        "title": "Best Programming Laptops - Fast Shipping",
        "url": "https://example.com/shop/laptops",
        "page": 1,
        "displayedUrl": "example.com/laptops",
        "domain": "example.com",
        "description": "Shop our selection of high-performance laptops for developers. Free shipping on orders over $500.",
        "sitelinks": [
          {
            "url": "https://example.com/gaming-laptops",
            "title": "Gaming Laptops",
            "description": "High-performance laptops for gaming and development"
          },
          {
            "url": "https://example.com/business-laptops",
            "title": "Business Laptops",
            "description": "Reliable laptops for professionals"
          }
        ]
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
    "html": [
      "https://storage.cloro.dev/results/b83e8dfd-c3a1-4b98-83b9-af91adc21e26/page-1.html"
    ]
  }
}
```

## Advanced features

### Sponsored ad extraction

The Google Search scraper automatically extracts sponsored ad results from both the top and bottom of search results pages. Each ad includes:

- **Position and placement**: Position within the ad block and whether it appeared at the top or bottom of the page
- **Ad details**: Title, destination URL, displayed URL, domain, and description
- **Sitelinks**: Extended ad sitelinks with titles, URLs, and descriptions when available

Ads are extracted automatically whenever they appear on the search results page—no additional parameters required.

### AI Overview extraction

Get Google's AI-generated summaries with source attribution by setting `include.aioverview` to `true` (or an object with `markdown: true`).

### Country-specific searches

Get localized search results for different countries by specifying the `country` parameter (e.g., "US", "GB", "DE", "FR", "JP").

### Multiple page scraping

Scrape multiple pages of search results in a single request using the `pages` parameter (up to 10 pages).

### Raw HTML access

For advanced use cases, get the complete HTML by setting `include.html` to `true`. Returns an array of HTML file URLs.

## Practical Google Search scraper use cases

1. **SEO monitoring:** Track keyword rankings and monitor search performance.
2. **Competitor analysis:** Analyze competitor presence and search visibility, including sponsored ad placements.
3. **Content ideas:** Generate content ideas from "People Also Ask" and related searches.
4. **Market research:** Gather insights on market trends and consumer interests.
5. **Brand monitoring:** Track brand mentions and sentiment in search results.
6. **Ad intelligence:** Monitor competitor ad strategies, ad copy, and landing pages to inform your own paid search campaigns.

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
- **Sponsored ad extraction** with full ad details, sitelinks, and placement information
- **Built-in anti-detection** to ensure consistent results

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP` and more to get localized results relevant to specific regions.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [docs.cloro.dev](https://docs.cloro.dev/)
- **Google Search scraper page:** [cloro.dev/google-search](https://cloro.dev/google-search/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and source confidence scoring.
- **[Google](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Google News](https://cloro.dev/google-news/)** - Extracts structured news articles from Google News with titles, snippets, sources, dates, and thumbnail images for news monitoring and media tracking.
- **[Grok](https://cloro.dev/grok/)** - Extracts structured data from Grok for current events, news tracking, and real-time information gathering.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us at [support@cloro.dev](mailto:support@cloro.dev).

---

Built with ❤️ by the cloro team

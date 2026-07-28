# Google Search Scraper API — Organic Results, PAA, Related Searches & AI Overview

[![Google Search scraper by cloro](https://github.com/cloro-dev/google-search-scraper/blob/main/google-scraper-hero-image.png)](https://cloro.dev/google-search/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

Scrape Google Search results via API. Returns parsed JSON with **organic results**, **People Also Ask** questions, **related searches**, featured snippets, knowledge panels, and optional **AI Overview** data. Python, cURL, and Node.js examples below.

Built for developers doing SEO rank tracking, SERP monitoring across countries and states, PAA/related-query research for content strategy, and market intelligence — without managing CAPTCHAs, rotating proxies, session state, or Google's anti-bot defenses.

## Quick start

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. Send a request:

   ```bash
   curl -X POST https://api.cloro.dev/v1/monitor/google \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"query": "best crm for small business", "country": "US"}'
   ```

3. Parse the returned JSON — `result.organic[]`, `result.paa[]`, `result.relatedSearches[]`, `result.aiOverview`.

Full examples in Python, cURL, and Node.js below.

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
    'location': 'New York,New York,United States',
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
    "location": "New York,New York,United States",
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
  location: "New York,New York,United States",
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
| `location`           | [Google canonical location name](https://developers.google.com/google-ads/api/reference/data/geotargets) for geo-targeted results (e.g., `New York,New York,United States`). Mutually exclusive with `uule` | –             |
| `uule`               | Pre-encoded Google UULE string for precise geo-targeting. Mutually exclusive with `location` | –             |
| `device`             | Device type for search results (`desktop` or `mobile`)                      | `desktop`     |
| `pages`              | Number of search results pages to scrape (1-10)                             | `1`           |
| `include.html`       | Include raw HTML response when set to true                                  | `false`       |
| `include.aioverview` | Include AI Overview (use `{"markdown": true}` for markdown format)          | `false`       |
| `include.paaAioverview` | Hydrate AI-Overview-type People Also Ask items with markdown and sources  | `false`       |

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
        "snippet": "Guide to choosing a laptop for software development..."
      }
    ],
    "ads": [
      {
        "position": 1,
        "blockPosition": "top",
        "type": "RESULT",
        "title": "Programming Laptops - Fast Shipping",
        "url": "https://example.com/shop/laptops",
        "page": 1,
        "displayedUrl": "example.com/laptops",
        "domain": "example.com",
        "description": "Shop our selection of laptops for developers. Free shipping on orders over $500.",
        "sitelinks": [
          {
            "url": "https://example.com/gaming-laptops",
            "title": "Gaming Laptops",
            "description": "Laptops for gaming and development"
          },
          {
            "url": "https://example.com/business-laptops",
            "title": "Business Laptops",
            "description": "Laptops for professionals"
          }
        ]
      },
      {
        "position": 1,
        "blockPosition": "rhs",
        "type": "SHOPPING_CARD",
        "category": "Sponsored products",
        "title": "Dell XPS 13 Developer Edition",
        "url": "https://www.google.com/aclk?sa=L&ai=...",
        "page": 1,
        "price": { "value": 1299, "currency": "$", "raw": "$1,299" },
        "store": "Dell.com",
        "imageUrl": "https://encrypted-tbn0.gstatic.com/images?q=tbn:..."
      }
    ],
    "peopleAlsoAsk": [
      {
        "question": "What specs should I look for in a programming laptop?",
        "type": "LINK",
        "snippet": "Key specifications include RAM, processor, storage...",
        "title": "Laptop specs for developers",
        "link": "https://example.com/laptop-specs"
      },
      {
        "question": "Is 16GB RAM enough for programming?",
        "type": "AIOVERVIEW",
        "markdown": "**Yes, 16GB RAM is generally sufficient** for most programming tasks...",
        "sources": [
          {
            "position": 1,
            "label": "Developer Hardware Guide",
            "url": "https://example.com/dev-hardware",
            "description": "Guide to developer hardware requirements"
          }
        ]
      }
    ],
    "peopleAreSaying": [
      {
        "position": 1,
        "title": "Best running shoes 2026 - what runners are saying",
        "link": "https://www.reddit.com/r/running/comments/example/",
        "date": "5 days ago"
      }
    ],
    "localResults": [
      {
        "position": 1,
        "title": "Joe's Pizza Broadway",
        "placeId": "/g/11bw4ws2mt",
        "rating": 4.4,
        "reviews": "26K",
        "price": "$10–20",
        "type": "Pizza",
        "address": "1435 Broadway",
        "description": "\"Fast service, great atmosphere, and truly scrumptious pizza.\""
      }
    ],
    "relatedSearches": [
      {
        "query": "best budget laptop for coding",
        "link": "https://google.com/search?q=best+budget+laptop+for+coding"
      }
    ],
    "shoppingCards": [
      {
        "title": "ASICS Women's Gel-Nimbus 28",
        "productLink": "",
        "category": "More products",
        "price": {
          "value": 169.99,
          "currency": "$",
          "raw": "$169.99"
        },
        "store": "DICK'S Sporting Goods",
        "rating": 4.5,
        "reviews": "384"
      }
    ],
    "aioverview": {
      "sources": [
        {
          "position": 1,
          "label": "Programming Laptop Guide",
          "url": "https://example.com/guide",
          "description": "Guide to development laptops"
        }
      ],
      "citationPills": [
        {
          "label": "Programming Laptop Guide",
          "citationPillId": 1,
          "url": "https://example.com/guide",
          "domain": "example.com",
          "description": "Guide to development laptops",
          "position": 1
        }
      ],
      "relatedLinks": [
        {
          "label": "Compare developer laptops",
          "citationPillId": 1,
          "url": "https://www.google.com/search?q=developer+laptops&ibp=oshop",
          "domain": "google.com"
        }
      ],
      "text": "Top laptops for programming include...",
      "markdown": "**Top laptops** for programming include...[Programming Laptop Guide](https://example.com/guide)[Compare developer laptops](https://www.google.com/search?q=developer+laptops&ibp=oshop)"
    },
    "html": [
      "https://storage.cloro.dev/results/b83e8dfd-c3a1-4b98-83b9-af91adc21e26/page-1.html"
    ]
  }
}
```

## Advanced features

### Sponsored ad extraction

The Google Search scraper automatically extracts sponsored ad results from every paid surface on the SERP. Each ad carries a `type` discriminator:

- **`type: "RESULT"`** — classic text ads at the top or bottom of the main column. Include `title`, destination `url`, `displayedUrl`, `domain`, `description`, and `sitelinks`.
- **`type: "SHOPPING_CARD"`** — shopping-style sponsored cards from the right-hand-side carousel (`blockPosition: "rhs"`) and the top-of-page carousels (`blockPosition: "top"`). Include `category` (carousel header text such as `"Sponsored products"`, `"Sponsored vehicles"`, or `"Sponsored hotels"`), `price`, optional `oldPrice` (MSRP / sale), `store`, and `imageUrl`. The destination `url` is a Google `aclk?` redirect.

For `SHOPPING_CARD` ads, the `description` field carries category-specific subtitle fragments joined with `·` (e.g. `Used - 94k miles · Greeley` on a vehicle card) rather than classic ad copy.

Ads are extracted automatically whenever they appear on the search results page. No additional parameters are required.

### People are saying extraction

When Google renders the "What people are saying" / "Trending posts and discussions" module on the SERP, the scraper extracts each card into the `peopleAreSaying` array. Each card carries `position`, `title`, `link`, and `date` (Google's raw relative-time text, not normalized). The field is omitted from `result` when no module is present.

### Local pack extraction

When Google renders the local pack (the map-backed "3-pack" of local businesses) on a local-intent query, the scraper extracts each place into the `localResults` array. Every place carries `position` and `title`; the remaining fields — `placeId`, `rating`, `reviews`, `price`, `type`, `yearsInBusiness`, `address`, `phone`, `hours`, `description`, and a `links` object (`website`, `directions`) — are included only when Google renders them, so restaurant packs tend to carry `price` while service packs (plumbers, dentists) tend to carry `yearsInBusiness`, `phone`, `hours`, and action `links`. Extraction is desktop only; the field is omitted from `result` when no local pack is present. The pack only exposes its visible places (typically 3); when Google renders a "More places" / "More businesses" control, the response also includes `localResultsMoreLink`, the URL of Google's expanded Local Finder for the query (the full list is not embedded — follow the link to retrieve it).

### Shopping card extraction

When Google renders organic shopping grids ("Popular products" or "More products") on the SERP, the scraper extracts each card into the `shoppingCards` array. When a section header can be parsed, each card includes a `category` field naming its parent section so callers can distinguish cards from coexisting shopping panels. The `shoppingCards` field is omitted from `result` when no shopping section is present.

### AI Overview extraction

Get Google's AI-generated summaries with source attribution by setting `include.aioverview` to `true` (or an object with `markdown: true`).

#### Citation pills array structure

When the AI Overview answer carries pill chips (e.g. a `[Chase Bank +3]` chip grouping four sources), the `aioverview.citationPills` array exposes each cited source as a self-contained entry. When a pill cites N sources, the array contains N entries sharing the same `citationPillId` but with per-source `label`, `url`, and `domain`. Group by `citationPillId` to recover pill-level structure. The field is omitted when no pills are present.

| Field            | Type    | Description                                                                                                                                                 |
| ---------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `label`          | string  | Per-source title from the sources rail (e.g. `"Programming Laptop Guide"`). Always present; may be an empty string when the rail has no title for this source — read `domain` / `url` for source identity in that case.  |
| `citationPillId` | integer | Stable identifier shared by all entries from the same visible chip. 1-based ordinal in document order.                                                      |
| `url`            | string  | Direct URL of the cited source.                                                                                                                             |
| `domain`         | string  | Host extracted from `url`, for grouping and display.                                                                                                        |
| `description`    | string  | Source snippet from the sources rail when Google ships one. Omitted when absent.                                                                            |
| `position`       | integer | 1-based position of this source in the sibling `aioverview.sources` array.                                                                                  |

#### Related links array structure

A citation chip can also expose a "View related links" flyout: URLs Google groups under the chip that are **not** in the `sources` rail (e.g. a Google Shopping comparison link). These surface in `aioverview.relatedLinks` so they never inflate `sources` or `citationPills`. Each entry shares the `citationPillId` of its chip but has no `position` — a related link is absent from `sources`. They also render inline in `markdown`, so `markdown` can carry more URLs than `sources` / `citationPills`; the extras are the related links listed here. The field is omitted when no chip carries related links.

| Field            | Type    | Description                                                                                                                          |
| ---------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `label`          | string  | The related page's own title (e.g. `"Compare developer laptops"`). May be an empty string when Google ships none — read `domain` / `url` for identity. |
| `citationPillId` | integer | Matches the `citationPillId` of the chip's citation-pill entries, so related links can be grouped with their pill.                  |
| `url`            | string  | Direct URL of the related link.                                                                                                     |
| `domain`         | string  | Host extracted from `url`, for grouping and display.                                                                                |
| `description`    | string  | Snippet Google ships for the related link. Omitted when absent.                                                                     |

### People Also Ask AI Overview hydration

Some People Also Ask items are AI-Overview-type answers rather than traditional linked snippets. Set `include.paaAioverview` to `true` to hydrate these items with markdown content and cited sources. Items that cannot be hydrated gracefully degrade to `type: "AIOVERVIEW"` without markdown or sources.

### Country and city-level geo-targeting

Get localized search results for different countries by specifying the `country` parameter (e.g., "US", "GB", "DE", "FR", "JP"). For city-level precision, add the `location` parameter using [Google canonical location names](https://developers.google.com/google-ads/api/reference/data/geotargets) (e.g., "New York,New York,United States") to geo-target results from ~100,000 supported locations. Alternatively, use `uule` with a pre-encoded Google UULE string. The `location` and `uule` parameters are mutually exclusive: provide one or the other, not both.

### Multiple page scraping

Scrape multiple pages of search results in a single request using the `pages` parameter (up to 10 pages).

### Raw HTML access

To get the full HTML, set `include.html` to `true`. Returns an array of HTML file URLs.

## Practical Google Search scraper use cases

1. **SEO monitoring:** Track keyword rankings and monitor search performance.
2. **Competitor analysis:** Analyze competitor presence and search visibility, including sponsored ad placements.
3. **Content ideas:** Generate content ideas from "People Also Ask" and related searches.
4. **Market research:** Gather insights on market trends and consumer interests.
5. **Brand monitoring:** Track brand mentions and sentiment in search results.
6. **Ad intelligence:** Monitor competitor ad strategies, ad copy, and landing pages to inform your own paid search campaigns.

## Why choose cloro?

- **Simple integration:** Clean API design with documentation and examples.
- **Reliable performance:** >99% uptime and low latencies.
- **No infrastructure hassle:** We handle rate limiting, proxies, and browser management.
- **Flexible pricing:** Low-cost subscription model with transparent pricing.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping Google allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's Google Search scraper unique?

cloro's Google endpoint provides access to Google Search with:

- **AI Overview extraction** with source attribution
- **Sponsored ad extraction** with ad details, sitelinks, and placement information
- **Built-in anti-detection** for consistent results

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP` and more to get localized results relevant to specific regions.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [cloro.dev/docs](https://cloro.dev/docs/)
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

If you have questions or need support, join our community at [r/cloroapi](https://www.reddit.com/r/cloroapi/).

---

Built with ❤️ by the cloro team

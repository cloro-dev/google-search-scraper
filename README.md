# Google Search Scraper

[![Google Search Scraper by cloro](https://github.com/cloro-dev/google-search-scraper/blob/main/google-scraper-hero-image.png)](https://cloro.dev/google-search/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google Search scraper](https://cloro.dev/google-search/?utm_source=github) by cloro returns the live results page as structured JSON: organic results, ads, People Also Ask, People Are Saying, local pack, related searches, shopping cards and the AI Overview.

## How do you scrape Google Search?

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. POST a query to `https://api.cloro.dev/v1/monitor/google`.
3. Read the parsed fields from the JSON response.

Google removed the `&num=100` parameter on September 11, 2025, so every provider now paginates at 10 results a page and depth is the dominant term in what a SERP costs. Use `pages` to control it, and price your workload at the depth you actually query rather than the headline rate.

### Request sample (Python)

```python
import requests

payload = {
    'query': 'best serp api',
    'country': 'US',
    'pages': 1,
    'include': {'aioverview': True},
}

response = requests.post(
    'https://api.cloro.dev/v1/monitor/google',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload,
)

print(response.json())
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/google \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query": "best serp api", "country": "US", "include": {"aioverview": true}}'
```

Node.js and async/webhook examples are in the [endpoint documentation](https://cloro.dev/docs/api-reference/endpoint/monitor-google).

### Request parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `query`\* | The search query | – |
| `country` | Country code for localized results (`US`, `GB`, `DE`) | `US` |
| `location` | [Google canonical location name](https://developers.google.com/google-ads/api/reference/data/geotargets) for geo-targeting. Mutually exclusive with `uule` | – |
| `uule` | Pre-encoded Google UULE string. Mutually exclusive with `location` | – |
| `device` | `desktop` or `mobile` | `desktop` |
| `pages` | Number of result pages to return | `1` |
| `include.aioverview` | Include the AI Overview block | `false` |
| `include.html` | Return a URL to the full HTML (expires after 24h) | `false` |

\* Required

## What data does the Google Search scraper return?

```json
{
  "success": true,
  "result": {
    "organicResults": [
      { "position": 1, "title": "Best SERP APIs", "url": "https://example.com/serp-apis", "domain": "example.com", "description": "Comparison of providers..." }
    ],
    "peopleAlsoAsk": [{ "question": "What is a SERP API?", "answer": "A SERP API returns search results as structured data..." }],
    "peopleAreSaying": [{ "source": "reddit.com", "snippet": "We switched after the num=100 change..." }],
    "localResults": [{ "title": "Example Agency", "rating": 4.6, "reviews": 128, "address": "123 Example St" }],
    "aioverview": { "text": "SERP APIs return search results as structured JSON...", "sources": [{ "position": 1, "url": "https://example.com" }] },
    "relatedSearches": ["serp api pricing", "google search api alternatives"]
  }
}
```

1. **`organicResults`** — position, title, URL, domain and description per result.
2. **`aioverview`** — the AI Overview block with its own cited sources, returned inline rather than behind a second request.
3. **`peopleAlsoAsk`** — PAA questions with their answers.
4. **`peopleAreSaying`** — the forum and social block Google now surfaces on many commercial queries.
5. **`localResults`** — the local pack with rating, reviews and address.
6. **`shoppingCards`** — product carousels with price and store.
7. **`ads`** — paid results, parsed and positioned.
8. **`relatedSearches`** — the related-query block.

Full field-level schemas are in the [endpoint reference](https://cloro.dev/docs/api-reference/endpoint/monitor-google).

## Use cases

- **Rank tracking** at city level using `location` or `uule`, where a national average and a metro result diverge.
- **AI Overview monitoring** — presence, text and cited sources, in the same response as the organic results.
- **SERP feature research** — how often PAA, local pack or People Are Saying fire on a query class.
- **Competitive analysis** — who holds the page for the queries your buyers run.

## FAQ

### What happened to `&num=100`?

Google removed it on September 11, 2025. Every provider now paginates at 10 results per page, which raised the cost of top-100 depth across the category. Use `pages` and price at your real depth.

### Does the AI Overview cost extra?

It is returned in the same response when you set `include.aioverview`, rather than requiring a second request. See the [pricing page](https://cloro.dev/pricing/) for how credits are counted at depth.

### Can I get city-level results?

Yes. `location` takes a Google canonical location name and `uule` takes a pre-encoded string. They are mutually exclusive.

### Is scraping Google results legal?

cloro returns publicly visible results pages. No court has ruled that scraping public results is unlawful, though the area is active. Check your own jurisdiction and use case.

## Learn more

- **Endpoint reference:** [cloro.dev/docs](https://cloro.dev/docs/api-reference/endpoint/monitor-google)
- **Product page:** [cloro.dev/google-search](https://cloro.dev/google-search/)

## Other cloro scrapers

[AI Mode](https://cloro.dev/ai-mode/) · [AI Overview](https://cloro.dev/ai-overview/) · [ChatGPT](https://cloro.dev/chatgpt/) · [Copilot](https://cloro.dev/copilot/) · [Gemini](https://cloro.dev/gemini/) · [Google News](https://cloro.dev/google-news/) · [Grok](https://cloro.dev/grok/) · [Perplexity](https://cloro.dev/perplexity/)

## Contact us

Questions or support: [r/cloroapi](https://www.reddit.com/r/cloroapi/).

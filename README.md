# Threads Search Scraper: Keyword Monitor

Search public Threads posts by keyword or phrase, sorted by recent or top. Author, text, engagement counts, media and permalinks. Monitor mode turns a keyword into a standing watch that bills only for posts new since the last run. No login, no API key.

**Run it on Apify:** [apify.com/themineworks/threads-search-scraper](https://apify.com/themineworks/threads-search-scraper)
**Docs, FAQ and pricing:** [themineworks.com/actors/threads-search-scraper](https://themineworks.com/actors/threads-search-scraper/)

**Price:** $1.00 per 1,000 posts on Apify's free plan, down to $0.60 on higher plans, plus a $0.005 start fee per run. Failed and empty results are never charged.

## What it returns

* Keyword or phrase search, recent or top ordering
* Author, text, and full engagement counts
* Media URLs and direct permalinks
* Monitor mode bills only for posts new since the last run
* No login, no API key

## Quick start

You need a free [Apify account](https://console.apify.com/sign-up) and its API token (Settings, API & Integrations).

### Python

```bash
pip install apify-client
```

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("themineworks/threads-search-scraper").call(run_input={
    "searchQuery": "web3",
    "maxPosts": 25
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```bash
npm install apify-client
```

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('themineworks/threads-search-scraper').call({
    "searchQuery": "web3",
    "maxPosts": 25
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### cURL

One request that runs the actor and returns the results in the response (for runs under 5 minutes):

```bash
curl -X POST "https://api.apify.com/v2/acts/themineworks~threads-search-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"searchQuery": "web3", "maxPosts": 25}'
```

### Command line

This repo includes ready-made clients that save results to JSON and CSV:

```bash
python3 threads_search_scraper.py --token YOUR_APIFY_TOKEN --search-query "web3" --max-posts "25"
node threads_search_scraper.mjs --token YOUR_APIFY_TOKEN --search-query "web3" --max-posts "25"
```

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `searchQuery` (required) | string | `""` | Search keyword |
| `maxPosts` | integer | `25` | Maximum posts to return across the whole run |
| `resultType` | string | `"recent"` | For search and hashtag modes: 'recent' or 'top' |
| `includeTagWindow` | boolean | `true` | Also read the tag feed for your query, which can surface posts that carry the tag without spelling out the… |
| `monitorMode` | boolean | `false` | Run on a schedule and get ONLY posts not delivered in a previous run, so a standing keyword watch bills for… |

## Output

One row per result, as JSON, CSV, Excel or through the API.

| Field | Type | Description |
|---|---|---|
| `post_id` | string | Unique numeric post ID |
| `code` | string | Threads post shortcode |
| `url` | string | Full URL to the Threads post |
| `username` | string | Username of the post author |
| `user_full_name` | string | Display name of the post author |
| `text` | string | Text content of the post |
| `posted_at` | string | ISO 8601 timestamp when the post was published |
| `like_count` | integer | Number of likes on the post |
| `reply_count` | integer | Number of replies to the post |
| `repost_count` | integer | Number of reposts |
| `quote_count` | integer | Number of quote posts |
| `media_type` | string | Type of media attached (text, image, video, carousel) |
| `hashtags` | array | List of hashtags used in the post |
| `scraped_at` | string | ISO 8601 timestamp of when the record was scraped |

## Use it from an AI agent

The actor works as a tool in Claude, Cursor or any MCP client through Apify's MCP server:

```
https://mcp.apify.com/?tools=themineworks/threads-search-scraper
```

## FAQ

### What can I search by?

Any keyword or phrase. Results come back sorted recent or top, the same as searching on Threads itself.

### What does monitor mode do?

Remembers every post already delivered for a given keyword, so a scheduled run returns and bills only for posts new since the last one.

### Is this useful for brand monitoring?

Yes. Point it at a brand name or product term and run it on a schedule to catch new mentions without re-paying for posts you already have.

### Does it need a login or API key?

No. It reads the same public search results a logged-out visitor sees.

### How is this different from Threads Scraper?

This one is scoped to keyword search only. Threads Scraper covers profile, post, search, and hashtag modes in one actor.

### How much does the Threads Search Scraper cost?

$1.00 per 1,000 posts on Apify's free plan, down to $0.60 on higher plans, plus a $0.005 start fee per run. Failed and empty results are never charged. You can cap what a single run may spend with the maximum cost setting on Apify.

### Can I export the results to CSV or Excel?

Yes. Every run saves to an Apify dataset you can download as JSON, CSV, Excel or XML, or read through the API. The Python and Node clients in this repo also write the results to local files.

### Can I run it on a schedule?

Yes. Save your input as a task on Apify and attach a schedule, or call the API from your own cron job. Scheduled runs are billed the same way as manual ones.

## Related scrapers

* [Threads Scraper](https://themineworks.com/actors/threads-scraper/): Meta Threads data that returns actual data
* [Reddit Scraper](https://themineworks.com/actors/reddit-scraper/): Free Reddit data with full comment trees
* [LinkedIn Post Scraper](https://themineworks.com/actors/linkedin-post-search/): Search LinkedIn posts by keyword without login

Part of [The Mine Works](https://themineworks.com/): 151 pay-per-result scrapers with no login and no browser setup on your side.

## License

MIT © The Mine Works

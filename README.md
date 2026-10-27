# FSBO.com Scraper - For Sale By Owner Homes

Extract structured data from [fsbo.com](https://fsbo.com) — for-sale-by-owner homes from fsbo.com by city, state or ZIP code. Each listing arrives with its asking price and full property facts such as beds and baths, lot size, HOA fee and photos. Monitor mode flags new and price-changed listings on every run.

**[FSBO.com Scraper - For Sale By Owner Homes on Apify →](https://apify.com/blackfalcondata/fsbo-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from fsbo.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from fsbo.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "home",
  "location": "Austin, TX",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Keywords matched against the listing text, e.g. "home", "waterfront", "acreage", "fixer upper". Use a JSON array for several searches in one run. Required if no startUrls provided. |
| `location` | string | — | Where to search: "City, ST" (e.g. "Austin, TX"), a state, or a 5-digit ZIP. Leave empty to search nationwide. Use a JSON array for several locations in one run. |
| `states` | array | `[]` | Two-letter US state codes (e.g. TX, FL), or ALL for nationwide. Each state is browsed on its own, so this works without a search term or location. |
| `propertyTypes` | array | `[]` | Only these property types. Leave empty for all types. |
| `minPrice` | integer | — | Lowest asking price in whole US dollars. |
| `maxPrice` | integer | — | Highest asking price in whole US dollars. |
| `minBeds` | integer | — | Fewest bedrooms. |
| `maxBeds` | integer | — | Most bedrooms. Listings that do not state a bedroom count are kept. |
| `minBaths` | integer | — | Fewest bathrooms. |
| `maxBaths` | integer | — | Most bathrooms. Listings that do not state a bathroom count are kept. |
| `minSqft` | integer | — | Smallest living area in square feet. |
| `maxSqft` | integer | — | Largest living area in square feet. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste fsbo.com URLs: search results (fsbo.com/search?q=...), state pages (fsbo.com/search/list/state/tx), city pages (fsbo.com/search/list/miami-fl, optionally narrowed by type such as /land) or single listing pages (fsbo.com/search/<listing>). Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch full listing details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `lat` | number | — | Latitude of the centre point for a radius filter. Use together with Longitude and Search Radius. |
| `lon` | number | — | Longitude of the centre point for a radius filter. Use together with Latitude and Search Radius. |
| `radiusKm` | integer | — | Only keep listings within this many kilometres of the centre point. Listings without map coordinates are left out. Combine it with a city, state or ZIP in Location to keep the run small. |

---

## Output fields

Every listing returns the same 73-field schema. Missing values are `null` — never omitted.

- `listingId`
- `listingUrl`
- `url`
- `portalUrl`
- `addressSlug`
- `headline`
- `title`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `address`
- `street`
- `unitNumber`
- `city`
- `state`
- `zip`
- `postalCode`
- `county`
- `neighborhood`
- `latitude`
- `longitude`
- `askingPriceCents`
- `askingPrice`
- `price`
- `currency`
- `propertyType`
- `beds`
- `bedrooms`
- `baths`
- `bathrooms`
- `halfBaths`
- `sqft`
- `lotSize`
- `lotUnit`
- `yearBuilt`
- `stories`
- `garage`
- `basement`
- `hasPool`
- `hasHOA`
- `hoaFee`
- `cooling`
- `heating`
- `features`
- `interiorFeatures`
- `utilities`
- `amenities`
- `status`
- `mlsListed`
- `forSaleByOwner`
- `isUnclaimed`
- `planName`
- `listingSource`
- `publishedAt`
- `listedAt`
- `expiresAt`
- `daysOnMarket`
- `motivationScore`
- `motivationReasons`
- `specialConditions`
- `valuations`
- `primaryPhotoUrl`
- `photoUrls`
- `photos`
- `photoCount`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "listingId": "cmqdtcidy01yss601867xlj28",
  "listingUrl": "https://fsbo.com/search/252-spring-road-tunbridge-vt-05077-1781443303697",
  "url": "https://fsbo.com/search/252-spring-road-tunbridge-vt-05077-1781443303697",
  "portalUrl": "https://fsbo.com/search/252-spring-road-tunbridge-vt-05077-1781443303697",
  "addressSlug": "252-spring-road-tunbridge-vt-05077-1781443303697",
  "headline": "Country home with 5 bedrooms",
  "title": "Country home with 5 bedrooms",
  "description": "Country home on a dirt road 2.5 mile from Tunbridge Village with 5 bedrooms, 3 bathrooms, bonus room with first floor laundry. Beautiful yard with perennials and rose bush plus flo…",
  "descriptionText": "Country home on a dirt road 2.5 mile from Tunbridge Village with 5 bedrooms, 3 bathrooms, bonus room with first floor laundry. Beautiful yard with perennials and rose bush plus flo…",
  "descriptionHtml": null,
  "descriptionMarkdown": null,
  "address": "252 Spring Road, Tunbridge, VT 05077"
}
```

*Truncated — full records contain 73 fields. See Output fields for the complete schema.*

**[Try FSBO.com Scraper - For Sale By Owner Homes now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/fsbo-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00135 |

See the [actor on Apify](https://apify.com/blackfalcondata/fsbo-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape fsbo.com?**
Use this actor on Apify to extract structured data from fsbo.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get fsbo.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape fsbo.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check fsbo.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [FSBO.com Scraper - For Sale By Owner Homes](https://apify.com/blackfalcondata/fsbo-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*

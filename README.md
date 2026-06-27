[Homes Co Nz Scraper](https://apify.com/abotapi/homes-co-nz-scraper?fpr=data)

# Homes.co.nz Scraper

Collect rich residential listing data from `homes.co.nz` using structured site data.

## What it does

- Supports `search` mode from plain location names such as `Wellington`.
- Supports `url` mode for detail URLs and map URLs.
- Accepts multiple locations or multiple URLs in one run.
- Advances forward across structured result pages without relying on DOM pagination.
- Pulls structured listing data directly from the site's underlying data sources.
- Enriches each listing with property card data, agent and branch details, title metadata, valuations, media, and open home schedules.
- Applies structured filters for listing status, bedrooms, bathrooms, and price range.
- Supports output sorting by publication time or numeric price when available.

## Why this actor is different

The actor is structured-data first:

- Detail pages are resolved from the page's server-rendered state and then enriched with listing data.
- Map pages are resolved from the page's server-rendered state and then expanded into individual listings.
- Property enrichment adds valuation, title, branch, and agent data.

That keeps extraction faster and more stable than page-structure parsing.

## Input

- `mode`: `search` or `url`
- `locations`: one or more location names for search mode
- `urls`: one or more Homes detail or map URLs for URL mode
- `listingStatus`: `for_sale`, `for_rent`, `just_sold`, or `off_market`
- `minBedrooms`, `minBathrooms`, `minPrice`, `maxPrice`: structured query filters
- `sortBy`: default ordering, newest, oldest, highest price, or lowest price
- `maxListings`: maximum number of listings to output
- `maxPages`: maximum output pages per search target
- `pageSize`: listings to emit per output page
- `maxMapItems`: maximum listing candidates to collect from each map URL before detail enrichment
- `fetchPropertyCards`: whether to include extra property enrichment
- `includeRawApiResponses`: include raw structured payloads in output for debugging

## Output highlights

Each item contains the standard listing fields plus richer structured sections such as:

- `openHomes`
- `images`
- `media`
- `agents`
- `branch`
- `propertyDetails`
- `valuations`
- `estimates`
- `titleRecord`
- `sourceEndpoints`
- `seedContext`

## Notes

- This actor is tuned for structured site data first.
- If a Homes URL changes shape, the fallback is still the page's embedded state rather than page selectors.
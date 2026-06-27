[Homes Co Nz Scraper](https://apify.com/fatihtahta/homes-co-nz-scraper?fpr=data)

# Homes.co.nz Scraper

**Slug:** `fatihtahta/homes-co-nz-scraper`

## Overview

Homes.co.nz Scraper collects structured property records from [https://homes.co.nz](https://homes.co.nz), including listing URLs, addresses, prices, property attributes, location coordinates, images, agency details, open-home times, and listing metadata. Homes.co.nz is a New Zealand property search and housing information website, making its public listings useful for real estate research, market monitoring, and property-data enrichment. The actor turns location-based searches and optional filters into repeatable JSON output that can be used in analytics tools, data pipelines, spreadsheets, and operational reporting. It is designed for automated, recurring data acquisition with consistent record structure and practical controls for scope, volume, and filtering. Use it when you need dependable collection workflows without manually reviewing search results page by page.

## Why Use This Actor

- **Market research and analytics teams:** collect structured extraction results for suburb-level pricing analysis, housing supply monitoring, sales activity review, and market intelligence reporting.
- **Product and content teams:** populate real estate experiences, internal directories, comparison views, or editorial datasets with normalized public property attributes.
- **Developers and data engineering teams:** feed repeatable collection results into downstream systems, ETL jobs, warehouses, search indexes, and enrichment pipelines.
- **Lead generation and enrichment teams:** identify public listings, agencies, agents, addresses, property characteristics, and current availability signals for targeted workflows.
- **Monitoring and competitive tracking teams:** schedule recurring runs to track changes in listings, prices, locations, open homes, and visible property inventory over time.

## Common Use Cases

- **Market intelligence:** monitor supply, pricing, availability, bedrooms, bathrooms, floor area, land area, listing status, and geographic distribution.
- **Lead generation:** build targeted prospect lists from public property listings, agency names, agent names, and location-specific property activity.
- **Competitive monitoring:** track changes in public listings across locations, deal types, price bands, and property profiles.
- **Catalog and directory building:** populate internal databases with structured property records, canonical URLs, images, and location coordinates.
- **Data enrichment:** add current public attributes to CRM, BI, underwriting, research, or analytics datasets.
- **Recurring reporting:** schedule periodic runs for dashboards, alerts, snapshots, and trend analysis.

## Quick Start

1. Choose the `location` you want to collect from, such as a suburb, town, city, region, or area name.
2. Select a `deal_type` such as all homes, for sale, sold, for rent, or not listed.
3. Set a small `limit`, such as `25`, for the first validation run.
4. Add optional price, bedroom, bathroom, or sold-date filters if you need a narrower dataset.
5. Run the actor in Apify Console and inspect the first dataset records to confirm the output shape.
6. Increase the limit or schedule recurring runs once the results match your workflow.

## Input Parameters

Configure the available filters below to define the collection scope.

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `deal_type` | string | Listing status to collect. Allowed values: `all_homes`, `for_sale`, `sold`, `for_rent`, `not_listed`. | `all_homes` |
| `location` | string | Required search location. Enter a suburb, town, city, region, or area name. | – |
| `sold_within_months` | string | Sale-history window used only when `deal_type` is `sold`. Allowed values: `1_month`, `2_month`, `3_month`, `4_month`, `5_month`, `6_month`, `7_month`, `8_month`, `9_month`, `10_month`, `11_month`, `12_month`. | `12_month` |
| `min_price` | integer | Minimum price in NZD. For rental searches, this represents weekly rent in NZD. | – |
| `max_price` | integer | Maximum price in NZD. For rental searches, this represents weekly rent in NZD. | – |
| `min_bedroom` | string | Minimum bedroom count. Allowed values: `1`, `2`, `3`, `4`, `5+`. | – |
| `max_bedroom` | string | Maximum bedroom count. Allowed values: `1`, `2`, `3`, `4`, `5+`. | – |
| `min_bathroom` | string | Minimum bathroom count. Allowed values: `1`, `2`, `3`, `4`, `5+`. | – |
| `max_bathroom` | string | Maximum bathroom count. Allowed values: `1`, `2`, `3`, `4`, `5+`. | – |
| `limit` | integer | Maximum number of listings to save for the location. Leave empty to collect as many matching results as the actor can reach. Minimum: `1`. | – |
| `proxyConfiguration` | object | Connection configuration for Apify Proxy or custom proxy settings. | `{"useApifyProxy":true,"apifyProxyGroups":["RESIDENTIAL"]}` |

## Choosing Inputs

Use `location` as the primary scope control. A broad location such as a city or region improves discovery, while a specific suburb or area produces a cleaner segment for reporting and comparison.

Filters make the dataset more targeted. Use `deal_type` to focus on sale, sold, rental, unlisted, or broad home records; use `sold_within_months` only for sold-property monitoring; and use price, bedroom, and bathroom fields to define the property profile you care about. Start with a small `limit` to validate the output shape, then increase it after confirming the dataset contains the right records.

## Example Inputs

### Broad location discovery

```
{
  "location": "Wellington",
  "deal_type": "all_homes",
  "limit": 50,
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

### Filtered homes for sale

```
{
  "location": "Auckland Central",
  "deal_type": "for_sale",
  "min_price": 600000,
  "max_price": 1200000,
  "min_bedroom": "2",
  "min_bathroom": "1",
  "limit": 100
}
```

### Recently sold property monitoring

```
{
  "location": "Christchurch",
  "deal_type": "sold",
  "sold_within_months": "3_month",
  "min_bedroom": "3",
  "max_bedroom": "5+",
  "limit": 75
}
```

## Output

### Output Destination

The actor writes results to an Apify dataset as JSON records. The dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

Each item contains a stable record envelope plus a listing payload. The envelope gives downstream systems predictable keys for sync, deduplication, and repeated collection runs.

### Record Envelope

The following fields are present on every record:

- **type** *(string, required)*: record type. For property listing records, the value is `listing`.
- **id** *(string, required)*: stable record identifier from the source record.
- **url** *(string, required)*: public Homes.co.nz listing URL.

Recommended idempotency key: `type + ":" + id`.

Use this key when deduplicating, upserting, or syncing records across repeated runs. The envelope makes records easier to merge, deduplicate, and synchronize across warehouses, CRMs, search indexes, and monitoring systems.

### Examples

#### Example: listing (`type = "listing"`)

```
{
  "type": "listing",
  "id": "d7312791-784d-49d5-a3be-6efb1e4e1f5b",
  "url": "https://homes.co.nz/address/wellington/te-aro/25-26-marion-street/o02G2",
  "canonicalUrl": "https://homes.co.nz/address/wellington/te-aro/25-26-marion-street/o02G2",
  "title": "Penthouse Living, Rooftop Deck + Secure Carpark",
  "price": "Deadline sale by 19 May",
  "currency": "NZD",
  "description": "Penthouse Living in the Heart of Te Aro | Rooftop Deck + Car Park Welcome to 25/26 Marion Street - a truly unique three-level penthouse offering space, sun, and an unbeatable inner-city lifestyle. Tucked away within an award-winning complex, this home delivers the perfect balance of vibrant city living and surprising privacy, centred around beautifully designed communal courtyards that enhance the sense of space and calm. Spread across three levels, the layout provides excellent separation and flexibility. The first level features two generous double bedrooms, both with built-in wardrobes and one with its own balcony, along with a combined bathroom and laundry. Upstairs, the main living level is filled with natural light thanks to floor-to-ceiling windows, opening out to a large balcony that creates seamless indoor-outdoor flow. The modern kitchen is well-appointed with excellent storage, and elevated views across the city. At the top, the showstopper - a supersized private rooftop deck enjoying all-day sun and stunning views, making it the perfect space for entertaining or simply unwinding above the city. Positioned with dual access from both Cuba and Marion Streets, you're just moments from Wellington's best cafés, restaurants, bars, and entertainment, with the waterfront, CBD, and town belt all within easy walking distance. Despite the central location, the complex itself remains quiet and well-maintained, offering a rare sense of retreat in the heart of the city. The property also includes the bonus of a secure covered car park and a storage locker with space for bikes or e-scooters, and the pet-friendly body corporate (subject to approval) adds further appeal. The current owner has moved overseas and is reluctantly selling a home he absolutely loved living in - a true reflection of the lifestyle on offer here. This is inner-city living at its absolute best - spacious, sun-soaked, and seriously hard to beat. Opportunities like this don't come up often. WCC: $2,906.86 BC: $16,228.85 Deadline Sale 19th May 2026 @ 2pm (unless sold prior)",
  "publishedAt": "2026-04-24T05:06:59.173000+00:00",
  "seedId": "ce8491fc345e",
  "seedType": "location",
  "seedValue": "Wellington",
  "pageIndex": 1,
  "address": "25/26 Marion Street, Te Aro, Wellington, Wellington",
  "bedrooms": 2,
  "bathrooms": 1,
  "carSpaces": 1,
  "floorArea": 89,
  "landArea": 0,
  "latitude": -41.294559478759766,
  "longitude": 174.77606201171875,
  "imageUrl": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15719945626921800957",
  "imageUrls": [
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15719945626921800957",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/8848780218094143516",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/5878184879063726804",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/759222190701225865",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/13444426752236865432",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/13749469367809929318",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/2246078709577459731",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/7396872896018035558",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/16745059885140919422",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/5144778119976408892",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15325649903130601737",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/12023478957422661001",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/17903213266326120768",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/17792728647648359443",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/11294238392467754968",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/11567304808583906920",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/14076594995771081872",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/4931280647118643036",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/9129516054508479834",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/12972976525719304782",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/733923465129589139",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15671044997487217473",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/126732565142021577",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/1008147220898952034",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/8102915747617304350",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/225819121387758502",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/6721209969657306488",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/9537383752840693664",
    "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/4856609782357179661"
  ],
  "agentNames": [
    "Angelina Tucci"
  ],
  "agencyName": "Comprende Real Estate",
  "propertyId": "0dd9ad61-9adf-430b-9dae-ae7e3d25f631",
  "listingId": "d7312791-784d-49d5-a3be-6efb1e4e1f5b",
  "listingType": "residential",
  "propertyType": "Townhouse",
  "headline": "Penthouse Living, Rooftop Deck + Secure Carpark",
  "displayPrice": "Deadline sale by 19 May",
  "parkingDescription": "Secure carpark",
  "smokers": false,
  "maxTenants": 0,
  "availableAt": "0001-01-01T00:00:00+00:00",
  "coverImageUrl": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15719945626921800957",
  "canUseImages": true,
  "images": [
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15719945626921800957",
      "classifications": [
        "outdoor_building"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/8848780218094143516",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/5878184879063726804",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/759222190701225865",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/13444426752236865432",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/13749469367809929318",
      "classifications": null,
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/2246078709577459731",
      "classifications": [
        "living_room"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/7396872896018035558",
      "classifications": [
        "kitchen"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/16745059885140919422",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/5144778119976408892",
      "classifications": [
        "living_room"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15325649903130601737",
      "classifications": [
        "bedroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/12023478957422661001",
      "classifications": [
        "bedroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/17903213266326120768",
      "classifications": [
        "bedroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/17792728647648359443",
      "classifications": [
        "bedroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/11294238392467754968",
      "classifications": [
        "bedroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/11567304808583906920",
      "classifications": [
        "foyer_entrance"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/14076594995771081872",
      "classifications": [
        "bathroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/4931280647118643036",
      "classifications": [
        "bathroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/9129516054508479834",
      "classifications": [
        "laundry_room"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/12972976525719304782",
      "classifications": [
        "bathroom"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/733923465129589139",
      "classifications": [
        "garage"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/15671044997487217473",
      "classifications": [
        "outdoor_building"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/126732565142021577",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/1008147220898952034",
      "classifications": [
        "patio_terrace"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/8102915747617304350",
      "classifications": [
        "dining_area"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/225819121387758502",
      "classifications": [
        "living_room"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/6721209969657306488",
      "classifications": [
        "living_room"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/9537383752840693664",
      "classifications": [
        "kitchen"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    },
    {
      "url": "https://s3-ap-southeast-2.amazonaws.com/homes-listing-images/4856609782357179661",
      "classifications": [
        "outdoor_building"
      ],
      "quality": "",
      "caption": "",
      "inspiring": null
    }
  ],
  "openHomes": [
    {
      "date": "2026-05-03",
      "start_time": "12:30",
      "end_time": "13:00"
    }
  ],
  "createdAt": "2026-04-24T05:06:59.173000+00:00",
  "updatedAt": "2026-04-24T05:24:41.423060+00:00",
  "featuredAt": "2026-04-24T05:06:59.173000+00:00",
  "status": 1,
  "authority": 6,
  "listingDetailUrl": "https://homes.co.nz/address/wellington/te-aro/25-26-marion-street/o02G2",
  "fingerprint": "8a35d56885f877bb3f63"
}
```

## Field Reference

### `listing`

- **type** *(string, required)*: record type, typically `listing`.
- **id** *(string, required)*: stable listing record identifier.
- **url** *(string, required)*: public listing URL.
- **canonicalUrl** *(string, optional)*: canonical public listing URL.
- **title / headline** *(string, optional)*: listing headline.
- **price / displayPrice** *(string, optional)*: visible price or price instruction.
- **currency** *(string, optional)*: currency code, typically `NZD`.
- **description** *(string, optional)*: listing description text.
- **publishedAt** *(string, optional)*: publication timestamp in ISO 8601 format.
- **createdAt / updatedAt / featuredAt** *(string, optional)*: listing lifecycle timestamps in ISO 8601 format.
- **seedId** *(string, optional)*: identifier for the input scope that produced the record.
- **seedType** *(string, optional)*: input scope type, such as `location`.
- **seedValue** *(string, optional)*: input value used for collection.
- **pageIndex** *(number, optional)*: result page index associated with the record.
- **address** *(string, optional)*: formatted property address.
- **bedrooms / bathrooms / carSpaces** *(number, optional)*: property room and parking counts.
- **floorArea / landArea** *(number, optional)*: property size values in square metres where available.
- **latitude / longitude** *(number, optional)*: geographic coordinates.
- **imageUrl / coverImageUrl** *(string, optional)*: primary image URL.
- **imageUrls** *(array of strings, optional)*: listing image URLs.
- **agentNames** *(array of strings, optional)*: visible listing agent names.
- **agencyName** *(string, optional)*: visible agency name.
- **propertyId** *(string, optional)*: property identifier.
- **listingId** *(string, optional)*: listing identifier.
- **listingType** *(string, optional)*: listing category, such as residential.
- **propertyType** *(string, optional)*: property type, such as townhouse.
- **parkingDescription** *(string, optional)*: parking information.
- **smokers** *(boolean, optional)*: smoking policy when provided.
- **maxTenants** *(number, optional)*: maximum tenant count when provided.
- **availableAt** *(string, optional)*: availability timestamp when provided.
- **canUseImages** *(boolean, optional)*: image usage indicator when provided.
- **images** *(array of objects, optional)*: image objects with metadata.
- **images.url** *(string, optional)*: image URL.
- **images.classifications** *(array of strings or null, optional)*: image classifications when available.
- **images.quality** *(string, optional)*: image quality label when available.
- **images.caption** *(string, optional)*: image caption when available.
- **images.inspiring** *(boolean or null, optional)*: image flag when available.
- **openHomes** *(array of objects, optional)*: scheduled open-home times.
- **openHomes.date** *(string, optional)*: open-home date.
- **openHomes.start_time / openHomes.end_time** *(string, optional)*: open-home time window.
- **status / authority** *(number, optional)*: source-provided listing status values.
- **listingDetailUrl** *(string, optional)*: public detail URL associated with the listing.
- **agencyListingIdentifier** *(string, optional)*: agency listing identifier when available.
- **pets / furnishings** *(string, optional)*: rental policy or furnishing details when provided.
- **videoUrls** *(array of strings, optional)*: listing video URLs when provided.
- **media** *(array of objects, optional)*: additional listing media payloads when provided.
- **priceValue** *(number, optional)*: source-provided numeric price when available.
- **buildingSiteCoverage** *(number, optional)*: source-provided site coverage value when available.
- **massGarageFreestanding / massGarageUnderRoof** *(number, optional)*: source-provided garage values when available.
- **fingerprint** *(string, optional)*: content fingerprint for change detection.

## Data Quality, Guarantees, And Handling

- **Structured records:** results are normalized into predictable JSON objects for downstream use.
- **Best-effort extraction:** fields may vary by region, session, availability, and target-site presentation changes.
- **Optional fields:** null-check optional fields in downstream code, especially media, open-home, pricing, and detail-enrichment values.
- **Deduplication:** use `type + ":" + id` as the recommended idempotency key.
- **Freshness:** results reflect the publicly available data at run time.
- **Repeated runs:** use the recommended idempotency key when syncing data into warehouses, CRMs, or search indexes.

## Tips For Best Results

- Start with a small `limit` to validate the output shape before scaling up.
- Use one location or market segment per run when you need cleaner segmentation.
- Leave optional filters empty when the goal is broad discovery.
- Add price, bedroom, bathroom, and deal-type filters gradually to understand how each field changes coverage.
- Use `sold_within_months` only when collecting sold-property records.
- Schedule recurring runs for monitoring workflows instead of relying on manual one-off jobs.
- Store `type + ":" + id` with each record to support deduplication and historical syncs.

## How to Run on Apify

1. Open the Actor in Apify Console.
2. Configure the available input fields for the target location and listing scope.
3. Set the maximum number of outputs to collect with `limit`.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or another supported dataset format.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**

Schedule runs to keep property datasets fresh for monitoring, reporting, and enrichment workflows. Recurring runs are useful for tracking listing changes, newly visible records, and market movement over time.

- Navigate to **Schedules** in Apify Console
- Create a new schedule, such as daily, weekly, or custom cron
- Configure input parameters
- Enable notifications for run completion
- Add webhooks for automated processing

### Integration Options

- **BI dashboards:** monitor pricing, availability, inventory, property attributes, and geographic coverage over time.
- **Data warehouses:** store historical listing snapshots for analysis, segmentation, and trend modeling.
- **CRM enrichment:** sync public listing, agency, agent, address, and property attributes into account or lead records.
- **Google Sheets or Airtable:** review smaller location-specific datasets, validate samples, and coordinate lightweight operations.
- **Webhooks:** trigger ingestion, validation, alerting, or reporting workflows after each completed run.
- **Slack, email, or notification workflows:** alert teams when scheduled runs complete or when downstream checks detect relevant changes.

## Export Formats And Downstream Use

Apify datasets can be exported through the Console or consumed programmatically by downstream systems. Use the format that best fits your operational workflow.

- **JSON:** for APIs, applications, and data pipelines.
- **CSV or Excel:** for spreadsheet workflows and manual review.
- **API access:** for automated ingestion into internal systems.
- **BI and warehouses:** for reporting, dashboards, and historical analysis.

## Performance

Estimated run times:

- **Small runs (< 1,000 outputs):** ~3–5 minutes
- **Medium runs (1,000–5,000 outputs):** ~5–15 minutes
- **Large runs (5,000+ outputs):** ~15–30 minutes

Execution time varies based on filters, result volume, and how much information is returned per record. Highly filtered runs can finish faster, while broad discovery or detail-rich records may take longer.

## Limitations

- Availability depends on what [https://homes.co.nz](https://homes.co.nz) publicly exposes at run time.
- Some optional fields may be missing on sparse records or listing types that do not publish those attributes.
- Very broad searches may take longer or require a higher `limit` to capture enough records.
- Target-side changes can affect field availability, naming, or visible values.
- Regional, account, or availability differences may change which results are visible.
- Sold, rental, unlisted, and active-sale records may expose different fields.

## Troubleshooting

- **No results returned:** check filter combinations, location spelling, and whether Homes.co.nz has matching public records for the selected scope.
- **Fewer results than expected:** broaden filters, raise `limit`, or verify that the target location contains enough matching records.
- **Some fields are empty:** optional fields depend on what each public record provides.
- **Run takes longer than expected:** reduce scope, lower `limit` for validation, or split broad collection into smaller location segments.
- **Output changed:** compare the current output with the field reference and report a small sample if support is needed.

## FAQ

### What data does this actor collect?

It collects public Homes.co.nz property listing records, including URLs, addresses, prices, descriptions, property attributes, images, coordinates, agency details, agent names, open-home times, and listing metadata when available.

### Can I filter by location, category, date, price, or other criteria?

You can filter by `location`, `deal_type`, sold-property date window, price range, bedroom range, bathroom range, and result `limit`. The actor does not expose category, direct URL, sort, radius, or keyword query inputs.

### Why did I receive fewer results than my limit?

The limit is a maximum, not a guaranteed count. A run may return fewer records if Homes.co.nz has fewer matching public results for the selected location and filters.

### Can I schedule recurring runs?

Yes. Use Apify schedules to run the actor daily, weekly, or on a custom cron schedule with the same input configuration.

### How do I avoid duplicates across runs?

Use `type + ":" + id` as the idempotency key when storing records in a database, warehouse, CRM, or search index.

### Can I export the data to CSV, Excel, or JSON?

Yes. Apify datasets support JSON, CSV, Excel, and other export formats from the Console and API.

### Does this actor collect private data?

The actor is intended to collect publicly available property listing information from Homes.co.nz. Users are responsible for ensuring their use of the data complies with applicable laws and terms.

### What should I include when reporting an issue?

Include the input used with sensitive values redacted, the run ID, expected versus actual behavior, and a small output sample if it helps explain the issue.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available property listing information from **[https://homes.co.nz](https://homes.co.nz)** for legitimate business purposes, including:

- **Real estate and property-market** research and market analysis
- **Operational reporting, enrichment, and monitoring workflows**
- **Public listing discovery and competitive intelligence**

Users are responsible for ensuring that their use of collected data complies with applicable laws, regulations, and the target site's terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site's terms.
- Respect individual privacy and personal information.
- Use data responsibly and avoid disruptive or excessive collection.
- Do not use this actor for spamming, harassment, or other harmful purposes.
- Follow relevant data protection requirements where applicable, such as GDPR and CCPA.

## Support

For help, use the actor page or Issues section associated with this actor. Include the input used with sensitive values redacted, the run ID, the expected versus actual behavior, and a small output sample when it is useful for diagnosis. Avoid sharing secrets, private account details, or unrelated personal information in support requests.
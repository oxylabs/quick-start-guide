# SERP Scraper API Quick Start Guide


- [Authentication](#authentication)
- [Integration methods](#integration-methods)
  - [Push-pull](#push-pull)
  - [Realtime](#realtime)
  - [Proxy Endpoint](#proxy-endpoint)

[SERP Scraper API](https://oxy.yt/3rNM) is a robust tool built to extract large volumes of public data from the leading search engines in real-time mode. With coordinate-level precision, you can use SERP Scraper API to access different search engines’ pages, such as regular search, hotel availability, keyword page, and other data types. SERP Scraper API is optimal for many business cases, including ads data tracking, brand monitoring, and others.

Read this Quick Start Guide to learn about SERP Scraper API, its technical features, how it works, and how to get started.

For a detailed explanation, see our [blog post](https://oxy.yt/NrMQ).

## Authentication

SERP Scraper API uses basic HTTP authentication that requires a username and password:

```shell
curl --user "USERNAME:PASSWORD" 'https://realtime.oxylabs.io/v1/queries' -H "Content-Type: application/json" -d '{"source": "SEARCH_ENGINE_search", "domain": "com", "query": "shoes"}'
```

## Integration methods

SERP Scraper API offers three main integration methods: Push-Pull, Realtime, and Proxy Endpoint, each of which has unique benefits.

### Push-pull  

**Example of a single query request:**

```python
curl --user "USERNAME:PASSWORD" 'https://data.oxylabs.io/v1/queries' -H "Content-Type: application/json" -d '{"source": "SERP_SCRAPER_API_SOURCE", "domain": "com", "query": "shoes", "geo_location": "United States"}'
```

The most popular search engine also supports data parsing out of the box. If you wish to get parsed and structured data instead of an HTML document of the page, please add `"parse": true` as a parameter.

**Sample of the initial response output:** 

```json
{
  "callback_url": null,
  "client_id": 1,
  "created_at": "2021-09-30 12:11:22",
  "domain": "com",
  "geo_location": "United States",
  "id": "6849314714636265473",
  "limit": 10,
  "locale": null,
  "pages": 1,
  "parse": false,
  "parser_type": null,
  "render": null,
  "url": null,
  "query": "shoes",
  "source": "SERP_SCRAPER_API_SOURCE",
  "start_page": 1,
  "status": "pending",
  "storage_type": null,
  "storage_url": null,
  "subdomain": "www",
  "content_encoding": "utf-8",
  "updated_at": "2021-09-30 12:11:22",
  "user_agent_type": "desktop",
  "session_info": null,
  "statuses": [],
  "_links": [
    {
      "rel": "self",
      "href": "http://data.oxylabs.io/v1/queries/6849314714636265473",
      "method": "GET"
    },
    {
      "rel": "results",
      "href": "http://data.oxylabs.io/v1/queries/6849314714636265473/results",
      "method": "GET"
    }
  ]
}
```

The initial response indicates that the job’s scrape-specific website has been created in our system.

In order to check whether the job is `"status": "done"`, you can use the link from `["_links"][0]["href"]`, which is *http://data.oxylabs.io/v1/queries/6849314714636265473*.

**Example of how to check job status:**

```shell
curl --user "USERNAME:PASSWORD" 'http://data.oxylabs.io/v1/queries/6849314714636265473'
```

The response will contain the same data as the initial response. If the job is `"status": "done"`, you can retrieve the contents using the link from `["_links"][1]["href"]`, which is *http://data.oxylabs.io/v1/queries/6849314714636265473/results*.

**Example of how to retrieve data:**

```
curl --user "USERNAME:PASSWORD" 'http://data.oxylabs.io/v1/queries/6849314714636265473/results'
```

**Sample of the response HTML data output:**

```json
{
  "results": [
    {
      "content": "<html>CONTENT</html>"
      "created_at": "2021-09-30 12:11:22",
      "updated_at": "2021-09-30 12:11:30",
      "page": 1,
      "url": "SEARCH_ENGINE_URL",
      "job_id": "6849314714636265473",
      "status_code": 200,
    }
  ]
}
```

**Sample of the response parsed data output in JSON:**

```json
{
    "results": [
        {
        "content": {
          "url": "SEARCH_ENGINE_URL",
          "page": 1,
          "results": {
            "pla": [
              {
                "pos": 1,
                "url": "https://www.dior.com/en_us/products/couture-3SN231YXX_H865_T48-b22-sneaker-",
                "price": "$1,300.00",
                "title": "DIOR B22 Sneaker White And Blue Technical Mesh And Gray Calfskin - Size 48 - Men",
                "seller": "Dior.com",
                "url_image": "",
                "image_data": "",
                "pos_overall": 1
              },
              {...}
            ],
            "paid": [],
            "organic": [
              {
                "pos": 1,
                "url": "https://www.shoes.com/",
                "desc": "Deals up to 75% off along with FREE Shipping above $50 on shoes, boots, sneakers, and sandals at Shoes.com. Shop top brands like Skechers, Clarks, ...‎Women · ‎Men · ‎Kids' Shoes · ‎Return Policy",
                "title": "Shoes, Sneakers, Sandals, Boots Up To 75% Off | Shoes.com",
                "url_shown": "https://www.shoes.com",
                "pos_overall": 22
              },
              {
                "pos": 2,
                "url": "https://www.rackroomshoes.com/",
                "desc": "Shop in-store or online for name brand sandals, athletic shoes, boots and accessories for women, men and kids. FREE shipping with $65+ online purchase.",
                "title": "Rack Room Shoes: Shoes Online with Free Shipping*",
                "url_shown": "https://www.rackroomshoes.com",
                "pos_overall": 23
              },
              {...}
            ],
            "local_pack": [
              {
                "phone": "(620) 331-9985",
                "title": "Shoe Dept.",
                "rating": 4.1,
                "address": "Independence, KS",
                "subtitle": "Shoe store",
                "pos_overall": 19,
                "rating_count": 33
              },
              {...}
            ],
            "related_searches": {
              "pos_overall": 36,
              "related_searches": [
                "shoes online",
                "shoes websites",
                "shoes for girls",
                "nike shoes",
                "shoes for men",
                "shoes drawing"
              ]
            },
            "related_questions": [
              {
                "pos": 1,
                "search": {
                  "url": "/search?gl=us&hl=en&q=What%27s+the+best+online+shoe+store%3F&sa=X&ved=2ahUKEwiMv-fD1abzAhXTK7kGHWYCBpAQzmd6BAgLEAU",
                  "title": "What's the best online shoe store?"
                },
                "source": {
                  "url": "https://www.thetrendspotter.net/online-shoe-stores/",
                  "title": "25 Best Online Shoe Stores for Looking Stylish - The Trend Spotter",
                  "url_shown": "https://www.thetrendspotter.net › online-shoe-stores"
                },
                "question": "What's the best online shoe store?",
                "pos_overall": 24
              },
              {...}
            ],
            "search_information": {
              "query": "shoes",
              "showing_results_for": "shoes",
              "total_results_count": 3090000000
            },
            "total_results_count": 3090000000
          },
          "last_visible_page": 10,
          "parse_status_code": 12000
        },
        "created_at": "2021-09-30 12:11:22",
        "updated_at": "2021-09-30 12:11:28",
        "page": 1,
        "url": "SEARCH_ENGINE_URL",
        "job_id": "6849314714636265473",
        "status_code": 200
        }
    ]
  }
```

### Realtime

**Sample:**

```shell
curl --user "USERNAME:PASSWORD" 'https://realtime.oxylabs.io/v1/queries' -H "Content-Type: application/json" -d '{"source": "SERP_SCRAPER_API_SOURCE", "domain": "com", "query": "shoes", "geo_location": "United States"}'
```

**Example response body that will be returned on open connection:**

```json
{
  "results": [
    {
      "content": "<html>
      CONTENT
      </html>"
      "created_at": "2019-10-01 00:00:01",
      "updated_at": "2019-10-01 00:00:15",
      "id": null,
      "page": 1,
      "url": "SERP URL",
      "job_id": "12345678900987654321",
      "status_code": 200
    }
  ]
}
```

### Proxy Endpoint

**Proxy Endpoint request sample:**

```shell
curl -k -x realtime.oxylabs.io:60000 -U USERNAME:PASSWORD -H "X-Oxylabs-Geo-Location: United States" "SEARCH_ENGINE_URL"
```

If you wish to find out more about SERP Scraper API Quick Start Guide, see our [blog post](https://oxy.yt/NrMQ).

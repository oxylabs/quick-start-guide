# Quick Start Guide for Scraping Search Engines
[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

[Web Scraper API](https://oxylabs.io/products/scraper-api/web) is a robust tool capable to extract large volumes of public data from the leading search engines in real-time mode. With coordinate-level precision, you can access different search engine data, such as regular search, ads, hotel availability, keywords, and other data types. Web Scraper API is perfect for many business cases, including SEO monitoring, ads data tracking, brand monitoring, and others.

This Python Guide will focus on scraping [Google](https://developers.oxylabs.io/scraper-apis/web-scraper-api/google) search results using the `google_search` source. You may also use `bing` and `bing_search` sources to scrape **Bing**, as well as the `universal` source to scrape **Baidu**, **Yandex**, and **other search engines**. For more programming languages and more complex examples, visit our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api).

You can get a **free API trial** by registering on the Oxylabs [dashboard](https://dashboard.oxylabs.io/en/), navigating to Web Scraper API, and selecting "Start free trial". Next, you'll be guided to create your API user credentials for authentication. Make sure to save these credentials in a secure place.

## How to set up the API?
- [1. Prepare Your Workspace](#1-prepare-your-workspace)
- [2. Insert Your API Credentials](#2-insert-your-api-credentials)
- [3. Create a Payload](#3-create-a-payload)
- [4. Send a Request](#4-send-a-request)
- [5. Get a Response](#5-get-a-response)
- [6. Enable Parsing](#6-enable-parsing)
  * [Example](#example)
- [7. Set the Geo-location](#7-set-the-geo-location)
  * [Example](#example-1)
- [8. Enable User-Agent](#8-enable-user-agent)
  * [Example](#example-2)
- [9. Render JavaScript](#9-render-javascript)
  * [Example](#example-3)
- [Integration methods](#integration-methods)
  * [Push-pull](#push-pull)
  * [Realtime](#realtime)
  * [Proxy Endpoint](#proxy-endpoint)
- [Additional information](#additional-information)


## 1. Prepare Your Workspace
Launch your IDE (integrated development environment) or any other preferred setup. Using `pip`, install a simple HTTP [requests](https://pypi.org/project/requests/) library via your terminal:

```bash
python -m pip install requests
```
You may want to use the `python3` keyword, depending on your Python setup:
```bash
python3 -m pip install requests
```

## 2. Insert Your API Credentials
Web Scraper API uses **basic HTTP authentication** that requires a username and password. Create a new Python file, import the installed library, and store your API credentials as `username` and `password` variables:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'
```

## 3. Create a Payload
Define a `payload` dictionary that will contain all the API parameters for your specific use case.

Pair the `source` parameter with the `google_search` value, which is designed to scrape Google search results using a keyword. Next, enter your preferred search phrase as a `query` parameter value. In this example, we'll use "gaming chair" as our search phrase:  

```python
import requests

username = 'username'
password = 'password'

payload = {
  'source': 'google_search', # Source indicates the scraper to use.
  'query': 'gaming chair'
}
```

## 4. Send a Request
Post the `payload` to the [realtime endpoint](https://developers.oxylabs.io/scraper-apis/getting-started/integration-methods/realtime) as per the example below:

```python
import requests

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))
```

## 5. Get a Response
The API's response is in a JSON format; therefore, you need to access the `content` key from the entire response, which contains scraped Google data.

```python
import requests

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']
```

Next, you can write the scraped HTML data into an HTML file:

```python
import requests

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']

with open ('scraped_website.html', 'w') as output:
  output.write(html_content)
```

After running this basic scraper, try out other parameters that will help you retrieve specific data.

## 6. Enable Parsing
To automatically parse search results data, add the `'parse': True` parameter to enable a dedicated parser. Visit [this link](https://faq.oxylabs.info/en/articles/8832750-what-are-oxylabs-dedicated-parsers) to find a list of all the dedicated parsers:

```python
  'parse': True
```

### Example

```python
import requests, json

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair',
  'parse': True
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

json_content = response.json()['results'][0]['content']

with open('parsed_website.json', 'w', encoding='utf-8') as output:
  json.dump(json_content, output, ensure_ascii=False, indent=4)
```
After running the above code, you should see a new JSON file with parsed data saved in your working directory.

If we don't have a dedicated parser for your target search engine page, you can use the [Custom Parser](https://developers.oxylabs.io/scraper-apis/custom-parser) feature instead to define your own parsing logic. It goes hand in hand with our **AI-powered OxyCopilot** feature, which can quickly generate `parsing_instructions` based on the prompts you provide in English. To access **OxyCopilot**, visit the [dashboard](https://dashboard.oxylabs.io/) and head to the **Web Scraper API Playground**.


## 7. Set the Geo-location
When scraping search engines, you may want to use a `geo_location` parameter to tailor your results to a specific location and overcome anti-scraping measures. The example below uses Google's Canonical Location Name `New York,New York,United States`:

```python
  'geo-location': 'New York,New York,United States'
```

### Example

```python
import requests, json

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair',
  'parse': True,
  'geo-location': 'New York,New York,United States'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

json_content = response.json()['results'][0]['content']

with open('parsed_website.json', 'w', encoding='utf-8') as output:
  json.dump(json_content, output, ensure_ascii=False, indent=4)
 ```

## 8. Enable User-Agent
If you want to receive search results as if you’d be visiting the search engine using a specific device, you can add a `user_agent_type` parameter. For this example, let's set it to `desktop_chrome`:

```python 
'user_agent_type': 'desktop_chrome'
```

### Example

```python
import requests, json

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair',
  'parse': True,
  'geo-location': 'New York,New York,United States',
  'user_agent_type': 'desktop_chrome'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

json_content = response.json()['results'][0]['content']

with open('parsed_website.json', 'w', encoding='utf-8') as output:
  json.dump(json_content, output, ensure_ascii=False, indent=4)
```
## 9. Render JavaScript
If your target search engine uses JavaScript to load content dynamically, use the `render` parameter and set it to `html` to enable our internal headless browser: 

```python 
    'render': 'html',
```

### Example
```python
import requests, json

username = 'username'
password = 'password'

payload = {
  'source': 'google_search',
  'query': 'gaming chair',
  'parse': True,
  'geo-location': 'New York,New York,United States',
  'user_agent_type': 'desktop_chrome',
  'render': 'html'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

json_content = response.json()['results'][0]['content']

with open('parsed_website.json', 'w', encoding='utf-8') as output:
  json.dump(json_content, output, ensure_ascii=False, indent=4)
```
This feature also allows you to control the browser using actions like **clicking**, **scrolling**, **inputting text**, and much more. See the [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/browser-instructions) to learn more.

## Integration methods

Web Scraper API offers three main integration methods: [Push-Pull](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull) (supports [batch queries](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull-batch)), [Realtime](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/realtime), and [Proxy Endpoint](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/proxy-endpoint), each of which has unique benefits.

### Push-pull

This method is asynchronous, allowing you to submit a scraping job and later retrieve the results with a separate API request once it's finished. This ensures quick and efficient handling of large-scale scraping operations.

**Example of a single query request:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'google_search',
    'query': 'gaming chair',
    'parse': True,
    'geo-location': 'New York,New York,United States'
}

response = requests.request(
    'POST',
    'https://data.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload
)

pprint(response.json())
```

**Sample of the initial response output:** 

```json
{
    "callback_url": null,
    "client_id": 1,
    "context": [
        {"key": "force_headers", "value": null},
        {"key": "force_cookies", "value": false},
        {"key": "hc_policy", "value": null},
        {"key": "results_language", "value": null},
        {"key": "safe_search", "value": null},
        {"key": "tbm", "value": null},
        {"key": "cr", "value": null},
        {"key": "filter", "value": null},
        {"key": "nfpr", "value": null},
        {"key": "tbs", "value": null},
        {"key": "fpstate", "value": null},
        {"key": "aomd", "value": null},
        {"key": "udm",  "value": null},
        {"key": "limit_per_page", "value": []}
    ],
    "created_at": "2024-09-17 12:17:20",
    "domain": "com",
    "geo_location": null,
    "id": "7241782277489835009",
    "limit": 10,
    "locale": null,
    "pages": 1,
    "parse": true,
    "parser_type": null,
    "parsing_instructions": null,
    "browser_instructions": null,
    "render": null,
    "url": null,
    "query": "gaming chair",
    "source": "google_search",
    "start_page": 1,
    "status": "pending",
    "storage_type": null,
    "storage_url": null,
    "subdomain": "www",
    "content_encoding": "utf-8",
    "updated_at": "2024-09-17 12:17:20",
    "user_agent_type": "desktop",
    "session_info": null,
    "statuses": [],
    "client_notes": null,
    "_links": [
        {
            "rel": "self",
            "href": "http://data.oxylabs.io/v1/queries/7241782277489835009",
            "method": "GET"
        },
        {
            "rel": "results",
            "href": "http://data.oxylabs.io/v1/queries/7241782277489835009/results",
            "method": "GET"
        },
        {
            "rel": "results-content",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241782277489835009/results/1/content"
            ],
            "method": "GET"
        },
        {
            "rel": "results-html",
            "href": "http://data.oxylabs.io/v1/queries/7241782277489835009/results?type=raw",
            "method": "GET"
        },
        {
            "rel": "results-content-html",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241782277489835009/results/1/content?type=raw"
            ],
            "method": "GET"
        },
        {
            "rel": "results-parsed",
            "href": "http://data.oxylabs.io/v1/queries/7241782277489835009/results?type=parsed",
            "method": "GET"
        },
        {
            "rel": "results-content-parsed",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241782277489835009/results/1/content?type=parsed"
            ],
            "method": "GET"
        }
    ]
}
```

The initial response indicates that the job’s results URL has been created in our system.

In order to check whether the job is `"status": "done"`, you can use the link from `["_links"][0]["href"]`, which is `http://data.oxylabs.io/v1/queries/7241782277489835009`.

**Example of how to check job status:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241782277489835009',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

The response will contain the same data as the initial response. If the job is `"status": "done"`, you can retrieve the contents using the link from `["_links"][1]["href"]`, which is `http://data.oxylabs.io/v1/queries/7241782277489835009/results`.

**Example of how to retrieve data:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241782277489835009/results',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

**Sample of the response HTML data output:**

```json
{
    "results": [
        {
            "content": "<html> CONTENT </html>",
            "created_at": "2024-09-17 12:17:20",
            "updated_at": "2024-09-17 12:17:24",
            "page": 1,
            "url": "https://www.google.com/search?q=gaming+chair&uule=w+CAIQICINdW5pdGVkIHN0YXRlcw&gl=us&hl=en",
            "job_id": "7241782277489835009",
            "is_render_forced": false,
            "status_code": 200,
            "parser_type": ""
        }
    ]
}
```

**Sample of the response output with parsed data in JSON:**

```json
{
    "results": [
        {
            "content": {
                "url": "https://www.google.com/search?q=gaming+chair&uule=w+CAIQICINdW5pdGVkIHN0YXRlcw&gl=us&hl=en",
                "page": 1,
                "results": {
                    "pla": {
                        "items": [
                            {
                                "pos": 1,
                                "url": "https://gtracing.com/products/footrest-series-gt800a?currency=USD&variant=40862153670736&utm_source=google&utm_medium=cpc&utm_campaign=Google%20Shopping&stkn=75810b58a06c",
                                "price": "$89.99",
                                "title": "GTRacing Gaming Chair wiht Footrest and Massage Lumbar Support GT800A 2024, WHITE",
                                "seller": "gtracing.com",
                                "url_image": "https://encrypted-tbn3.gstatic.com/shopping?q=tbn:ANd9GcRXrVwjaqGiJDrH2uBcb1kX8wj9VWzhF9oQg8TXn5uX_H4GxRaM-3inBEqRNeTRP_DC7iDBZfE1eJnJdFVv2nwxJGQ49VrWQu2IcHYm-FoPL3wmOWaDlqk9kMERgKlPUhWCABqd7xk&usqp=CAc",
                                "image_data": "UklGRmgUAABXRUJQVlA4..."
                            },
                            {"...": "..."}
                        ],
                        "pos_overall": 1
                    },
                    "paid": [],
                    "organic": [
                        {
                            "pos": 1,
                            "url": "https://www.pcgamer.com/best-gaming-chairs/",
                            "desc": "Aug 29, 2024 \u2014 The Secretlab Titan Evo stands out as the best gaming chair today, blending the best features from Secretlab's previous models and a decent warranty should\u00a0...",
                            "title": "Best gaming chairs in 2024: the seats I'd suggest for any ...",
                            "images": [
                                "/9j/4AAQSkZJRgABAQAAAQABAAD..."
                            ],
                            "url_shown": "https://www.pcgamer.com\u203a Hardware \u203a Gaming Chairs",
                            "pos_overall": 3,
                            "favicon_text": "PC Gamer"
                        },
                        {"...": "..."}
                    ],
                    "popular_products": [
                        {
                            "items": [
                                {
                                    "pos": 1,
                                    "price": "$44.99",
                                    "title": "BestOffice PC Gaming Chair",
                                    "rating": "3.6",
                                    "seller": "Amazon.com - Seller",
                                    "currency": "USD",
                                    "image_data": "UklGRngXAABXRUJQVlA4IGwXAABQYw...",
                                    "description": "Amazon.com - Seller,\u00a010+",
                                    "rating_count": 169
                                },
                                {"...": "..."}
                            ],
                            "pos_overall": 2
                        },
                        {
                            "items": [
                                {
                                    "pos": 1,
                                    "price": "$189.99",
                                    "title": "Dowinx Gaming Chair with Cat Ears and Massage Lumbar Support",
                                    "rating": "4.9",
                                    "seller": "Amazon.com - Seller",
                                    "currency": "USD",
                                    "description": "Amazon.com - Seller,\u00a010+",
                                    "rating_count": 8
                                },
                                {"...": "..."}
                            ],
                            "pos_overall": 11
                        }
                    ],
                    "related_searches": {
                        "pos_overall": 14,
                        "related_searches": [
                            "gaming chair under \u00a350",
                            "Gaming Chair cheap",
                            "Best gaming chair",
                            "Gaming Chair nearby",
                            "Gaming Chair under $100",
                            "Gaming Chair Amazon"
                        ]
                    },
                    "related_questions": {
                        "items": [
                            {
                                "pos": 1,
                                "answer": "Gaming chairs are designed in such a way that it provides adequate support for the lower back to alleviate muscle strain when sitting upright. Soreness and stiffness are alleviated by the memory foam head pillow, which provides extra support for the natural curvature of the neck.",
                                "source": {
                                    "url": "https://www.dxracer.com/pages/blog/the-benefits-of-ergonomic-gaming-chairs#:~:text=Gaming%20chairs%20are%20designed%20in,natural%20curvature%20of%20the%20neck.",
                                    "title": "The Benefits of Ergonomic Gaming Chairs | Blog | DXRacer USA",
                                    "url_shown": "DXRacer"
                                },
                                "question": "Are gaming chairs healthy?"
                            },
                            {"...": "..."}
                        ],
                        "pos_overall": 12
                    },
                    "search_information": {
                        "query": "gaming chair",
                        "showing_results_for": "gaming chair",
                        "total_results_count": 186000000
                    },
                    "total_results_count": 186000000
                },
                "last_visible_page": 10,
                "parse_status_code": 12000
            },
            "created_at": "2024-09-17 12:17:20",
            "updated_at": "2024-09-17 12:17:24",
            "page": 1,
            "url": "https://www.google.com/search?q=gaming+chair&uule=w+CAIQICINdW5pdGVkIHN0YXRlcw&gl=us&hl=en",
            "job_id": "7241782277489835009",
            "is_render_forced": false,
            "status_code": 200,
            "parser_type": ""
        }
    ]
}
```
You can find a complete Google search output example with parsed results [here](https://public-files.oxylabs.io/examples/google_output.json). Additionally, check out this [Google Jobs scraping tutorial](https://github.com/oxylabs/how-to-scrape-google-jobs) to see how you can utilize the Push-Pull method using `asyncio` and `aiohttp` libraries in Python.

### Realtime

This approach lets you send a request and receive the data over the same open HTTPS connection.

**Sample:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'google_search',
    'query': 'gaming chair',
    'parse': True,
    'geo-location': 'New York,New York,United States'
}

response = requests.request(
    'POST',
    'https://realtime.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload
)

pprint(response.json())
```

**Example response body that will be returned on an open connection:**

```json
{
    "results": [
        {
            "content": {"Here you'll see parsed data": "data"},
            "created_at": "2024-09-17 12:46:07",
            "updated_at": "2024-09-17 12:46:12",
            "page": 1,
            "url": "https://www.google.com/search?q=gaming+chair&uule=w+CAIQICINdW5pdGVkIHN0YXRlcw&gl=us&hl=en",
            "job_id": "7241789519521909761",
            "is_render_forced": false,
            "status_code": 200,
            "parser_type": ""
        }
    ]
}
```

### Proxy Endpoint

Instead of parameters such as `domain` and search `query`, Proxy Endpoint takes completely formed URLs. Therefore, it supports only a handful of additional request parameters.

**Proxy Endpoint request sample:**

```python
import requests
from pprint import pprint

USERNAME, PASSWORD = 'USERNAME', 'PASSWORD'

proxies = {
    'http': f'http://{USERNAME}:{PASSWORD}@realtime.oxylabs.io:60000',
    'https': f'https://{USERNAME}:{PASSWORD}@realtime.oxylabs.io:60000'
}

headers = {
    'x-oxylabs-geo-location': 'New York,New York,United States',
    'x-oxylabs-parse': '1'
}

response = requests.request(
    'GET',
    'https://www.google.com/search?q=gaming+chair',
    headers=headers, # Pass the defined headers.
    verify=False,  # Accept our certificate.
    proxies=proxies,
)

pprint(response.json())
```

## Additional information

As you can see from this detailed example, Web Scraper API is easy to integrate and is packed with lots of valuable features. You can find more details on scraping other popular targets in our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api). Web Scraper API additionally includes E-Commerce and Other website scraping capabilities, which you can access via a single subscription plan. [Get in touch](https://oxy.yt/LrYs) with our sales team if you need more assistance with Web Scraper API.


# Quick Start Guide for Scraping Other Websites

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

[Web Scraper API](https://oxylabs.io/products/scraper-api/web) serves as a trustworthy solution for gathering information from complicated targets and ensures the ease of the crawling process. It fits various use cases, including website changes monitoring, fraud protection, and travel fare monitoring.

This guide will show you how to scrape any public website, other than e-commerce sites and search engines, by utilizing the `universal` source of Web Scraper API. For more programming languages and more complex examples, visit our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/other-websites).

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
- [8. Render JavaScript](#8-render-javascript)
  * [Example](#example-2)
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
Web Scraper API uses **basic HTTP authentication** which requires username and password. Create a new Python file, import the installed library, and store your Scraper API credentials as `username` and `password` variables:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'
```

## 3. Create a Payload
Define a `payload` dictionary that will contain all the API parameters for your specific use case.

Pair the `source` parameter with the `universal` value, which is designed for scraping any kind of website. Next, define the `url` parameter and paste the URL of the website you want to scrape. In this example, we use the [scraping sandbox](https://sandbox.oxylabs.io/products) search results page.

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal', # Here, we define the Universal source
    'url': 'https://sandbox.oxylabs.io/products'
}
```

## 4. Send a Request
Post the `payload` to the [realtime endpoint](https://developers.oxylabs.io/scraper-apis/getting-started/integration-methods/realtime) as per the example below:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))
```

## 5. Get a Response
The API's response is in a JSON format; therefore, you need to access the `content` key from the entire response, which contains scraped web data.

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']
```

Next, you can write the scraped HTML data into an HTML file:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']

with open('scraped_website.html', 'w') as output:
    output.write(html_content)
```

After running this basic scraper, try out other parameters that will help you retrieve specific data from your target website.

## 6. Enable Parsing
If you want to get parsed data, you can utilize the [Custom Parser](https://developers.oxylabs.io/scraper-apis/custom-parser) feature to define your own parsing logic. It goes hand in hand with our **AI-powered OxyCopilot** feature, which can quickly generate `parsing_instructions` based on the prompts you provide in English. To access **OxyCopilot**, visit the [dashboard](https://dashboard.oxylabs.io/) and head to the **Scraper API Playground**.

### Example

```python
import requests, json

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products',
    'parse': True,
    'parsing_instructions': {
        'products': {
            '_fns': [
                {
                    '_fn': 'xpath',
                    '_args': ['//div[contains(@class, "product-card")]']
                }
            ],
            '_items': {
                'Title': {
                    '_fns': [
                        {
                            '_fn': 'xpath_one',
                            '_args': ['.//h4/text()']
                        }
                    ]
                },
                'Price': {
                    '_fns': [
                        {
                            '_fn': 'xpath_one',
                            '_args': ['.//div[contains(@class, "price")]/text()']
                        }
                    ]
                }
            }
        }
    }
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

parsed_content = response.json()['results'][0]['content']

with open('parsed_website.json', 'w') as output:
    json.dump(parsed_content, output, indent=4)
```

After running the above code, you should see a new JSON file with parsed data saved in your working directory.

## 7. Set the Geo-location
The geo-location parameter will help you collect geo-restricted data from websites where the content is only available in certain countries. This parameter also helps to overcome IP bans and other anti-scraping measures. The example below uses a new IP address based in the United States with each request.

```python
    'geo-location': 'United States'
```

### Example

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products',
    'geo-location': 'United States'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']

with open('scraped_website.html', 'w') as output:
    output.write(html_content)
```

## 8. Render JavaScript
If a website uses JavaScript to load content dynamically, use the `render` parameter and set it to `html` to enable our internal headless browser: 

```python
    'render': 'html'
```

### Example

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products',
    'geo-location': 'United States',
    'render': 'html'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']

with open('scraped_website.html', 'w') as output:
    output.write(html_content)
```

This feature also allows you to control the browser using actions like **clicking**, **scrolling**, **inputting text**, and much more. See the [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/browser-instructions) to learn more.

## Integration methods

You can integrate Web Scraper API using the three main methods: [Push-Pull](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull) (supports [batch queries](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull-batch)), [Realtime](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/realtime), and [Proxy Endpoint](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/proxy-endpoint), each offering unique benefits.

### Push-Pull

This is an asynchronous method, allowing you to submit a scraping job and then make another request to the API to retrieve the scraped results once the job is done. This way, you'll be able to handle large scale scraping quickly and efficiently.

**Example of a single query request:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products',
    'geo-location': 'United States'
}

response = requests.request(
    'POST',
    'https://data.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload
)

pprint(response.json())
```
If you're observing low success rates or retrieve empty content, please try using the additional `'render': 'html'` parameter in your request. See more information about the render parameter [here](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/javascript-rendering).

**Sample of the initial response output:**

```json
{
    "callback_url": null,
    "client_id": 1,
    "context": [
        {"key": "force_headers", "value": null},
        {"key": "force_cookies", "value": false},
        {"key": "hc_policy", "value": null},
        {"key": "successful_status_codes", "value": []},
        {"key": "follow_redirects", "value": null},
        {"key": "cookies", "value": []},
        {"key": "headers", "value": []},
        {"key": "session_id", "value": null},
        {"key": "http_method", "value": "get"},
        {"key": "content", "value": null},
        {"key": "store_id", "value": null}
    ],
    "created_at": "2024-09-17 14:21:16",
    "domain": "io",
    "geo_location": null,
    "id": "7241813467278083073",
    "limit": 10,
    "locale": null,
    "pages": 1,
    "parse": false,
    "parser_type": null,
    "parsing_instructions": null,
    "browser_instructions": null,
    "render": null,
    "url": "https://sandbox.oxylabs.io/products",
    "query": "",
    "source": "universal",
    "start_page": 1,
    "status": "pending",
    "storage_type": null,
    "storage_url": null,
    "subdomain": "sandbox",
    "content_encoding": "utf-8",
    "updated_at": "2024-09-17 14:21:16",
    "user_agent_type": "desktop",
    "session_info": null,
    "statuses": [],
    "client_notes": null,
    "_links": [
        {
            "rel": "self",
            "href": "http://data.oxylabs.io/v1/queries/7241813467278083073",
            "method": "GET"
        },
        {
            "rel": "results",
            "href": "http://data.oxylabs.io/v1/queries/7241813467278083073/results",
            "method": "GET"
        },
        {
            "rel": "results-content",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241813467278083073/results/1/content"
            ],
            "method": "GET"
        },
        {
            "rel": "results-html",
            "href": "http://data.oxylabs.io/v1/queries/7241813467278083073/results?type=raw",
            "method": "GET"
        },
        {
            "rel": "results-content-html",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241813467278083073/results/1/content?type=raw"
            ],
            "method": "GET"
        }
    ]
}
```

In order to check whether the job is `"status": "done"`, you can use the link from `["_links"][0]["href"]` which is `http://data.oxylabs.io/v1/queries/7241813467278083073`.

**Example of how to check a job status:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241813467278083073',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

The response will contain the same data as the initial response. If the job is `"status": "done"`, you can retrieve the contents using the link from [“_links”][1][“href”] which is `http://data.oxylabs.io/v1/queries/7241813467278083073/results`.

**Example of how to retrieve data:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241813467278083073/results',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

**Sample of the response data output:**

```json
{
    "results": [
        {
            "content": "<!DOCTYPE html> CONTENT </html>",
            "created_at": "2024-09-17 14:21:16",
            "updated_at": "2024-09-17 14:21:22",
            "page": 1,
            "url": "https://sandbox.oxylabs.io/products",
            "job_id": "7241813467278083073",
            "is_render_forced": false,
            "status_code": 200
        }
    ]
}
```
You can find a complete response sample [here](https://public-files.oxylabs.io/examples/Zillow.json). Additionally, check out this [Airbnb scraping tutorial](https://github.com/oxylabs/how-to-scrape-airbnb) to see how you can utilize the Push-Pull method using `asyncio` and `aiohttp` libraries in Python.

### Realtime

With this method, you can send your request and receive data back on the same open HTTPS connection straight away. 

**Sample request:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'universal',
    'url': 'https://sandbox.oxylabs.io/products',
    'geo-location': 'United States'
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
            "content": "<!DOCTYPE html> CONTENT </html>",
            "created_at": "2024-09-18 06:24:26",
            "updated_at": "2024-09-18 06:24:28",
            "page": 1,
            "url": "https://sandbox.oxylabs.io/products",
            "job_id": "7242055853912623105",
            "is_render_forced": false,
            "status_code": 200
        }
    ]
}
```

### Proxy Endpoint

Proxy Endpoint takes completely formed URLs and supports only a handful of parameters that must be sent as HTTP headers. 

**Proxy Endpoint code sample:**

```python
import requests
from pprint import pprint

USERNAME, PASSWORD = 'USERNAME', 'PASSWORD'

proxies = {
    'http': f'http://{USERNAME}:{PASSWORD}@realtime.oxylabs.io:60000',
    'https': f'https://{USERNAME}:{PASSWORD}@realtime.oxylabs.io:60000'
}

headers = {
    'x-oxylabs-geo-location': 'United States'
}

response = requests.request(
    'GET',
    'https://sandbox.oxylabs.io/products',
    headers=headers, # Pass the defined headers.
    verify=False,  # Accept our certificate.
    proxies=proxies
)

pprint(response.json())
```

## Additional information

You can find more details on scraping other popular targets in our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api). Web Scraper API additionally includes SERP and E-Commerce website scraping capabilities, which you can access via a single subscription plan. [Get in touch](https://oxy.yt/LrYs) with our sales team if you need more assistance with Web Scraper API.

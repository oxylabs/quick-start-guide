# Quick Start Guide for Scraping E-Commerce Websites

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

With [Web Scraper API](https://oxylabs.io/products/scraper-api/web), you can easily retrieve real-time localized pricing and search information from most e-commerce sites at scale. It can scrape even the most complex e-commerce websites and perfectly fits business use cases such as price monitoring, product catalog mapping, competitor analysis. 

This Python guide will focus on scraping **Amazon** using the `amazon` source. The API also comes with dedicated sources for scraping **Walmart**, **Etsy**, **Best Buy**, and **Target**, as well as a `universal` source for **other e-commerce sites**. For more programming languages and more complex examples, visit our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api). 

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
- [Integration methods](#integration-methods)
  * [Push-Pull](#push-pull)
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
Web Scraper API uses **basic HTTP authentication** which requires username and password. Create a new Python file, import the installed library, and store your Scraper API credentials as `username` and `password` variables:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'
```

## 3. Create a Payload
Define a `payload` dictionary that will contain all the API parameters for your specific use case.

Pair the `source` parameter with the `amazon` value, which is designed to scrape any Amazon URL. Next, define the `url` parameter and paste the URL of the website you want to scrape. In the below example, we use the Amazon search results page for the search term `shoes`:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
  'source': 'amazon', # Here we define the source
  'url': 'https://www.amazon.com/s?k=shoes'
}
```

## 4. Send a Request
Post the `payload` to the [realtime endpoint](https://developers.oxylabs.io/scraper-apis/getting-started/integration-methods/realtime) as per the example below:

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
  'source': 'amazon',
  'url': 'https://www.amazon.com/s?k=shoes'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))
```

## 5. Get a Response
The API's response is in a JSON format; therefore, you need to access the `content` key from the entire response, which contains scraped Amazon data.

```python
import requests

username = 'USERNAME'
password = 'PASSWORD'

payload = {
  'source': 'amazon',
  'url': 'https://www.amazon.com/s?k=shoes'
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
  'source': 'amazon', # Here, we define the source
  'url': 'https://www.amazon.com/s?k=shoes'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

html_content = response.json()['results'][0]['content']

with open ('scraped_website.html', 'w') as output:
  output.write(html_content)
```

After running this basic scraper, try out other parameters that will help you retrieve specific data.

## 6. Enable Parsing
To automatically parse product data, add the `'parse': True` parameter to enable a dedicated parser. Visit [this link](https://faq.oxylabs.info/en/articles/8832750-what-are-oxylabs-dedicated-parsers) to find a list of all the dedicated parsers:

```python
    'parse': True
```

### Example

```python
import requests, json

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'amazon',
    'url': 'https://www.amazon.com/s?k=shoes',
    'parse': True
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

parsed_content = response.json()['results'][0]['content']

with open ('parsed_website.json', 'w') as output:
    json.dump(parsed_content, output, indent=4)
```
After running the above code, you should see a new JSON file with parsed data saved in your working directory.

If we don't have a dedicated parser for your target website, you can use the [Custom Parser](https://developers.oxylabs.io/scraper-apis/custom-parser) feature instead to define your own parsing logic. It goes hand in hand with our **AI-powered OxyCopilot** feature, which can quickly generate `parsing_instructions` based on the prompts you provide in English. To access **OxyCopilot**, visit the [dashboard](https://dashboard.oxylabs.io/) and head to the **Scraper API Playground**.


## 7. Set the Geo-location
The geo-location parameter will help you collect geo-restricted data from websites where the content is only available in certain locations, including delivery information. This parameter also helps to overcome IP bans and other anti-scraping measures. The example below uses a US-based ZIP code `90210` (see more details [here](https://developers.oxylabs.io/scraper-apis/e-commerce-scraper-api/features/geo-location#amazon) for localizing Amazon results):

```python
    'geo-location': '90210'
```

### Example

```python
import requests, json

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'amazon',
    'url': 'https://www.amazon.com/s?k=shoes',
    'parse': True,
    'geo-location': '90210'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

parsed_content = response.json()['results'][0]['content']

with open ('parsed_website.json', 'w') as output:
    json.dump(parsed_content, output, indent=4)
 ```
 
## 8. Render JavaScript
If your target marketplace uses JavaScript to load content dynamically, use the `render` parameter and set it to `html` to enable our internal headless browser: 

```python 
    'render': 'html',
```

### Example

```python
import requests, json

username = 'USERNAME'
password = 'PASSWORD'

payload = {
    'source': 'amazon',
    'url': 'https://www.amazon.com/s?k=shoes',
    'parse': True,
    'geo-location': '90210',
    'render': 'html'
}

response = requests.post('https://realtime.oxylabs.io/v1/queries', json=payload, auth=(username, password))

parsed_content = response.json()['results'][0]['content']

with open ('parsed_website.json', 'w') as output:
    json.dump(parsed_content, output, indent=4)
```
This feature also allows you to control the browser using actions like **clicking**, **scrolling**, **inputting text**, and much more. See the [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/browser-instructions) to learn more.


## Integration methods

Web Scraper API allows you to integrate using three main methods: [Push-Pull](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull) (supports [batch queries](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/push-pull-batch)), [Realtime](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/realtime), and [Proxy Endpoint](https://developers.oxylabs.io/scraper-apis/web-scraper-api/integration-methods/proxy-endpoint), each offering unique benefits.

### Push-Pull

This asynchronous method lets you submit a scraping job and later retrieve the results by making another API request once the job is complete. This approach enables quick and efficient handling of large-scale scraping tasks.

**Example of a single query request:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'amazon',
    'url': 'https://www.amazon.com/s?k=shoes',
    'parse': True,
    'geo-location': '90210'
}

response = requests.request(
    'POST',
    'https://data.oxylabs.io/v1/queries',
    auth=('USERNAME', 'PASSWORD'),
    json=payload
)

pprint(response.json())
```

If you are observing low success rates or retrieve empty content, please try using additional `'render': 'html'` parameter in your request. More information about render parameter can be found [here](https://developers.oxylabs.io/scraper-apis/web-scraper-api/features/javascript-rendering).

**Sample of the initial response output:**

```json
{
    "callback_url": null,
    "client_id": 1,
    "context": [
        {"key": "force_headers", "value": null},
        {"key": "force_cookies", "value": false},
        {"key": "hc_policy", "value": null},
        {"key": "category_id", "value": null},
        {"key": "merchant_id", "value": null},
        {"key": "check_empty_geo", "value": null},
        {"key": "safe_search", "value": true}
    ],
    "created_at": "2024-09-17 13:46:04",
    "domain": "com",
    "geo_location": null,
    "id": "7241804607775578113",
    "limit": 10,
    "locale": null,
    "pages": 1,
    "parse": true,
    "parser_type": null,
    "parsing_instructions": null,
    "browser_instructions": null,
    "render": null,
    "url": "https://www.amazon.com/s?k=shoes",
    "query": "shoes",
    "source": "amazon_search",
    "start_page": 1,
    "status": "pending",
    "storage_type": null,
    "storage_url": null,
    "subdomain": "www",
    "content_encoding": "utf-8",
    "updated_at": "2024-09-17 13:46:04",
    "user_agent_type": "desktop",
    "session_info": null,
    "statuses": [],
    "client_notes": null,
    "_links": [
        {
            "rel": "self",
            "href": "http://data.oxylabs.io/v1/queries/7241804607775578113",
            "method": "GET"
        },
        {
            "rel": "results",
            "href": "http://data.oxylabs.io/v1/queries/7241804607775578113/results",
            "method": "GET"
        },
        {
            "rel": "results-content",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241804607775578113/results/1/content"
            ],
            "method": "GET"
        },
        {
            "rel": "results-html",
            "href": "http://data.oxylabs.io/v1/queries/7241804607775578113/results?type=raw",
            "method": "GET"
        },
        {
            "rel": "results-content-html",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241804607775578113/results/1/content?type=raw"
            ],
            "method": "GET"
        },
        {
            "rel": "results-parsed",
            "href": "http://data.oxylabs.io/v1/queries/7241804607775578113/results?type=parsed",
            "method": "GET"
        },
        {
            "rel": "results-content-parsed",
            "href_list": [
                "http://data.oxylabs.io/v1/queries/7241804607775578113/results/1/content?type=parsed"
            ],
            "method": "GET"
        }
    ]
}
```
The initial response shows that the job’s results URL has been created in our system.

In order to check whether the job is `"status": "done"`, you can use the link from `["_links"][0]["href"]`, which is `http://data.oxylabs.io/v1/queries/7241804607775578113`.

**Example of how to check job status:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241804607775578113',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

The response will contain the same data as the initial response. If the job is `"status": "done"`, you can retrieve the contents using the link from `["_links"][1]["href"]`, which is `http://data.oxylabs.io/v1/queries/7241804607775578113/results`.

**Example of how to retrieve data:**

```python
import requests
from pprint import pprint


response = requests.request(
    'GET',
    'http://data.oxylabs.io/v1/queries/7241804607775578113/results',
    auth=('USERNAME', 'PASSWORD')
)

pprint(response.json())
```

**Sample of the response data output:**

```json
{
    "results": [
        {
            "content": {
                "url": "https://www.amazon.com/s?k=shoes",
                "page": 1,
                "query": "shoes",
                "results": {
                    "paid": [],
                    "organic": [
                        {
                            "pos": 1,
                            "url": "/Under-Armour-Charged-Assert-Running/dp/B087YYGSBY/ref=sr_1_1?dib=eyJ2IjoiMSJ9.zyqZPhulEA0ObyqPPKvIbJTy_phZLLUyTHsXna_u2wILv2VLwptLYj1LBVk9AInI2pIBVD1IxJ-H96RikeOZOi4RwJO-1eXUt0qDrdhpP9SK8or0O-hKUYCIZA7mBB40_o_n3n-yvwg9m-tBcwPbRtBjBF9tR-SFqNr4_SOTsLDiVOv8Fi9Wd6ko8gHJsDcfkCPLCcIPA0pB02CDmeweQh8Mg4-FDuFmwT1Cb790GDXapI59HfGW0ryA1uOugjJ13qKk67YVKW0a0AiHugjgct6ckQ5ApK9E-7Te3h4PArk.Sw1Pk-Vv5o9OEdEhAbxOEgNYv6k0wtaaM_sXgROYCJc&dib_tag=se&keywords=shoes&qid=1726580765&sr=8-1",
                            "asin": "B087YYGSBY",
                            "price": 49.95,
                            "title": "Men's Charged Assert 9 Running Shoe",
                            "rating": 4.6,
                            "currency": "USD",
                            "is_prime": true,
                            "url_image": "https://m.media-amazon.com/images/I/410-L0vF3+L._AC_UL320_.jpg",
                            "best_seller": false,
                            "price_upper": 49.95,
                            "is_sponsored": false,
                            "manufacturer": "",
                            "sales_volume": "700+ bought in past month",
                            "pricing_count": 1,
                            "reviews_count": 60759,
                            "is_amazons_choice": false,
                            "price_strikethrough": 70,
                            "shipping_information": "FREE delivery Sun, Sep 22 Or fastest delivery Thu, Sep 19"
                        },
                        {"...": "..."}
                    ],
                    "suggested": [],
                    "amazons_choices": [
                        {
                            "pos": 2,
                            "url": "/Under-Armour-Charged-Assert-Running/dp/B09XBVT7WX/ref=sr_1_2?dib=eyJ2IjoiMSJ9.zyqZPhulEA0ObyqPPKvIbJTy_phZLLUyTHsXna_u2wILv2VLwptLYj1LBVk9AInI2pIBVD1IxJ-H96RikeOZOi4RwJO-1eXUt0qDrdhpP9SK8or0O-hKUYCIZA7mBB40_o_n3n-yvwg9m-tBcwPbRtBjBF9tR-SFqNr4_SOTsLDiVOv8Fi9Wd6ko8gHJsDcfkCPLCcIPA0pB02CDmeweQh8Mg4-FDuFmwT1Cb790GDXapI59HfGW0ryA1uOugjJ13qKk67YVKW0a0AiHugjgct6ckQ5ApK9E-7Te3h4PArk.Sw1Pk-Vv5o9OEdEhAbxOEgNYv6k0wtaaM_sXgROYCJc&dib_tag=se&keywords=shoes&qid=1726580765&sr=8-2",
                            "asin": "B09XBVT7WX",
                            "price": 55.97,
                            "title": "Men's Charged Assert 10",
                            "rating": 4.5,
                            "currency": "USD",
                            "is_prime": true,
                            "url_image": "https://m.media-amazon.com/images/I/71k2ZobLduL._AC_UL320_.jpg",
                            "best_seller": false,
                            "price_upper": 55.97,
                            "is_sponsored": false,
                            "manufacturer": "",
                            "sales_volume": "400+ bought in past month",
                            "pricing_count": 1,
                            "reviews_count": 7719,
                            "is_amazons_choice": true,
                            "price_strikethrough": 75,
                            "shipping_information": "FREE delivery Sun, Sep 22 Or fastest delivery Thu, Sep 19"
                        }
                    ]
                },
                "last_visible_page": 7,
                "parse_status_code": 12000,
                "total_results_count": 70000
            },
            "created_at": "2024-09-17 13:46:04",
            "updated_at": "2024-09-17 13:46:07",
            "page": 1,
            "url": "https://www.amazon.com/s?k=shoes",
            "job_id": "7241804607775578113",
            "is_render_forced": false,
            "status_code": 200,
            "parser_type": ""
        }
    ]
}
```

You can find a complete Amazon search output example with parsed results [here](https://public-files.oxylabs.io/examples/amazon_search.json). You may also want to use the `asyncio` and `aiohttp` libraries in Python to handle the Push-Pull method effectively. For example, see this [Airbnb scraping tutorial](https://github.com/oxylabs/how-to-scrape-airbnb).

### Realtime

This method allows you to send a request and receive data over the same open HTTPS connection.

**Example of a realtime request:**

```python
import requests
from pprint import pprint


payload = {
    'source': 'amazon',
    'url': 'https://realtime.oxylabs.io/v1/queries',
    'parse': True,
    'geo-location': '90210'
}

response = requests.request(
    'POST',
    'https://data.oxylabs.io/v1/queries',
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
            "created_at": "2024-09-17 13:57:10",
            "updated_at": "2024-09-17 13:57:12",
            "page": 1,
            "url": "https://www.amazon.com/s?k=shoes",
            "job_id": "7241807398980055041",
            "is_render_forced": false,
            "status_code": 200,
            "parser_type": ""
        }
    ]
}
```

### Proxy Endpoint

Instead of parameters such as `domain` and search `query`, Proxy Endpoint uses completely formed URLs. As such, this method accepts fewer request parameters than other approaches.

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
    'x-oxylabs-geo-location': '90210',
    'x-oxylabs-parse': '1'
}

response = requests.request(
    'GET',
    'https://www.amazon.com/s?k=shoe',
    headers=headers, # Pass the defined headers.
    verify=False,  # Accept our certificate.
    proxies=proxies,
)

pprint(response.json())
```

## Additional information

As you can see from this detailed example, Web Scraper API is easy to integrate and is packed with lots of valuable features. You can find more details on scraping other popular targets in our [documentation](https://developers.oxylabs.io/scraper-apis/web-scraper-api). Web Scraper API additionally includes SERP and Other website scraping capabilities, which you can access via a single subscription plan. [Get in touch](https://oxy.yt/LrYs) with our sales team if you need more assistance with Web Scraper API.


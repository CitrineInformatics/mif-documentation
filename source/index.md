---
title: Citrination API Reference

language_tabs:
  - python

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Citrination API Documentation! The Citrination API can be used to query our extensive database of material properties in a couple of exciting ways.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authenticate, you must obtain your API key from Citrine Informatics

```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
```

```
# Authentication tokens must be applied to the headers of any requests made to the Citrination API
curl "api_endpoint_here"
  -H "X-API-Key: your-unique-api-key"
```

Citrination uses API keys to allow access to the API. You can register a new Kittn API key by logging in to your instance of Citrination, clicking on your username and selecting 'Account'.

Citrination expects for the API key to be included in all API requests to the server in a header that looks like the following:

`X-API-Key: your-unique-api-key`

# Materials Data

## Search Data
```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
client.search(term='GaN', from_page=0, per_page=10)
```

```shell
curl --data "term=GaN&from=0&per_page=10"
 "https://your-site.citrination.com/api/measurements/search"
  -H "X-API-Key: your-api-key"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": [
        {
          "citationFull": "Dr. Materials Band Gap Paper",
          "citationShort": "Dr. Materials Band Gap Paper",
          "url": null
        }
      ],
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": null,
      "minimif_id": "129353",
      "mif_id": 213,
      "permalink": "/uploads/213/samples/gan-band-gap"
    },
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": null,
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": null,
      "minimif_id": "129349",
      "mif_id": 212,
      "permalink": "/uploads/212/samples/gan-band-gap"
    }
  ],
  "time": 14,
  "hits": 2
}
```

This endpoint searches data based on text input to the term field. We index chemical formulas in a variety of ways, and the term field in this method is very flexible. For example, you could search "band gap of gallium nitride", or "ternary oxides" and get back a variety of interesting results, ranked according to our proprietary scoring algorithm.

### HTTP Request
`POST https://your-site.citrination.com/api/measurements/search`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
term | true | The basic search query
formula | false | Limit the search results by the chemical formula entered here
contributor | false | Limit the search results by the name of the person that contributed the data
reference | false | Limit the search results by the original reference for the data
min_measurement | false | Minimum decimal value for property value
max_measurement | false | Maximum decimal value for property value
from | false | If using pagination, set the starting record. Defaults to 0
per_page | false | If using pagination, sets how many records to return. Defaults to 10


<aside class="success">
Don't forget your API key!
</aside>

## Filter Data

```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
client.filter(formula='GaN', from_page=0, per_page=10)
```

```shell
curl --data "formula=GaN&from=0&per_page=10"
 "http://your-site.citrination.com/api/samples/filter"
  -H "X-API-Key: your-api-key"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": [
        {
          "citationFull": "Dr. Materials Band Gap Paper",
          "citationShort": "Dr. Materials Band Gap Paper",
          "url": null
        }
      ],
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": null,
      "minimif_id": "129353",
      "mif_id": 213,
      "permalink": "/uploads/213/samples/gan-band-gap"
    },
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": null,
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": null,
      "minimif_id": "129349",
      "mif_id": 212,
      "permalink": "/uploads/212/samples/gan-band-gap"
    }
  ],
  "time": 14,
  "hits": 2
}
```

Filtering is a bit like searching, but for when you want a limited set of exact matches of data instead of a "friendlier" term search. Filtering on "GaN", for example, will not return results like GaN2. The API is very similar, but there is no term field. Note that our samples index uses the same underlying data as our measurement index, but the return values are slightly different.

### HTTP Request
`POST https://your-site.citrination.com/api/samples/filter`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
formula | false | Limit the search results by the chemical formula entered here
contributor | false | Limit the search results by the name of the person that contributed the data
reference | false | Limit the search results by the original reference for the data
min_measurement | false | Minimum decimal value for property value
max_measurement | false | Maximum decimal value for property value
from | false | If using pagination, set the starting record. Defaults to 0
per_page | false | If using pagination, sets how many records to return. Defaults to 10


<aside class="success">
Don't forget your API key!
</aside>

## Search a specific data set  
```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
client.search(term='GaN', from_page=0, per_page=10, data_set_id=213)
```

```shell
curl --data "term=GaN&from=0&per_page=10"
 "https://your-site.citrination.com/api/data_sets/213/measurements/search"
  -H "X-API-Key: your-api-key"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": [
        {
          "citationFull": "Dr. Materials Band Gap Paper",
          "citationShort": "Dr. Materials Band Gap Paper",
          "url": null
        }
      ],
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": "experimental",
      "minimif_id": "129353",
      "mif_id": 213,
      "permalink": "/uploads/213/samples/gan-band-gap"
    }
  ],
  "time": 14,
  "hits": 2
}
```
This API is identical to the main search API, but limited to a particular data_set. You will notice that the search results include a "mif_id" field. This ID is the value to supply to the data_sets endpoint when searching.

This endpoint searches data based on text input to the term field. We index chemical formulas in a variety of ways, and the term field in this method is very flexible. For example, you could search "band gap of gallium nitride", or "ternary oxides" and get back a variety of interesting results, ranked according to our proprietary scoring algorithm.

<aside class="warning">
You can use a ? in the 'term' parameter to retrieve all structured data for a given sample.
</aside>

### HTTP Request

`POST https://your-site.citrination.com/api/data_sets/<id>/measurements/search`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the sample to search

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
term | true | The basic search query
formula | false | Limit the search results by the chemical formula entered here
contributor | false | Limit the search results by the name of the person that contributed the data
reference | false | Limit the search results by the original reference for the data
min_measurement | false | Minimum decimal value for property value
max_measurement | false | Maximum decimal value for property value
from | false | If using pagination, set the starting record. Defaults to 0
per_page | false | If using pagination, sets how many records to return. Defaults to 10


<aside class="success">
Don't forget your API key!
</aside>

## Filter a specific data set  
```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
client.filter(formula='GaN', from_page=0, per_page=10, data_set_id=213)
````

```shell
curl --data "formula=GaN&from=0&per_page=10"
 "https://your-site.citrination.com/api/data_sets/213/samples/filter"
  -H "X-API-Key: your-api-key"
  -H "Content-Type: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "results": [
    {
      "formula": "GaN",
      "property_name": "Band gap",
      "conditions": [],
      "references": [
        {
          "citationFull": "Dr. Materials Band Gap Paper",
          "citationShort": "Dr. Materials Band Gap Paper",
          "url": null
        }
      ],
      "display_contributor": "Dr. Materials",
      "measurement": {
        "name": "Band gap",
        "units": "eV",
        "type": "experimental",
        "value_display": "3.4",
        "print_value": "3.4 eV"
      },
      "plot_types": [],
      "data_type": "experimental",
      "minimif_id": "129353",
      "mif_id": 213,
      "permalink": "/uploads/213/samples/gan-band-gap"
    }
  ],
  "time": 14,
  "hits": 2
}
```
This API is identical to the main filter API, but limited to a particular data_set. You will notice that the search results include a "mif_id" field. This ID is the value to supply to the data_sets endpoint when searching.

Filtering is a bit like searching, but for when you want a limited set of exact matches of data instead of a "friendlier" term search. Filtering on "GaN", for example, will not return results like GaN2. The API is very similar, but there is no term field. Note that our samples index uses the same underlying data as our measurement index, but the return values are slightly different..

### HTTP Request

`POST https://your-site.citrination.com/api/data_sets/<id>/samples/filter`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the data_set to filter

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
formula | false | Limit the search results by the chemical formula entered here
contributor | false | Limit the search results by the name of the person that contributed the data
reference | false | Limit the search results by the original reference for the data
min_measurement | false | Minimum decimal value for property value
max_measurement | false | Maximum decimal value for property value
from | false | If using pagination, set the starting record. Defaults to 0
per_page | false | If using pagination, sets how many records to return. Defaults to 10


<aside class="success">
Don't forget your API key!
</aside>

## Upload Data
```python
from citrination_client import CitrinationClient
client = CitrinationClient('your-unique-api-key', 'https://yoursite.citrination.com')
client.upload(name='My Published Paper', description='Band Gaps of My Favorite Compounds')
```

You can upload data using the python client, but not directly through HTTP at this time.
Uploading data will start it off in our data processing pipeline, and in a few minutes time
it will be available on the web via https://your-site.citrination.com/data_uploads

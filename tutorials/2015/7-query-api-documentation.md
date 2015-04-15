---
title: Query API Documentation
categories: tutorials
tags: search-functionality, api-scripting
author: jaswsinc
github-issue: https://github.com/websharks/wp-kb-articles-kb/issues/7
---

With WP KB Articles Pro installed, your site is equipped with the WPKBA Query API; making it possible for you (or anyone) to list and/or search through KB articles that you've published for the world to see.

---

## Query API Endpoint

```text
http://YOURDOMAIN.com/wp-kb-articles/api/query
	e.g., http://wpkbarticles.com/wp-kb-articles/api/query
```

_**Note:** Be sure to change `YOURDOMAIN.com`_

---

## Query API Code Samples

<iframe src="//api.apiembed.com/?source=https://raw.githubusercontent.com/websharks/wp-kb-articles-kb/master/query-api.har.json&targets=php:curl,php:http1,php:http2,javascript:jquery,javascript:xhr,shell:curl,shell:wget,shell:httpie,node:request,node:native,node:unirest,ruby:native,python:requests,python:python3,java:okhttp,java:unirest,go:native,ocaml:cohttp,swift:nsurlsession,objc:nsurlsession,csharp:restsharp,c:libcurl" frameborder="0" scrolling="no" width="100%" height="575px" seamless></iframe>

---

## GET `/wp-kb-articles/api/query` Parameters

<div class="li-margins"></div>

- `page` (integer) - Optional page number to return results for. Default: `1`

- `per_page` (integer) - Optional per-page restriction; i.e., max results per page. Default: `25`

- `orderby` (string|array) - Optional comma-delimited list (or associative array) of columns to order by. This should be listed in order of precedence that you prefer.

  **Default orderby list is as follows:**
  ```text
  relevance:DESC,popularity:DESC,visits:DESC,comment_count:DESC,views:DESC,date:DESC
  ```
  Each item in the list should contain `column:[ASC|DESC]`.

  _Note: If you pass an associative array, the column name is the array key, and the value for that key is either `ASC` or `DESC`_

- `author` (string|integer|array) - Optional author filter. This can be a comma-delimited list of author usernames and/or author IDs. Or, instead of passing a comma-delimited list, you can pass an array of author usernames and/or IDs.

- `category` (string|integer|array) - Optional category filter. This can be a comma-delimited list of category slugs and/or category IDs. Or, instead of passing a comma-delimited list, you can pass an array of category slugs and/or IDs.

- `tag` (string|integer|array) - Optional tag filter. This can be a comma-delimited list of tag slugs and/or tag IDs. Or, instead of passing a comma-delimited list, you can pass an array of tag slugs and/or IDs.

- `q` (string) - Optional search query. This supports [MySQL boolean search syntax](https://dev.mysql.com/doc/refman/5.5/en/fulltext-boolean.html) also.

- `trending_days` (integer) - Optional trending timeframe. This is only applicable if you have a `trending` category, and only if you filter the results by that `trending` category; i.e., if you include `trending` in the `category` list.

  In a trending view, articles visited in the last `trending_days` are sorted by visits DESC. By default, WPKBA will look at the last 7 days. You can alter this behavior by passing this query parameter.

- `snippet_size` (integer) - Applicable only when you are performing a search; i.e., when snippets are returned for each result. The default snippet length is `100` characters (plus the length of the search term found in the snippet). You can increase or decrease the size of each snippet if you'd like to display a larger piece of text.

- `expand` (boolean|string|array) -  This can be a boolean-ish value (e.g., `1|on|yes|true` or `0|off|no|false`), or a comma-delimited list of properties to expand. By default, all properties are expanded; i.e., the JSON response that you receive will include all available data. However, if you would like to optimize by retrieving only specific details you can customize the list of expanded properties.

  A `true` value (the default) indicates that _all_ properties should be expanded. A value of `false` indicates that no properties should be expanded; i.e., that only the `id`, `title`, and `url` should be returned in a set of `results`—nothing more.

  **Expandable Property Identifiers**

  - `args` To include a list of validated/sanitized arguments interpreted by the API. These are based on the query parameters that you passed in your original request.
  - `pagination` To include an object containing pagination-specific property values; e.g., `total_results`, `total_pages`, `per_page`, `current_page`.
  - `results` To force _all_ result properties to be expanded. If you include `results` in an array or comma-delimited list there is no reason to also include `result.*`. In other words, `results` indicates that you want all result properties to be expanded. Thus, no need to be specific about which `result.[property]` names you want to expand when this is present.
  - `result.snippet` To include a snippet property for each result.
  - `result.relevance` To include the relevance score for each result.
  - `result.hearts` To include the total hearts for each result.
  - `result.visits` To include the total visits for each result.
  - `result.views` To include the total pageviews for each result.
  - `result.last_view_time` To include the last view time for each result.
  - `result.time` To include the publication time for each result.
  - `result.author` To include an object containing author-related properties for each result.
  - `result.categories` To include an array of categories for each result.
  - `result.tags` To include an array of tags for each result.

---

## Example Output Response

`Content-Type: application/json`

```json
{
    "results": [
        {
            "id": 312,
            "title": "Setting up Two Membership Types (Professional / Student) with Two Sub-Types (Gold / Bronze) and a 3-Day Free Trial",
            "url": "http://wordpress.vm/kb-article/setting-up-two-membership-types-professional-student-with-two-sub-types-gold-bronze-and-a-3-day-free-trial",
            "snippet": "with Gold and Bronze sub-types. I want to offer a free 3-day trial during which each account type will h",
            "relevance": 2,
            "hearts": 0,
            "visits": 0,
            "views": 0,
            "last_view_time": 0,
            "time": 1428578662,
            "author": {
                "id": 1,
                "login": "jaswsinc",
                "nicename": "jaswsinc",
                "nickname": "jasnick",
                "first_name": "Jason",
                "last_name": "Caldwell",
                "display_name": "Caldwell Jason"
            },
            "categories": [
                {
                    "id": 52,
                    "slug": "tutorials",
                    "name": "tutorials"
                }
            ],
            "tags": [
                {
                    "id": 72,
                    "slug": "business-models",
                    "name": "business-models"
                },
                {
                    "id": 41,
                    "slug": "custom-capabilities",
                    "name": "custom-capabilities"
                },
                {
                    "id": 5,
                    "slug": "pro-forms",
                    "name": "pro-forms"
                },
                {
                    "id": 37,
                    "slug": "restriction-options",
                    "name": "restriction-options"
                }
            ]
        },
        {
            "id": 202,
            "title": "Can a Free Subscriber register more than once?",
            "url": "http://wordpress.vm/kb-article/can-a-free-subscriber-register-more-than-once",
            "snippet": "ot be able to \"re-register\" for another free account using an email address that matches their",
            "relevance": 2,
            "hearts": 0,
            "visits": 0,
            "views": 0,
            "last_view_time": 0,
            "time": 1428578044,
            "author": {
                "id": 1,
                "login": "jaswsinc",
                "nicename": "jaswsinc",
                "nickname": "jasnick",
                "first_name": "Jason",
                "last_name": "Caldwell",
                "display_name": "Caldwell Jason"
            },
            "categories": [
                {
                    "id": 4,
                    "slug": "questions",
                    "name": "questions"
                }
            ],
            "tags": [
                {
                    "id": 7,
                    "slug": "login-registration",
                    "name": "login-registration"
                },
                {
                    "id": 5,
                    "slug": "pro-forms",
                    "name": "pro-forms"
                }
            ]
        }
    ],
    "args": {
        "page": 1,
        "per_page": 2,
        "orderby": {
            "relevance": "DESC",
            "popularity": "DESC",
            "visits": "DESC",
            "comment_count": "DESC",
            "views": "DESC",
            "date": "DESC"
        },
        "author": [],
        "category": [],
        "category_no_tp": [],
        "tag": [],
        "q": "free",
        "trending_days": 7,
        "snippet_size": 100,
        "strings": {
            "page": "1",
            "per_page": "2",
            "orderby": "relevance:DESC,popularity:DESC,visits:DESC,comment_count:DESC,views:DESC,date:DESC",
            "author": "",
            "category": "",
            "category_no_tp": "",
            "tag": "",
            "q": "free",
            "trending_days": "7",
            "snippet_size": "100",
            "expand": "true"
        },
        "expand": true
    },
    "pagination": {
        "total_results": 30,
        "total_pages": 15,
        "per_page": 2,
        "current_page": 1
    }
}
```

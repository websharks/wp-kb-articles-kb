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
  "swagger": "2.0",
  "info": {
    "version": "",
    "title": "WPKBA Query API",
    "description": "List and/or search KB articles.",
    "license": {
      "name": "MIT",
      "url": "http://github.com/gruntjs/grunt/blob/master/LICENSE-MIT"
    }
  },
  "host": "wpkbarticles.com",
  "basePath": "/wp-kb-articles/api/v1",
  "securityDefinitions": {},
  "schemes": [
    "http"
  ],
  "consumes": [
    "application/json"
  ],
  "produces": [
    "application/json"
  ],
  "paths": {
    "/query": {
      "post": {
        "description": "List and/or search KB articles.",
        "tags": [
          "WPKBA Query"
        ],
        "operationId": "Query",
        "produces": [
          "application/json"
        ],
        "parameters": [
          {
            "name": "callback",
            "in": "query",
            "required": false,
            "type": "string",
            "description": "For JSONP requests only. This should be the name of a JavaScript callback function."
          },
          {
            "name": "wp_kb_articles[query_api][page]",
            "in": "form",
            "required": true,
            "type": "integer",
            "format": "int64",
            "description": "Page number to retrieve results for.",
            "default": "1"
          }
        ],
        "responses": {
          "200": {
            "schema": {
              "$ref": "#/definitions/response"
            },
            "description": ""
          },
          "400": {
            "description": "Bad Request"
          }
        },
        "security": {}
      }
    }
  },
  "definitions": {
    "response": {
      "required": [
        "results",
        "args",
        "pagination"
      ],
      "title": "response",
      "type": "object",
      "properties": {
        "results": {
          "Type": "array",
          "Description": "Array of all results; i.e., KB articles.",
          "Items": {
            "$ref": "#/definitions/result"
          }
        },
        "args": {
          "$ref": "#/definitions/args",
          "Description": "Based on input parameters; this is a list of parsed/validated arguments that were used to satisfy the request."
        },
        "pagination": {
          "$ref": "#/definitions/pagination",
          "Description": "Pagination-related properties; e.g., total_results, per_page, current_page, etc."
        }
      },
      "description": "Full response object model."
    },
    "pagination": {
      "required": [
        "total_results",
        "total_pages",
        "per_page",
        "current_page"
      ],
      "title": "pagination",
      "type": "object",
      "properties": {
        "total_results": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Total results available."
        },
        "total_pages": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Total pages available."
        },
        "per_page": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Results per page."
        },
        "current_page": {
          "Type": "integer",
          "Format": "int64",
          "Description": "The current page."
        }
      },
      "description": "Pagination-related properties; e.g., total_results, per_page, current_page, etc."
    },
    "result": {
      "required": [
        "id",
        "title",
        "url",
        "snippet",
        "relevance",
        "hearts",
        "visits",
        "views",
        "last_view_time",
        "time",
        "author",
        "categories",
        "tags"
      ],
      "title": "result",
      "type": "object",
      "properties": {
        "id": {
          "Type": "integer",
          "Format": "int64",
          "Description": "KB article ID in WordPress."
        },
        "title": {
          "Type": "string",
          "Description": "KB article title."
        },
        "url": {
          "Type": "string",
          "Description": "KB article URL."
        },
        "snippet": {
          "Type": "string",
          "Description": "A contextual snippet. Filled only when a search was performed."
        },
        "relevance": {
          "Type": "integer",
          "Format": "int64",
          "Description": "A relevance score. Filled only when a search is performed."
        },
        "hearts": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Popularity; i.e., the total number of hearts the article has."
        },
        "visits": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Total number of unique visitors."
        },
        "views": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Total pageviews; i.e., hits."
        },
        "last_view_time": {
          "Type": "integer",
          "Format": "int64",
          "Description": "UTC timestamp indicating last view time. This is 0 if never viewed."
        },
        "time": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Publish date, expressed as UTC timestamp."
        },
        "author": {
          "$ref": "#/definitions/author",
          "Description": "Article author-related properties."
        },
        "categories": {
          "Type": "array",
          "Description": "An array of all categories the article is in.",
          "Items": {
            "$ref": "#/definitions/category"
          }
        },
        "tags": {
          "Type": "array",
          "Description": "An array of all tags the article has.",
          "Items": {
            "$ref": "#/definitions/tag"
          }
        }
      },
      "description": "A result object."
    },
    "author": {
      "required": [
        "id",
        "login",
        "nicename",
        "nickname",
        "first_name",
        "last_name",
        "display_name"
      ],
      "title": "author",
      "type": "object",
      "properties": {
        "id": {
          "Type": "integer",
          "Format": "int64",
          "Description": "WordPress user ID."
        },
        "login": {
          "Type": "string",
          "Description": "WordPress username."
        },
        "nicename": {
          "Type": "string",
          "Description": "WordPress nicename."
        },
        "nickname": {
          "Type": "string",
          "Description": "WordPress nickname."
        },
        "first_name": {
          "Type": "string",
          "Description": "First name."
        },
        "last_name": {
          "Type": "string",
          "Description": "Last name."
        },
        "display_name": {
          "Type": "string",
          "Description": "Display name."
        }
      },
      "description": "Author object model."
    },
    "category": {
      "required": [
        "id",
        "slug",
        "name"
      ],
      "title": "category",
      "type": "object",
      "properties": {
        "id": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Category term ID in WordPress."
        },
        "slug": {
          "Type": "string",
          "Description": "Category slug/identifier."
        },
        "name": {
          "Type": "string",
          "Description": "Category name/label."
        }
      },
      "description": "Category object model."
    },
    "tag": {
      "required": [
        "id",
        "slug",
        "name"
      ],
      "title": "tag",
      "type": "object",
      "properties": {
        "id": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Tag term ID in WordPress."
        },
        "slug": {
          "Type": "string",
          "Description": "Tag slug/identifier."
        },
        "name": {
          "Type": "string",
          "Description": "Tag name/label."
        }
      },
      "description": "Tag object model."
    },
    "args": {
      "required": [
        "page",
        "per_page",
        "orderby",
        "author",
        "category",
        "category_no_tp",
        "tag",
        "q",
        "trending_days",
        "snippet_size",
        "expand"
      ],
      "title": "args",
      "type": "object",
      "properties": {
        "page": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Current page number.",
          "Default": "1"
        },
        "per_page": {
          "Type": "integer",
          "Format": "int64",
          "Description": "Number of results per page.",
          "Default": "25"
        },
        "orderby": {
          "Type": "object",
          "Description": "Associative orderby object. Property names correlate with data columns, and each value is either ASC or DESC.",
          "Default": "relevance:DESC,popularity:DESC,visits:DESC,comment_count:DESC,views:DESC,date:DESC"
        },
        "author": {
          "Type": "array",
          "Description": "If filtered by author, an array of those author IDs.",
          "Items": {
            "type": "integer",
            "format": "int64"
          }
        },
        "category": {
          "Type": "array",
          "Description": "If filtered by category, an array of those category IDs.",
          "Items": {
            "type": "integer",
            "format": "int64"
          }
        },
        "category_no_tp": {
          "Type": "array",
          "Description": "If filtered by category, an array of those category IDs; minus trending/popular categories which are considered special in the WPKBA plugin for WordPress.",
          "Items": {
            "type": "integer",
            "format": "int64"
          }
        },
        "tag": {
          "Type": "array",
          "Description": "If filtered by tag, an array of those tag IDs.",
          "Items": {
            "type": "integer",
            "format": "int64"
          }
        },
        "q": {
          "Type": "string",
          "Description": "If searching, the search terms in the request."
        },
        "trending_days": {
          "Type": "integer",
          "Format": "int64",
          "Description": "The number of days used in a trending view timeframe.",
          "Default": "7"
        },
        "snippet_size": {
          "Type": "integer",
          "Format": "int64",
          "Description": "The length of each contextual snippet.",
          "Default": "100"
        },
        "expand": {
          "Type": "array",
          "Description": "Boolean, string, or an array of strings.",
          "Items": {
            "type": "string"
          },
          "Default": "true"
        }
      },
      "description": "Based on input parameters; this is a list of parsed/validated arguments that were used to satisfy the request."
    }
  }
}
```

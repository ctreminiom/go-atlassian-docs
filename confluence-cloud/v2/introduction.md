# 🏔 Introduction

## Overview

The _go-atlassian_ `confluence/v2` module provides a set of functions and types for interacting with the new Confluence Cloud V2 API endpoints.

It offers two main improvements over previous versions: **endpoint specialization** and **lower request latency**. Endpoints that have specific, restricted scope behave predictably, are easier to optimize, and are compatible with our commitment to granular OAuth 2.0 scopes.

{% embed url="https://blog.developer.atlassian.com/the-confluence-cloud-rest-api-v2-brings-major-performance-improvements/" %}

### The end of content <a href="#the-end-of-content" id="the-end-of-content"></a>

Confluence has numerous types of publishable data – pages, blogposts, comments, and attachments – each of which differ substantially. We consider each type to be distinct, in the same way that pages and spaces are distinct. To reflect this distinction, the version 2 API offers separate endpoints for each data type. It does not use the term `Content`, and it does not offer endpoints for fetching or manipulating `Content` as a broad concept.

### The end of expansions <a href="#the-end-of-expansions" id="the-end-of-expansions"></a>

In the REST API V2, response payloads do not contain nested data types that are served by other endpoints; instead, any references to different data types are identifiers that can be passed to separate API calls to fetch the corresponding data. Because of this, REST API V2 does not support expansions or the `expand` query parameter.

Expansions are contrary to the concept of specialized endpoints, since they enable fetching arbitrary data types from a single endpoint. Endpoints without expansions enable lower request latency, since database calls are much more predictable and easier to optimize.

### Cursor pagination and lower latency <a href="#cursor-pagination-and-lower-latency" id="cursor-pagination-and-lower-latency"></a>

The REST API V2 uses [cursor-based pagination](https://jsonapi.org/profiles/ethanresnick/cursor-pagination/) instead of the offset-based pagination present in REST API V1. Cursor-based pagination provides two main benefits:

* Cursor-paginated requests have substantially better latency than offset-paginated requests, especially when comparing high-offset requests with the equivalent cursor-paginated requests.
* Adding or deleting data during cursor-based pagination does not result in missing data. In offset-based pagination, on the other hand, adding or removing an item causes the indices of all following items to increase or decrease by one; if this occurs while a client is paginating, an item will be included again or missing from the client's next page.

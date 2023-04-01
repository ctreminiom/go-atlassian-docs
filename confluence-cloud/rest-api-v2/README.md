# 🔊 REST API V2

{% embed url="https://blog.developer.atlassian.com/the-confluence-cloud-rest-api-v2-brings-major-performance-improvements/" %}

Last October, we released [the first REST API V2 endpoints for Confluence Cloud](https://community.developer.atlassian.com/t/release-of-v2-confluence-rest-api-for-pages-and-blogposts-experimental/62164), which enabled developers to fetch pages and blogposts. We’ve continued to expand the surface area of the V2 API, and we are now happy to announce the release of V2 endpoints related to the following data types:

* attachments
* blogposts
* comments
* custom content
* labels
* pages
* spaces
* space properties
* versions

These endpoints support Connect scopes and granular OAuth 2.0 scopes.

The REST API V2 offers two main improvements over previous versions: **endpoint specialization** and **lower request latency**. Endpoints that have specific, restricted scope behave predictably, are easier to optimize, and are compatible with our commitment to granular OAuth 2.0 scopes.

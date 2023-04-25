# 🌊 V2

{% embed url="https://blog.developer.atlassian.com/the-confluence-cloud-rest-api-v2-brings-major-performance-improvements/" %}

Last October, we released [the first REST API V2 endpoints for Confluence Cloud](https://community.developer.atlassian.com/t/release-of-v2-confluence-rest-api-for-pages-and-blogposts-experimental/62164), which enabled developers to fetch pages and blogposts. We’ve continued to expand the surface area of the V2 API, and we are now happy to announce the release of V2 endpoints related to the following data types:

<table><thead><tr><th></th><th data-type="content-ref"></th></tr></thead><tbody><tr><td>pages</td><td><a href="page.md">page.md</a></td></tr><tr><td>spaces</td><td></td></tr><tr><td>attachments</td><td></td></tr><tr><td>blogposts</td><td></td></tr><tr><td>comments</td><td></td></tr><tr><td>custom-content</td><td></td></tr><tr><td>labels</td><td></td></tr><tr><td>spaces</td><td></td></tr><tr><td>space properties</td><td></td></tr><tr><td>versions</td><td></td></tr></tbody></table>

These endpoints support Connect scopes and granular OAuth 2.0 scopes.

The REST API V2 offers two main improvements over previous versions: **endpoint specialization** and **lower request latency**. Endpoints that have specific, restricted scope behave predictably, are easier to optimize, and are compatible with our commitment to granular OAuth 2.0 scopes.

{% embed url="https://developer.atlassian.com/cloud/confluence/changelog/#CHANGE-742" %}
Changelog API's
{% endembed %}

# 🧺 Attachments

## Get attachment by id

Get returns a specific attachment

<table><thead><tr><th>Name</th><th>Description</th><th data-type="select" data-multiple>Properties</th></tr></thead><tbody><tr><td><strong>attachment</strong></td><td>The ID of the attachment to be returned. </td><td></td></tr><tr><td><strong>version</strong></td><td>Allows you to retrieve a previously published version. Specify the previous version's number to retrieve its details.</td><td></td></tr><tr><td><strong>serialize-ids-as-strings</strong></td><td>Due to JavaScript's max integer representation of 2^53-1, the type of any IDs returned in the response body for this endpoint will be changed from a numeric type to a string type at the end of the deprecation period</td><td></td></tr></tbody></table>



## Get attachments by type

Gets returns the attachments of specific entity type.

* You can extract the attachments for blog-posts,custom-contents, labels and pages
* Depending on the entity type, the endpoint will change based on the entity type.
* Valid entityType values: <mark style="color:purple;">**blogposts**</mark>, <mark style="color:orange;">**custom-content**</mark>, <mark style="color:green;">**labels**</mark>, <mark style="color:red;">**pages**</mark>
* The number of results is limited by the limit parameter and additional results (if available) will be available through the next URL present in the Link response header.

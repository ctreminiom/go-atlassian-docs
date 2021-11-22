---
description: >-
  An agile epic is a body of work that can be broken down into specific tasks
  (called user stories) based on the needs/requests of customers or end-users.
---

# 📈 Epics

### Get epic <a href="api-agile-1-0-epic-epicidorkey-get" id="api-agile-1-0-epic-epicidorkey-get"></a>

Returns the epic for a given epic ID. This epic will only be returned if the user has permission to view it.

{% hint style="warning" %}
**Note:** This operation does not work for epics in next-gen projects.
{% endhint %}

{% embed url="https://gist.github.com/ctreminiom/55b6c5d3f9e29b69088b86aca5669d85" %}

### Get issues for epic

Returns all issues that belong to the epic, for the given epic ID. This only includes issues that the user has permission to view. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% hint style="warning" %}
**Note:** If you are querying a next-gen project, do not use this operation. Instead, search for issues that belong to an epic by using the Search for issues using JQL operation in the Jira platform REST API.&#x20;
{% endhint %}

{% hint style="warning" %}
Build your JQL query using the parent clause. For more information on the parent JQL field, see Advanced searching.
{% endhint %}

{% embed url="https://gist.github.com/ctreminiom/fab5aaae6608469e3f7dc2ca4feb6fd7" %}
\

{% endembed %}

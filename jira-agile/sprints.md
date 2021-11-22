---
description: >-
  A sprint is a short, time-boxed period when a scrum team works to complete a
  set amount of work. Sprints are at the very heart of scrum and agile
  methodologies, and getting sprints right will help you
---

# 🗓 Sprints

### Create sprint <a href="create-print" id="create-print"></a>

Creates a future sprint. Sprint name and origin board id are required. Start date, end date, and goal are optional.

{% embed url="https://gist.github.com/ctreminiom/b527d723de9e8a04f49390a1e8240e76" %}

### Get sprint

Returns the sprint for a given sprint ID. The sprint will only be returned if the user can view the board that the sprint was created on, or view at least one of the issues in the sprint.

{% embed url="https://gist.github.com/ctreminiom/23440f4e540913af2f93be21f2e432b1" %}

### Update sprint

Performs a full update of a sprint. A full update means that the result will be exactly the same as the request body. Any fields not present in the request JSON will be set to null.

{% embed url="https://gist.github.com/ctreminiom/8a4b85fefaa170ab5cef33919e39fd76" %}

### Partially update sprint

Performs a partial update of a sprint. A partial update means that fields not present in the request JSON will not be updated.

{% embed url="https://gist.github.com/ctreminiom/7b8c576c2056aee3edc355214430684d" %}

### Get issues for sprint <a href="get-issues-for-sprint" id="get-issues-for-sprint"></a>

Returns all issues in a sprint, for a given sprint ID. This only includes issues that the user has permission to view. By default, the returned issues are ordered by rank.

{% embed url="https://gist.github.com/ctreminiom/c314e7bf2161430c1bcef1c9de8e1436" %}

### Start sprint

{% embed url="https://gist.github.com/ctreminiom/13901be6dd7eac18f16d0d5094112376" %}

### Close Sprint

{% embed url="https://gist.github.com/ctreminiom/537e6078aebb9a9df5598c59952b6b8b" %}


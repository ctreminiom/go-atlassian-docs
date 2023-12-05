---
cover: ../../.gitbook/assets/strategic-planning-vs-cognitive-biases-1560x760.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🏣 Priorities

An issue's priority defines its importance in relation to other issues, so it helps your users determine which issues should be tackled first. Jira comes with a set of default priorities, which you can modify or add to. You can also choose different priorities for your projects.

![](<../../.gitbook/assets/image (12).png>)

## Get priorities

`GET /rest/api/{2-3}/priority`

Returns the list of all issue priorities, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	priorities, response, err := atlassian.Issue.Priority.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for pos, priority := range priorities {
		log.Println(pos, priority)
	}
}
```
{% endcode %}

## Get priority

`GET /rest/api/{2-3}/priority/{id}`

Returns an issue priority, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var priorityID = "1"

	priority, response, err := atlassian.Issue.Priority.Get(context.Background(), priorityID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(priority)
}
```
{% endcode %}

## Create priority

`POST /rest/api/3/priority`

Creates an issue priority.

> No implemented,  yet. Feel free to open a new PR or issue

## Set default priority

`PUT /rest/api/3/priority/default`

Sets default issue priority.

> No implemented,  yet. Feel free to open a new PR or issue

## Move priorities

`PUT /rest/api/3/priority/move`

Changes the order of issue priorities.

> No implemented,  yet. Feel free to open a new PR or issue

## Search priorities

`GET /rest/api/3/priority/search`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of priorities. The list can contain all priorities or a subset determined by any combination of these criteria:

* a list of priority IDs. Any invalid priority IDs are ignored.
* whether the field configuration is a default. This returns priorities from company-managed (classic) projects only, as there is no concept of default priorities in team-managed projects.

> No implemented,  yet. Feel free to open a new PR or issue

## Update priority

`PUT /rest/api/3/priority/{id}`

Updates an issue priority.

> No implemented,  yet. Feel free to open a new PR or issue

## Delete priority

`DELETE /rest/api/3/priority/{id}`

Deletes an issue priority.

{% hint style="info" %}
This operation is [asynchronous](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#async). Follow the `location` link in the response to determine the status of the task and use [Get task](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-tasks/#api-rest-api-3-task-taskid-get) to obtain subsequent updates.
{% endhint %}

> No implemented,  yet. Feel free to open a new PR or issue

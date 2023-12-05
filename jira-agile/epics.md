---
cover: ../.gitbook/assets/whats-your-chronotype-1560x760.jpg
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

# 📈 Epics

## Get Epic

`GET /rest/agile/1.0/epic/{epicIdOrKey}`

Returns the epic for a given epic ID. This epic will only be returned if the user has permission to view it.

{% hint style="warning" %}
**Note:** This operation does not work for epics in next-gen projects.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	epic, response, err := atlassian.Epic.Get(context.Background(), "KP-16")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(epic)
}
```
{% endcode %}

## Get Issues for epic

`GET /rest/agile/1.0/epic/{epicIdOrKey}/issue`

Returns all issues that belong to the epic, for the given epic ID. This only includes issues that the user has permission to view. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% hint style="warning" %}
**Note:** If you are querying a next-gen project, do not use this operation. Instead, search for issues that belong to an epic by using the Search for issues using JQL operation in the Jira platform REST API.&#x20;
{% endhint %}

{% hint style="warning" %}
Build your JQL query using the parent clause. For more information on the parent JQL field, see Advanced searching.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		options = &models.IssueOptionScheme{
			JQL:           "project = KP",
			ValidateQuery: true,
			Fields:        []string{"status", "issuetype", "summary"},
			Expand:        []string{"changelog"},
		}

		epicKey   = "KP-16"
		startAt   = 0
		maxResult = 50
	)

	issues, response, err := atlassian.Epic.Issues(context.Background(), epicKey, options, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issues.Issues {
		log.Println(issue.Key)
	}
}
```
{% endcode %}

## Move issues to Epic

`POST /rest/agile/1.0/epic/{epicIdOrKey}/issue`

Move moves issues to an epic, for a given epic id. Issues can be only in a single epic at the same time. That means that already assigned issues to an epic, will not be assigned to the previous epic anymore. The user needs to have the edit issue permission for all issue they want to move and to the epic. The maximum number of issues that can be moved in one operation is 50.&#x20;

{% hint style="info" %}
This operation does not work for epics in next-gen projects.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	response, err := atlassian.Epic.Move(context.Background(), "KP-16", []string{"DUMMY-1"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

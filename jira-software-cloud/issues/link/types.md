---
cover: ../../../.gitbook/assets/in-defense-of-meetings-2240x1090-1-1560x760.jpg
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

# 🖇️ Types

This resource represents issue link types. Use it to get, create, update, and delete link issue types as well as get lists of all link issue types.

## Get issue link types

`GET /rest/api/{2-3}/issueLinkType`

Returns a list of all issue link types, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	types, response, err := atlassian.Issue.Link.Type.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, value := range types.IssueLinkTypes {
		log.Println(value)
	}

}
```
{% endcode %}

## Create issue link type

`POST /rest/api/{2-3}/issueLinkType`

Creates an issue link type. Use this operation to create descriptions of the reasons why issues are linked.&#x20;

The issue link type consists of a name and descriptions for a link's inward and outward relationships, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := models.LinkTypeScheme{
		Inward:  "Clone/Duplicated by",
		Name:    "Clone/Duplicate",
		Outward: "Clone/Duplicates",
	}

	issueLinkType, response, err := atlassian.Issue.Link.Type.Create(context.Background(), &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueLinkType)
}
```
{% endcode %}

## Get issue link type

`GET /rest/api/{2-3}/issueLinkType/{issueLinkTypeId}`

Returns an issue link type, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueLinkType, response, err := atlassian.Issue.Link.Type.Get(context.Background(), "10000")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueLinkType)
}
```
{% endcode %}

## Update issue link type

`PUT /rest/api/{2-3}/issueLinkType/{issueLinkTypeId}`

Updates an issue link type, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := models.LinkTypeScheme{
		Inward:  "Clone/Duplicated by - Updated",
		Name:    "Clone/Duplicate - Updated",
		Outward: "Clone/Duplicates - Updated",
	}

	issueLinkType, response, err := atlassian.Issue.Link.Type.Update(context.Background(), "10008", &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueLinkType)
}
```
{% endcode %}

## Delete issue link type

`DELETE /rest/api/{2-3}/issueLinkType/{issueLinkTypeId}`

Deletes an issue link type, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	response, err := atlassian.Issue.Link.Type.Delete(context.Background(), "10008")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

---
cover: ../../.gitbook/assets/artboard-2@3x-1560x760.png
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

# ⏰ SLA

## Get SLA information

`GET /rest/servicedeskapi/request/{issueIdOrKey}/sla`

This method returns all the SLA records on a customer request.

* A customer request can have zero or more SLAs. Each SLA can have recordings for zero or more "completed cycles" and zero or 1 "ongoing cycle".&#x20;
* Each cycle includes information on when it started and stopped, and whether it breached the SLA goal.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-3"
		start        = 0
		limit        = 50
	)

	slas, response, err := atlassian.Request.SLA.Gets(context.Background(), issueKeyOrID, start, limit)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, sla := range slas.Values {
		log.Println(sla)
	}
}
```
{% endcode %}

## Get SLA information by id

`GET /rest/servicedeskapi/request/{issueIdOrKey}/sla/{slaMetricId}`

This method returns the details for an SLA on a customer request.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-3"
		slaMetricID  = 1
	)

	sla, response, err := atlassian.Request.SLA.Get(context.Background(), issueKeyOrID, slaMetricID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("SLA ID", sla.ID)
	log.Println("SLA Name", sla.Name)
}
```
{% endcode %}

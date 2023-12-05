---
cover: ../../.gitbook/assets/mb-personalities_1120x545-@2xcompressed-1560x760.png
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

# 👥 Participants

## Get request participants

`GET /rest/servicedeskapi/request/{issueIdOrKey}/participant`

This method returns a list of all the participants on a customer request.

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

	participants, response, err := atlassian.Request.Participant.Gets(context.Background(), issueKeyOrID, start, limit)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, participant := range participants.Values {
		log.Println(participant.DisplayName, participant.EmailAddress)
	}
}
```
{% endcode %}

## Add request participants

`POST /rest/servicedeskapi/request/{issueIdOrKey}/participant`

This method adds participants to a customer request.

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
		accountIDs   = []string{""}
	)

	participants, response, err := atlassian.Request.Participant.Add(context.Background(), issueKeyOrID, accountIDs)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, participant := range participants.Values {
		log.Println(participant.DisplayName, participant.EmailAddress)
	}
}
```
{% endcode %}

## Remove request participants

`DELETE /rest/servicedeskapi/request/{issueIdOrKey}/participant`

This method removes participants from a customer request.

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
		accountIDs   = []string{""}
	)

	participants, response, err := atlassian.Request.Participant.Remove(context.Background(), issueKeyOrID, accountIDs)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, participant := range participants.Values {
		log.Println(participant.DisplayName, participant.EmailAddress)
	}
}
```
{% endcode %}

---
cover: ../../.gitbook/assets/leadership-principles_1120x545@2x-1560x760.png
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

# 🚫 Approval

## Get approvals

`GET /rest/servicedeskapi/request/{issueIdOrKey}/approval`

This method returns all approvals on a customer request.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
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

	var issueKey = "DESK-12"

	approvals, response, err := atlassian.Request.Approval.Gets(context.Background(), issueKey, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, customRequest := range approvals.Values {
		dataAsJson, err := json.MarshalIndent(customRequest, "", "\t")
		if err != nil {
			log.Fatal(err)
		}
		log.Println(string(dataAsJson))
	}
}
```
{% endcode %}

## Get approval by id

`GET /rest/servicedeskapi/request/{issueIdOrKey}/approval/{approvalId}`

This method returns an approval. Use this method to determine the status of an approval and the list of approvers.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
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
		issueKey   = "DESK-12"
		approvalID = 2
	)

	approvalsMembers, response, err := atlassian.Request.Approval.Get(context.Background(), issueKey, approvalID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	dataAsJson, err := json.MarshalIndent(approvalsMembers, "", "\t")
	if err != nil {
		log.Fatal(err)
	}
	log.Println(string(dataAsJson))
}
```
{% endcode %}

## Answer approval

`POST /rest/servicedeskapi/request/{issueIdOrKey}/approval/{approvalId}`

This method enables a user to **Approve** or **Decline** an approval on a customer request. The approval is assumed to be owned by the user making the call.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
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
		issueKey   = "DESK-12"
		approvalID = 2
	)

	approvalsMembers, response, err := atlassian.Request.Approval.Answer(context.Background(), issueKey, approvalID, true)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	dataAsJson, err := json.MarshalIndent(approvalsMembers, "", "\t")
	if err != nil {
		log.Fatal(err)
	}
	log.Println(string(dataAsJson))
}
```
{% endcode %}

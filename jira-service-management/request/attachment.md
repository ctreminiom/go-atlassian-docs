---
cover: >-
  ../../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
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

# 📂 Attachments

## Create attachment

`POST /rest/servicedeskapi/request/{issueIdOrKey}/attachment`

This method adds one or more temporary files (attached to the request's service desk using [servicedesk/{serviceDeskId}/attachTemporaryFile](https://developer.atlassian.com/cloud/jira/service-desk/rest/api-group-request/#api-rest-servicedeskapi-request-issueidorkey-attachment-post)) as attachments to a customer request and set the attachment visibility using the `public` flag.

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
		issueKeyOrID           = "IT-3"
		temporaryAttachmentIDs = []string{"temp910441317820424274"}
	)

	attachments, response, err := atlassian.Request.Attachment.Create(context.Background(), issueKeyOrID, temporaryAttachmentIDs, true)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(attachments)
}
```
{% endcode %}

## Get attachments for request

`GET /rest/servicedeskapi/request/{issueIdOrKey}/attachment`

This method returns all the attachments for a customer requests.

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
		issueKeyOrID = "IT-3"
		start        = 0
		limit        = 50
	)

	attachments, response, err := atlassian.Request.Attachment.Gets(context.Background(), issueKeyOrID, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, attachment := range attachments.Values {
		log.Println(attachment)
	}
}
```
{% endcode %}

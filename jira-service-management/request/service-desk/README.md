---
cover: ../../../.gitbook/assets/team_personality_tests_1120x545@2x-1560x760.jpeg
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

# ⚙ Service Desk

## Get service desks

`GET /rest/servicedeskapi/servicedesk`

This method returns all the service desks in the Jira Service Management instance that the user has permission to access. Use this method where you need a list of service desks or need to locate a service desk by name or keyword.

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
		start = 0
		limit = 50
	)

	serviceDesks, response, err := atlassian.ServiceDesk.Gets(context.Background(), start, limit)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, serviceDesk := range serviceDesks.Values {
		log.Println(serviceDesk.ID, serviceDesk.ProjectName, serviceDesk.ProjectKey)
	}
}
```
{% endcode %}

## Get service desk by id

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}`

This method returns a service desk. Use this method to get service desk details whenever your application component is passed a service desk ID but needs to display other service desk details.

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
		serviceDeskProjectID = 1
	)

	serviceDesk, response, err := atlassian.ServiceDesk.Get(context.Background(), serviceDeskProjectID)

	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(serviceDesk.ID, serviceDesk.ProjectName, serviceDesk.ProjectKey)
}
```
{% endcode %}

## Attach temporary file

`POST /rest/servicedeskapi/servicedesk/{serviceDeskId}/attachTemporaryFile`

This method adds one or more temporary attachments to a service desk, which can then be permanently attached to a customer request using [servicedeskapi/request/{issueIdOrKey}/attachment](https://developer.atlassian.com/cloud/jira/service-desk/rest/api-group-servicedesk/#api-rest-servicedeskapi-servicedesk-servicedeskid-attachtemporaryfile-post).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
	"path/filepath"
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
		serviceDeskProjectID = 1
		filePath             = "jira/sm/mocks/image.png"
	)

	absolutePath, err := filepath.Abs(filePath)
	if err != nil {
		return
	}

	reader, err := os.Open(absolutePath)
	if err != nil {
		log.Fatal(err)
	}
	defer reader.Close()

	attachments, response, err := atlassian.ServiceDesk.Attach(context.Background(), serviceDeskProjectID, filePath, reader)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, attachment := range attachments.TemporaryAttachments {
		log.Println(attachment.FileName, attachment.TemporaryAttachmentID)
	}
}
```
{% endcode %}

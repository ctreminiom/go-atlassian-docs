---
cover: ../../../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
coverY: 0
---

# 🛎 Queue

## Get queues

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/queue`

This method returns the queues in a service desk. To include a customer request count for each queue (in the `issueCount` field) in the response, set the query parameter `includeCount` to true (its default is false).

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
		serviceDeskID      = 1
		includeCount  bool = true
		start, limit  int  = 0, 50
	)

	queues, _, err := atlassian.ServiceDesk.Queue.Gets(context.Background(), serviceDeskID, includeCount, start, limit)
	if err != nil {
		log.Fatal(err)
	}

	for pos, queue := range queues.Values {
		log.Println("------------------------------------")
		log.Printf("Queue ID #%v: %v", pos+1, queue.ID)
		log.Printf("Queue Name #%v: %v", pos+1, queue.Name)
		log.Printf("Queue JQL #%v: %v", pos+1, queue.Jql)
		log.Printf("Queue Issue Count #%v: %v", pos+1, queue.IssueCount)
		log.Printf("Queue Fields #%v: %v", pos+1, queue.Fields)
		log.Println("------------------------------------")
	}
}
```
{% endcode %}

## Get queue

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/queue/{queueId}`

This method returns a specific queues in a service desk. To include a customer request count for the queue (in the `issueCount` field) in the response, set the query parameter `includeCount` to true (its default is false).

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
		serviceDeskID      = 1
		queueID            = 1
		includeCount  bool = true
	)

	queue, _, err := atlassian.ServiceDesk.Queue.Get(context.Background(), serviceDeskID, queueID, includeCount)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("------------------------------------")
	log.Printf("Queue ID:  %v", queue.ID)
	log.Printf("Queue Name: %v", queue.Name)
	log.Printf("Queue JQL: %v", queue.Jql)
	log.Printf("Queue Issue Count: %v", queue.IssueCount)
	log.Printf("Queue Fields: %v", queue.Fields)
	log.Println("------------------------------------")
}
```
{% endcode %}

## Get issues in queue

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/queue/{queueId}/issue`

This method returns the customer requests in a queue. Only fields that the queue is configured to show are returned. For example, if a queue is configured to show description and due date, then only those two fields are returned for each customer request in the queue.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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
		serviceDeskID = 1
		queueID       = 1
		start, limit  = 0, 50
	)

	issues, _, err := atlassian.ServiceDesk.Queue.Issues(context.Background(), serviceDeskID, queueID, start, limit)
	if err != nil {
		log.Fatal(err)
	}

	for _, issue := range issues.Values {
		fmt.Println(issue)
	}
}
```
{% endcode %}

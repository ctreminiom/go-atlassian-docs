---
cover: ../../.gitbook/assets/how-to-survive-your-next-work-trip@2x-100-1-1560x760.jpg
coverY: 448
---

# 📙 Request

## Get customer requests

`GET /rest/servicedeskapi/request`

This method returns all customer requests for the user executing the query.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"github.com/ctreminiom/go-atlassian/jira/sm"
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

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	options := &models.ServiceRequestOptionScheme{
		SearchTerm:        "",
		RequestOwnerships: []string{"OWNED_REQUESTS"},
		RequestStatus:     "ALL_REQUESTS",
		ApprovalStatus:    "",
		OrganizationID:    0,
		ServiceDeskID:     0,
		RequestTypeID:     0,
		Expand:            []string{"serviceDesk", "requestType", "status", "action"},
	}

	customerRequests, response, err := atlassian.Request.Gets(context.Background(), options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, customRequest := range customerRequests.Values {

		dataAsJson, err := json.MarshalIndent(customRequest, "", "\t")
		if err != nil {
			log.Fatal(err)
		}

		log.Println(string(dataAsJson))
	}
}
```
{% endcode %}

## Get customer request by id or key

`GET /rest/servicedeskapi/request/{issueIdOrKey}`

This method returns a customer request.

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
		issueKey = "DESK-11"
		expand   = []string{"serviceDesk", "requestType"}
	)

	request, response, err := atlassian.Request.Get(context.Background(), issueKey, expand)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	dataAsJson, err := json.MarshalIndent(request, "", "\t")
	if err != nil {
		log.Fatal(err)
	}

	log.Println(string(dataAsJson))
}
```
{% endcode %}

## Subscribe

`PUT /rest/servicedeskapi/request/{issueIdOrKey}/notification`

This method subscribes the user to receiving notifications from a customer request.

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
	
	response, err := atlassian.Request.Subscribe(context.Background(), "DUMMY-4")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Unsubscribe

`DELETE /rest/servicedeskapi/request/{issueIdOrKey}/notification`

This method unsubscribes the user from notifications from a customer request.

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
	response, err := atlassian.Request.Unsubscribe(context.Background(), "DUMMY-4")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.Bytes.String()))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endcode %}

## Get customer transitions

`GET /rest/servicedeskapi/request/{issueIdOrKey}/transition`

This method returns a list of transitions, the workflow processes that moves a customer request from one status to another, that the user can perform on a request.&#x20;

* Use this method to provide a user with a list if the actions they can take on a customer request.

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

	transitions, response, err := atlassian.Request.Transitions(context.Background(), issueKeyOrID, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, transition := range transitions.Values {
		log.Println(transition.ID, transition.Name)
	}
}
```
{% endcode %}

## Perform customer transition

`POST /rest/servicedeskapi/request/{issueIdOrKey}/transition`

This method performs a customer transition for a given request and transition. An optional comment can be included to provide a reason for the transition.

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
		transitionID = "921"
		comment      = "lorem"
	)

	response, err := atlassian.Request.Transition(context.Background(), issueKeyOrID, transitionID, comment)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Create Customer Request

`POST /rest/servicedeskapi/request`

This method creates a customer request at a service desk.&#x20;

* The payload must include the service desk and customer request type, as well as any fields that are required for the request type.&#x20;
* A list of the fields required by a customer request type can be obtained using the `sm.RequestType.Fields` method.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
	"time"
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

	form := &models.CustomerRequestFields{Fields: make(map[string]interface{})}

	if err := form.Text("summary", "Summary Sample"); err != nil {
		log.Fatal(err)
	}

	if err := form.Date("duedate", time.Now()); err != nil {
		log.Fatal(err)
	}

	if err := form.Components([]string{"Intranet"}); err != nil {
		log.Fatal(err)
	}

	if err := form.Labels([]string{"label-00", "label-01"}); err != nil {
		log.Fatal(err)
	}

	payload := &models.CreateCustomerRequestPayloadScheme{
		RequestParticipants: nil,
		ServiceDeskID:       "1",
		RequestTypeID:       "10",
	}

	ticket, response, err := atlassian.Request.Create(context.Background(), payload, form)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	fmt.Println("IssueID", ticket.IssueID)
	fmt.Println("IssueKey", ticket.IssueKey)
	fmt.Println("Reporter.EmailAddress", ticket.Reporter.EmailAddress)
	fmt.Println("CreatedDate.Friendly", ticket.CreatedDate.Friendly)

	for _, field := range ticket.RequestFieldValues {
		fmt.Println(field)
	}
}
```
{% endcode %}

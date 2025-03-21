---
cover: ../../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
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

# 🗺️ Status

{% @github-files/github-code-block url="https://github.com/ctreminiom/go-atlassian/issues/140" %}

## Search Workflow Statuses

`GET /rest/api/{2-3}/statuses/search`

Search returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of statuses that match a search on name or project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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
	
	options := &models.WorkflowStatusSearchParams{
		ProjectID:      "10000",
		StatusCategory: "IN_PROGRESS",
		Expand:         []string{"usages"},
	}

	var projectStatuses []*models.WorkflowStatusDetailScheme
	var startAt int

	for {

		page, response, err := atlassian.Workflow.Status.Search(context.Background(), options, startAt, 50)
		if err != nil {
			log.Println(response.Code)
			log.Println(response.Endpoint)
			log.Fatal(err)
		}

		projectStatuses = append(projectStatuses, page.Values...)

		if page.IsLast {
			break
		}

		startAt = startAt + 50
	}

	for _, status := range projectStatuses {
		fmt.Println(status.Name, status.ID, status.StatusCategory)


		fmt.Println("--------------------------------")
		fmt.Println("Status Name:", status.Name)
		fmt.Println("Status ID:", status.ID)
		fmt.Println("Status Category:", status.StatusCategory)

		for index, project := range status.Usages {

			fmt.Println("--- Project Usage #", index)
			fmt.Println("----  Project ID:", project.Project.ID)
			fmt.Println("----  Project IssueTypes:", project.IssueTypes)
		}
		fmt.Println("--------------------------------")
	}
}
```
{% endcode %}

## Gets Workflow Statuses

`GET /rest/api/{2-3}/statuses`

Get returns a list of the statuses specified by one or more status IDs.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	statuses, response, err := atlassian.Workflow.Status.Gets(context.Background(), []string{"401", "400", "10008"}, []string{"usages"})
	if err != nil {
		log.Println(response.Code)
		log.Println(response.Endpoint)
		log.Fatal(err)
	}

	for _, status := range statuses {
		fmt.Println(status.Name, status.ID, status.StatusCategory)

		fmt.Println("--------------------------------")
		fmt.Println("Status Name:", status.Name)
		fmt.Println("Status ID:", status.ID)
		fmt.Println("Status Category:", status.StatusCategory)

		for index, project := range status.Usages {

			fmt.Println("--- Project Usage #", index)
			fmt.Println("----  Project ID:", project.Project.ID)
			fmt.Println("----  Project IssueTypes:", project.IssueTypes)
		}
		fmt.Println("--------------------------------")
	}
}
```
{% endcode %}

## Create Workflow Statuses

`POST /rest/api/{2-3}/statuses`

Create creates statuses for a global or project scope.&#x20;

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	payload := &models.WorkflowStatusPayloadScheme{
		Statuses: []*models.WorkflowStatusNodeScheme{
			{
				Name:           "UAT TEST",
				StatusCategory: "IN_PROGRESS",
				Description: "status description",
			},
			{
				Name:           "Stage TEST",
				StatusCategory: "IN_PROGRESS",
				Description: "status description",
			},
		},
		Scope: &models.WorkflowStatusScopeScheme{
			Type: "GLOBAL",
		},
	}

	statuses, response, err := atlassian.Workflow.Status.Create(context.Background(), payload)
	if err != nil {
		log.Println(response.Code)
		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, status := range statuses {
		fmt.Println(status.Name, status.ID, status.StatusCategory)

		fmt.Println("--------------------------------")
		fmt.Println("Status Name:", status.Name)
		fmt.Println("Status ID:", status.ID)
		fmt.Println("Status Category:", status.StatusCategory)
		fmt.Println("--------------------------------")
	}
}
```
{% endcode %}

## Update Workflow Statuses

`PUT /rest/api/{2-3}/statuses`

Update updates statuses by ID.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	payload := &models.WorkflowStatusPayloadScheme{
		Statuses: []*models.WorkflowStatusNodeScheme{
			{
				ID:             "10013",
				Name:           "UAT Finished",
				StatusCategory: "DONE",
				Description:    "status description",
			},
			{
				ID:             "10014",
				Name:           "Stage Finished",
				StatusCategory: "DONE",
				Description:    "status description",
			},
		},
	}

	response, err := atlassian.Workflow.Status.Update(context.Background(), payload)
	if err != nil {
		log.Println(response.Code)
		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}
}
```
{% endcode %}

## Delete Workflow Statuses

`DELETE /rest/api/{2-3}/statuses`

Delete deletes statuses by ID.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	response, err := atlassian.Workflow.Status.Delete(context.Background(), []string{"10013", "10014"})
	if err != nil {
		log.Println(response.Code)
		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}
}
```
{% endcode %}

## Bulk Workflow Statuses

`GET /rest/api/{2-3}/status`

Bulk returns a list of all statuses associated with active workflows.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v3.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	activeStatuses, response, err := instance.Workflow.Status.Bulk(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	for _, status := range activeStatuses {
		fmt.Println(status.Name, status.ID)
	}
}
```
{% endcode %}

## Get Workflow Status

`GET /rest/api/{2-3}/status/{idOrName}`

Get returns a status.&#x20;

* The status must be associated with an active workflow to be returned.
* If a name is used on more than one status, only the status found first is returned. Therefore, identifying the status by its ID may be preferable.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v3.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	status, response, err := instance.Workflow.Status.Get(context.Background(), "Open")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(status.Name, status.ID)
}
```
{% endcode %}

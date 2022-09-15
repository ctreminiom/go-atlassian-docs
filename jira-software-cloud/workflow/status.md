---
description: >-
  An issue's status indicates its current place in the project's workflow.
  Default statuses are created when you create a project from one of our
  templates. The statuses are recommended starting points
---

# 🔄 Status

{% embed url="https://github.com/ctreminiom/go-atlassian/issues/140" %}
Tracking Issue
{% endembed %}

## Search Workflow Statuses

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of statuses that match a search on name or project.

{% code title="Extract the IN_PROGRESS workflow status from the project 10000" lineNumbers="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

Returns a list of the statuses specified by one or more status IDs.

{% code lineNumbers="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

Creates statuses for a global or project scope.&#x20;

{% code lineNumbers="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

Updates statuses by ID.

{% code lineNumbers="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Delete Workflow Statuses

Deletes statuses by ID.

{% code lineNumbers="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

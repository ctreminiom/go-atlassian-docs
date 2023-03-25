# 📟 Scheme

## Gets Workflows Schemes

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all workflow schemes, not including draft workflow schemes.

```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/jira/v3"
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

	workflowSchemes, response, err := instance.Workflow.Scheme.Gets(context.Background(), 0, 50)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	for _, workflowScheme := range workflowSchemes.Values {
		fmt.Println(workflowScheme.Name)
		fmt.Println(workflowScheme.Description)
		fmt.Println(workflowScheme.ID)
		fmt.Println(workflowScheme.Self)
		fmt.Println(workflowScheme.DefaultWorkflow)
	}

	fmt.Println(workflowSchemes.IsLast)
}
```

## Create Workflows Scheme

Creates a workflow scheme.

```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/jira/v3"
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

	instance, err := v3.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.WorkflowSchemePayloadScheme{
		DefaultWorkflow: "jira",
		Name:            "Example workflow scheme",
		Description:     "The description of the example workflow scheme.",
		IssueTypeMappings: map[string]interface{}{
			"10002": "PV: Project Management Workflow",
			"10005": "Software Simplified Workflow for Project K2",
		},
	}

	newWorkflowScheme, response, err := instance.Workflow.Scheme.Create(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(newWorkflowScheme.Name)
	fmt.Println(newWorkflowScheme.Description)
	fmt.Println(newWorkflowScheme.ID)
	fmt.Println(newWorkflowScheme.Self)
	fmt.Println(newWorkflowScheme.DefaultWorkflow)
}
```

## Get Workflow Scheme

Returns a workflow scheme.

```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/jira/v3"
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

	workflowScheme, response, err := instance.Workflow.Scheme.Get(context.Background(), 10007, false)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(workflowScheme.Name)
	fmt.Println(workflowScheme.Description)
	fmt.Println(workflowScheme.ID)
	fmt.Println(workflowScheme.Self)
	fmt.Println(workflowScheme.DefaultWorkflow)
}

```

## Update Workflow Scheme

## Delete Workflow Scheme

## Get Workflow Schemes Associations

## Assign Workflow Scheme

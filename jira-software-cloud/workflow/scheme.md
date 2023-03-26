# 🚛 Scheme

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

Updates a workflow scheme, including the name, default workflow, issue type to project mappings, and more. If the workflow scheme is active (that is, being used by at least one project), then a draft workflow scheme is created or updated instead, provided that `updateDraftIfNeeded` is set to `true`.

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
		Name:        "Example workflow scheme - UPDATED",
		Description: "The description of the example workflow scheme.",
		IssueTypeMappings: map[string]interface{}{
			"10002": "PV: Project Management Workflow",
			"10005": "Software Simplified Workflow for Project K2",
		},
		UpdateDraftIfNeeded: true,
	}

	workflowSchemeUpdated, response, err := instance.Workflow.Scheme.Update(context.Background(), 10003, payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(workflowSchemeUpdated.Name)
	fmt.Println(workflowSchemeUpdated.Description)
	fmt.Println(workflowSchemeUpdated.ID)
	fmt.Println(workflowSchemeUpdated.Self)
	fmt.Println(workflowSchemeUpdated.DefaultWorkflow)
}
```

## Delete Workflow Scheme

Delete deletes a workflow scheme. Please note that a workflow scheme **cannot** be deleted if it is active (that is, being used by at least one project).

```go
package main

import (
	"context"
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

	response, err := instance.Workflow.Scheme.Delete(context.Background(), 10003)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}
}
```

## Get Workflow Schemes Associations

Associations returns a list of the workflow schemes associated with a list of projects.&#x20;

* Each returned workflow scheme includes a list of the requested projects associated with it.
* Any team-managed or non-existent projects in the request are ignored and no errors are returned.

{% hint style="info" %}
If the project is associated with the `Default Workflow Scheme` no ID is returned. This is because the way the `Default Workflow Scheme` is stored means it has no ID.
{% endhint %}

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

	schemesByProject, response, err := instance.Workflow.Scheme.Associations(context.Background(), []int{10001})
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	for _, mapping := range schemesByProject.Values {
		fmt.Println(mapping.WorkflowScheme.Name, mapping.WorkflowScheme.ID)
		fmt.Println(mapping.ProjectIds)
	}
}
```

## Assign Workflow Scheme

Assign assigns a workflow scheme to a project. This operation is performed only when there are no issues in the project. The workflow schemes can only be assigned to classic projects.

```go
package main

import (
	"context"
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

	response, err := instance.Workflow.Scheme.Assign(context.Background(), "10001", "10001")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}
}
```

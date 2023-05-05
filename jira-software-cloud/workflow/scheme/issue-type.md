# ▶ Issue Type

## Get workflow for issue type in workflow scheme

Get returns the issue type-workflow mapping for an issue type in a workflow scheme.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Status</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>id</strong></td><td>The ID of the workflow scheme.</td><td></td><td></td></tr><tr><td><strong>issueType</strong></td><td>The ID of the issue type.</td><td></td><td></td></tr><tr><td><strong>returnDraftIfExists</strong></td><td>Returns the mapping from the workflow scheme's draft rather than the workflow scheme, if set to true. If no draft exists, the mapping from the workflow scheme is returned.</td><td></td><td></td></tr></tbody></table>

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

	mapping, response, err := instance.Workflow.Scheme.IssueType.Get(context.Background(), 10002, "10007", false)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(mapping)
}
```

## Set workflow for issue type in workflow scheme

Set sets the workflow for an issue type in a workflow scheme.

Note that active workflow schemes cannot be edited. If the workflow scheme is active, set `updateDraftIfNeeded` to `true` in the request body and a draft workflow scheme is created or updated with the new issue type-workflow mapping. The draft workflow scheme can be published in Jira.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Status</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>id</strong></td><td>The ID of the workflow scheme.</td><td></td><td></td></tr><tr><td><strong>issueType</strong></td><td>The ID of the issue type.</td><td></td><td></td></tr><tr><td><strong>payload</strong></td><td>The issue type-project mapping.</td><td></td><td></td></tr></tbody></table>

<details>

<summary>Payload Sample</summary>

```go
&IssueTypeWorkflowPayloadScheme{
	IssueType:           "10000", // The ID of the issue type. Not required if updating the issue type-workflow mapping.
	UpdateDraftIfNeeded: false, // Set to true to create or update the draft of a workflow scheme and update the mapping in the draft, when the workflow scheme cannot be edited. Defaults to false. Only applicable when updating the workflow-issue types mapping.
	Workflow:            "jira", // The name of the workflow.
}
```

</details>

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

	payload := &models.IssueTypeWorkflowPayloadScheme{
		IssueType:           "10000",
		UpdateDraftIfNeeded: false,
		Workflow:            "DESK: Jira Service Management default workflow",
	}

	workflowScheme, response, err := instance.Workflow.Scheme.IssueType.Set(context.Background(), 10007, "10007", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(workflowScheme)
}
```

## Delete workflow for issue type in workflow scheme

Delete deletes the issue type-workflow mapping for an issue type in a workflow scheme.

Note that active workflow schemes cannot be edited. If the workflow scheme is active, set `updateDraftIfNeeded` to `true` and a draft workflow scheme is created or updated with the issue type-workflow mapping deleted. The draft workflow scheme can be published in Jira.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Status</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>id</strong></td><td>The ID of the workflow scheme.</td><td></td><td></td></tr><tr><td><strong>issueType</strong></td><td>The ID of the issue type.</td><td></td><td></td></tr><tr><td><strong>updateDraftIfNeeded</strong></td><td><p>Set to true to create or update the draft of a workflow scheme and update the mapping in the draft, when the workflow scheme cannot be edited. Defaults to <code>false</code>.</p><p><br></p></td><td></td><td></td></tr></tbody></table>

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

	mapping, response, err := instance.Workflow.Scheme.IssueType.Delete(context.Background(), 10002, "10000", false)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	fmt.Println(mapping)
}
```

## Get issue types for workflows in workflow scheme

Mapping returns the workflow-issue type mappings for a workflow scheme.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Status</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>id</strong></td><td>The ID of the workflow scheme.</td><td></td><td></td></tr><tr><td><strong>workflowName</strong></td><td>The name of a workflow in the scheme. Limits the results to the workflow-issue type mapping for the specified workflow.</td><td></td><td></td></tr><tr><td><strong>returnDraftIfExists</strong></td><td>Returns the mapping from the workflow scheme's draft rather than the workflow scheme, if set to true. If no draft exists, the mapping from the workflow scheme is returned.</td><td></td><td></td></tr></tbody></table>

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

	mapping, response, err := instance.Workflow.Scheme.IssueType.Mapping(context.Background(), 10007, "", false)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}
	log.Println(response.Endpoint)

	for _, element := range mapping {
		fmt.Println(element.Workflow, element.Workflow)

	}
}
```

## Set issue types for workflow in workflow scheme

{% hint style="warning" %}
Not implemented, feel free to open a PR :thumbsup:
{% endhint %}

## Delete issue types for workflow in workflow scheme

{% hint style="warning" %}
Not implemented, feel free to open a PR :thumbsup:
{% endhint %}

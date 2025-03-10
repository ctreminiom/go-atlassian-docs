---
cover: ../../../.gitbook/assets/csd-6414_blog-hero-1160x652@2x-1-1560x760.png
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

# 📯 Issue Type

## Get workflow for issue type in workflow scheme

`GET /rest/api/{2-3}/workflowscheme/{id}/issuetype/{issueType}`

Get returns the issue type-workflow mapping for an issue type in a workflow scheme.

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
{% endcode %}

## Set workflow for issue type in workflow scheme

`PUT /rest/api/{2-3}/workflowscheme/{id}/issuetype/{issueType}`

Set sets the workflow for an issue type in a workflow scheme.

Note that active workflow schemes cannot be edited. If the workflow scheme is active, set `updateDraftIfNeeded` to `true` in the request body and a draft workflow scheme is created or updated with the new issue type-workflow mapping. The draft workflow scheme can be published in Jira.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v3 "github.com/ctreminiom/go-atlassian/v2/jira/v3"
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
{% endcode %}

## Delete workflow for issue type in workflow scheme

`DELETE /rest/api/{2-3}/workflowscheme/{id}/issuetype/{issueType}`

Delete deletes the issue type-workflow mapping for an issue type in a workflow scheme.

Note that active workflow schemes cannot be edited. If the workflow scheme is active, set `updateDraftIfNeeded` to `true` and a draft workflow scheme is created or updated with the issue type-workflow mapping deleted. The draft workflow scheme can be published in Jira.

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
{% endcode %}

## Get issue types for workflows in workflow scheme

`GET /rest/api/{2-3}/workflowscheme/{id}/workflow`

Mapping returns the workflow-issue type mappings for a workflow scheme.

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
{% endcode %}

## Set issue types for workflow in workflow scheme

{% hint style="warning" %}
Not implemented, feel free to open a PR :thumbsup:
{% endhint %}

## Delete issue types for workflow in workflow scheme

{% hint style="warning" %}
Not implemented, feel free to open a PR :thumbsup:
{% endhint %}

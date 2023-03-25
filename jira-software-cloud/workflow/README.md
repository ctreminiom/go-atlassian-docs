# 🧮 Workflow

A Jira workflow is a set of _statuses_ and _transitions_ that an issue moves through during its lifecycle, and typically represents a process within your organization. Workflows can be associated with particular projects and, optionally, specific issue types by using a [workflow scheme](https://confluence.atlassian.com/adminjiracloud/issue-workflow-schemes-844500788.html).

Workflows can be customized to fit the needs of a particular team or project. Customization options include adding new stages or statuses, defining new transitions between statuses, and setting rules and conditions that determine when an issue can move from one stage to another. With a well-designed Jira workflow, teams can track the progress of their work more effectively and ensure that all team members are on the same page about the status of each task or issue.

## Create Workflow

Create creates a workflow. You can define transition rules using the shapes detailed in the following sections. If no transitional rules are specified the default system transition rules are used.

### Conditions

Conditions enable workflow rules that govern whether a transition can execute.

{% tabs %}
{% tab title="Always false conditions" %}
A condition that always fails.

{% code overflow="wrap" %}
```go
{
	Type: "RemoteOnlyCondition",
},
```
{% endcode %}
{% endtab %}

{% tab title="Block transition until approval" %}
A condition that blocks issue transition if there is a pending approval.

```go
{
    Type: "BlockInProgressApprovalCondition",
}
```
{% endtab %}

{% tab title="Compare number custom field condition" %}
A condition that allows transition if a comparison between a number custom field and a value is true.

```go
{
	Type: "CompareNumberCFCondition",
	Configuration: map[string]interface{}{
		"comparator": "=",
		"fieldId":    "customfield_10029",
		"fieldValue": 2,
	},
}
```
{% endtab %}

{% tab title="Hide from user condition" %}
A condition that hides a transition from users. The transition can only be triggered from a workflow function or REST API operation.

```go
{
    Type: "RemoteOnlyCondition",
}
```


{% endtab %}

{% tab title="Only assignee condition" %}
A condition that allows only the assignee to execute a transition.

```go
{
    Type: "RemoteOnlyCondition",
}
```
{% endtab %}
{% endtabs %}

```go
package main

import (
	"context"
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

	payload := &models.WorkflowPayloadScheme{
		Name:        "Workflow Sample",
		Description: "description of the workflow sample",
		Statuses: []*models.WorkflowTransitionScreenScheme{
			{
				ID: "1", // Open
				Properties: map[string]interface{}{
					"jira.issue.editable": true,
				},
			},
			{
				ID: "3", // In Progress
			},
		},
		Transitions: []*models.WorkflowTransitionPayloadScheme{
			{
				Name: "Open",
				To:   "1",
				Type: "initial",
			},
			{
				Name: "Open",
				From: []string{"1"},
				Properties: map[string]interface{}{
					"custom-property": "custom-value",
				},
				Rules: &models.WorkflowTransitionRulePayloadScheme{
					Conditions: &models.WorkflowConditionScheme{
						Conditions: []*models.WorkflowConditionScheme{
							{
								Type: "RemoteOnlyCondition",
							},
							{
								Configuration: map[string]interface{}{
									"groups": []string{"confluence-users", "scrum-masters"},
								},
								Type: "UserInAnyGroupCondition",
							},
						},
						Operator: "AND",
					},
				},
				To:   "3",
				Type: "directed",
			},
		},
	}

	newWorkflow, response, err := instance.Workflow.Create(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	log.Println(newWorkflow.Name)
	log.Println(newWorkflow.EntityID)
}

```

## Search Workflows

## Delete Workflow

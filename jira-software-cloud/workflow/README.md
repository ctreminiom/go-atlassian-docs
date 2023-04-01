# 🖨 Workflow

A Jira workflow is a set of _statuses_ and _transitions_ that an issue moves through during its lifecycle, and typically represents a process within your organization. Workflows can be associated with particular projects and, optionally, specific issue types by using a [workflow scheme](https://confluence.atlassian.com/adminjiracloud/issue-workflow-schemes-844500788.html).

Workflows can be customized to fit the needs of a particular team or project. Customization options include adding new stages or statuses, defining new transitions between statuses, and setting rules and conditions that determine when an issue can move from one stage to another. With a well-designed Jira workflow, teams can track the progress of their work more effectively and ensure that all team members are on the same page about the status of each task or issue.

## Create Workflow

Create creates a workflow. You can define transition rules using the shapes detailed in the following sections. If no transitional rules are specified the default system transition rules are used.

### Conditions

Conditions enable workflow rules that govern whether a transition can execute.

#### Always false conditions

A condition that always fails.

{% code overflow="wrap" %}
```go
{
	Type: "RemoteOnlyCondition",
},
```
{% endcode %}

#### Block transition until approval

A condition that blocks issue transition if there is a pending approval.

```go
{
    Type: "BlockInProgressApprovalCondition",
}
```

#### Compare number custom field condition

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

#### Hide from user condition

A condition that hides a transition from users. The transition can only be triggered from a workflow function or REST API operation.

```go
{
    Type: "RemoteOnlyCondition",
}
```

#### Only assignee condition

A condition that allows only the assignee to execute a transition.

```go
{
    Type: "AllowOnlyAssignee",
}
```

#### **Only reporter condition**

A condition that allows only the reporter to execute a transition.

```go
{
    Type: "AllowOnlyReporter",
}
```

#### **Permission condition**

A condition that allows only users with a permission to execute a transition.

```go
{
	Type: "PermissionCondition",
	Configuration: map[string]interface{}{
		"permissionKey": "BROWSE_PROJECTS",
	},
},
```

#### **Previous status condition**

A condition that allows a transition based on whether an issue has or has not transitioned through a status.

```go
{
	Type: "PreviousStatusCondition",
	Configuration: map[string]interface{}{
		"ignoreLoopTransitions": true,
		"includeCurrentStatus":  true,
		"mostRecentStatusOnly":  true,
		"reverseCondition":      true,
		"previousStatus": map[string]interface{}{
			"id": "5",
		},
	},
}
```

#### **Separation of duties condition**

A condition that prevents a user to perform the transition, if the user has already performed a transition on the issue.

```go
{
	Type: "SeparationOfDutiesCondition",
	Configuration: map[string]interface{}{
		"fromStatus": map[string]interface{}{"id": "5"},
		"toStatus":   map[string]interface{}{"id": "6"},
	},
}
```

#### **Subtask blocking condition**

A condition that blocks transition on a parent issue if any of its subtasks are in any of one or more statuses.

```go
{
	Type: "SubTaskBlockingCondition",
	Configuration: map[string]interface{}{
		"statuses": []map[string]interface{}{
			{
				"id": "1",
			},
			{
				"id": "3",
			},
		},
	},
},
```

#### **User is in any group condition**

A condition that allows users belonging to any group from a list of groups to execute a transition.

```go
{
	Type: "UserInAnyGroupCondition",
	Configuration: map[string]interface{}{
		"groups": []string{"administrators", "atlassian-addons-admin"},
	},
}
```

#### **User is in any project role condition**

A condition that allows only users with at least one project roles from a list of project roles to execute a transition.

```go
{
	Type: "InAnyProjectRoleCondition",
	Configuration: map[string]interface{}{
		"projectRoles": []map[string]interface{}{
			{
				"id": "10002",
			},
			{
				"id": "10003",
			},
			{
				"id": "10012",
			},
			{
				"id": "10013",
			},
		},
	},
}
```

#### **User is in custom field condition**

A condition that allows only users listed in a given custom field to execute the transition.

```go
{
	Type: "UserIsInCustomFieldCondition",
	Configuration: map[string]interface{}{
		"allowUserInField": false,
		"fieldId":          "customfield_10010",
	},
}
```

#### **User is in group condition**

A condition that allows users belonging to a group to execute a transition.

```go
{
	Type: "UserInGroupCondition",
	Configuration: map[string]interface{}{
		"group": "administrators",
	},
}
```

#### **User is in group custom field condition**

A condition that allows users belonging to a group specified in a custom field to execute a transition.

```go
{
	Type: "InGroupCFCondition",
	Configuration: map[string]interface{}{
		"fieldId": "customfield_10012",
	},
}
```

#### **User is in project role condition**

A condition that allows users with a project role to execute a transition.

```go
{
	Type: "InProjectRoleCondition",
	Configuration: map[string]interface{}{
		"projectRole": map[string]interface{}{
			"id": "10002",
		},
	},
}
```

#### **Value field condition**

A conditions that allows a transition to execute if the value of a field is equal to a constant value or simply set.

```go
{
	Type: "ValueFieldCondition",
	Configuration: map[string]interface{}{
		"fieldId":        "assignee",
		"fieldValue":     "qm:6e1ecee6-8e64-4db6-8c85-916bb3275f51:54b56885-2bd2-4381-8239-78263442520f",
		"comparisonType": "NUMBER",
		"comparator":     "=",
	},
}
```

### **Validators**

Validators check that any input made to the transition is valid before the transition is performed.

#### **Date field validator**

A validator that compares two dates.

```go
{
	Type: "DateFieldValidator",
	Configuration: map[string]interface{}{
		"comparator":  ">",
		"date1":       "updated",
		"date2":       "created",
		"expression":  "1d",
		"includeTime": true,
	},
}
```

#### **Windows date validator**

A validator that checks that a date falls on or after a reference date and before or on the reference date plus a number of days.

```go
{
	Type: "WindowsDateValidator",
	Configuration: map[string]interface{}{
		"date1":       "customfield_10009",
		"date2":       "created",
		"windowsDays": 5,
	},
}
```

#### **Field required validator**

A validator that checks fields are not empty. By default, if a field is not included in the current context it's ignored and not validated.

```go
{
	Type: "FieldRequiredValidator",
	Configuration: map[string]interface{}{
		"ignoreContext": true,
		"errorMessage":  "Hey",
		"fieldIds": []string{
			"versions",
			"customfield_10037",
			"customfield_10003",
		},
	},
}
```

#### **Field changed validator**

A validator that checks that a field value is changed. However, this validation can be ignored for users from a list of groups.

```go
{
	Type: "FieldChangedValidator",
	Configuration: map[string]interface{}{
		"fieldId":      "comment",
		"errorMessage": "Hey",
		"exemptedGroups": []string{
			"administrators",
			"atlassian-addons-admin",
		},
	},
}
```

#### **Field has single value validator**

A validator that checks that a multi-select field has only one value. Optionally, the validation can ignore values copied from subtasks.

```go
{
	Type: "FieldHasSingleValueValidator",
	Configuration: map[string]interface{}{
		"fieldId":         "attachment",
		"excludeSubtasks": true,
	},
}
```

#### **Parent status validator**

A validator that checks the status of the parent issue of a subtask. Ìf the issue is not a subtask, no validation is performed.

```go
{
	Type: "ParentStatusValidator",
	Configuration: map[string]interface{}{
		"parentStatuses": []map[string]interface{}{
			{
				"id": "2",
			},
			{
				"id": "1",
			},
		},
	},
}
```

#### **Permission validator**

A validator that checks the user has a permission.

```go
{
	Type: "PermissionValidator",
	Configuration: map[string]interface{}{
		"permissionKey": "ADMINISTER_PROJECTS",
	},
},
```

#### **Previous status validator**

A validator that checks if the issue has held a status.

```gn
{
	Type: "PreviousStatusValidator",
	Configuration: map[string]interface{}{
		"mostRecentStatusOnly": false,
		"previousStatus":       map[string]interface{}{"id": "15"},
	},
},
```

#### **Regular expression validator**

A validator that checks the content of a field against a regular expression.

```go
{
	Type: "RegexpFieldValidator",
	Configuration: map[string]interface{}{
		"regExp":  "[0-9]",
		"fieldId": "customfield_10029",
	},
},
```

#### **User permission validator**

A validator that checks if a user has a permission. Obsolete. You may encounter this validator when getting transition rules and can pass it when updating or creating rules, for example, when you want to duplicate the rules from a workflow on a new workflow.

```go
{
	Type: "UserPermissionValidator",
	Configuration: map[string]interface{}{
		"permissionKey": "BROWSE_PROJECTS",
		"nullAllowed":   false,
		"username":      "TestUser",
	},
},
```

### **Post functions**

Post functions carry out any additional processing required after a Jira workflow transition is executed.

#### **Fire issue event function**

A post function that fires an event that is processed by the listeners.

```go
{
	Type: "FireIssueEventFunction",
	Configuration: map[string]interface{}{
		"event": map[string]interface{}{"id": "1"},
	},
},
```

#### **Update issue status**

A post function that sets issue status to the linked status of the destination workflow status.

```go
{
    Type: "UpdateIssueStatusFunction",
}
```

{% hint style="info" %}
This post function is a default function in global and directed transitions. It can only be added to the initial transition and can only be added once.
{% endhint %}

#### **Create comment**

A post function that adds a comment entered during the transition to an issue.

```go
{
    Type: "CreateCommentFunction",
}
```

{% hint style="info" %}
This post function is a default function in global and directed transitions. It can only be added to the initial transition and can only be added once.
{% endhint %}

#### **Store issue**

A post function that stores updates to an issue.

```go
{
    Type: "CreateCommentFunction",
}
```

{% hint style="info" %}
This post function can only be added to the initial transition and can only be added once.
{% endhint %}

#### **Assign to current user function**

A post function that assigns the issue to the current user if the current user has the `ASSIGNABLE_USER` permission.

```go
{
    Type: "CreateCommentFunction",
}
```

{% hint style="info" %}
This post function can be included once in a transition.
{% endhint %}

#### **Assign to lead function**

A post function that assigns the issue to the project or component lead developer.

```go
{
    Type: "CreateCommentFunction",
}
```

{% hint style="info" %}
This post function can be included once in a transition.
{% endhint %}

#### **Assign to reporter function**

A post function that assigns the issue to the reporter.

```go
{
    Type: "CreateCommentFunction",
}
```

{% hint style="info" %}
This post function can be included once in a transition.
{% endhint %}

#### **Clear field value function**

A post function that clears the value from a field.

```go
{
	Type: "ClearFieldValuePostFunction",
	Configuration: map[string]interface{}{
		"fieldId": "assignee",
	},
},
```

#### **Copy value from other field function**

A post function that copies the value of one field to another, either within an issue or from parent to subtask.

```go
{
	Type: "CopyValueFromOtherFieldPostFunction",
	Configuration: map[string]interface{}{
		"sourceFieldId":      "assignee",
		"destinationFieldId": "creator",
		"copyType":           "same",
	},
},
```

#### **Update issue custom field function**

A post function that updates the content of an issue custom field.

```go
{
	Type: "UpdateIssueCustomFieldPostFunction",
	Configuration: map[string]interface{}{
		"mode":       "append",
		"fieldId":    "customfield_10003",
		"fieldValue": "yikes",
	},
},
```

#### **Update issue field function**

A post function that updates a simple issue field.

```go
{
	Type: "UpdateIssueFieldFunction",
	Configuration: map[string]interface{}{
		"fieldId":    "assignee",
		"fieldValue": "5f0c277e70b8a90025a00776",
	},
},
```

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

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of published classic workflows. When workflow names are specified, details of those workflows are returned. Otherwise, all published classic workflows are returned.

{% hint style="warning" %}
This operation does not return next-gen workflows
{% endhint %}

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

	options := &models.WorkflowSearchOptions{
		WorkflowName: nil,
		Expand:       []string{"transitions", "statuses", "default", "schemes", "projects", "hasDraftWorkflow", "operations"},
		QueryString:  "",
		OrderBy:      "name",
		IsActive:     false,
	}

	workflows, response, err := instance.Workflow.Gets(context.Background(), options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	for _, workflow := range workflows.Values {
		fmt.Println(workflow.ID, workflow.Description, len(workflow.Statuses))
	}

}
```

## Delete Workflow

Deletes a workflow, the workflow cannot be deleted if it is:

* an active workflow.
* a system workflow.
* associated with any workflow scheme.
* associated with any draft workflow scheme.

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

	response, err := instance.Workflow.Delete(context.Background(), "6f2a8273-7f30-4444-96ad-7ddb5d4c9f8e")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

}
```

---
cover: ../../.gitbook/assets/jiraautomation_bloghero.jpg
coverY: 27
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

# 🖨️ Workflow

A Jira workflow is a set of _statuses_ and _transitions_ that an issue moves through during its lifecycle, and typically represents a process within your organization. Workflows can be associated with particular projects and, optionally, specific issue types by using a [workflow scheme](https://confluence.atlassian.com/adminjiracloud/issue-workflow-schemes-844500788.html).

Workflows can be customized to fit the needs of a particular team or project. Customization options include adding new stages or statuses, defining new transitions between statuses, and setting rules and conditions that determine when an issue can move from one stage to another. With a well-designed Jira workflow, teams can track the progress of their work more effectively and ensure that all team members are on the same page about the status of each task or issue.

## Create Workflow

`POST /rest/api/{2-3}/workflow`

{% hint style="warning" %}
Deprecated
{% endhint %}

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

{% code fullWidth="true" %}
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
{% endcode %}

## Search Workflows

`GET /rest/api/{2-3}/workflow/search`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of published classic workflows. When workflow names are specified, details of those workflows are returned. Otherwise, all published classic workflows are returned.

{% hint style="warning" %}
This operation does not return next-gen workflows, Deprecated
{% endhint %}

{% code fullWidth="true" %}
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
{% endcode %}

## Delete Workflow

`DELETE /rest/api/{2-3}/workflow/{entityId}`

Deletes a workflow, the workflow cannot be deleted if it is:

* an active workflow.
* a system workflow.
* associated with any workflow scheme.
* associated with any draft workflow scheme.

{% code fullWidth="true" %}
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
{% endcode %}

## Bulk Get Workflows

`POST /rest/api/{2-3}/workflows`

Returns a list of workflows and related statuses by providing workflow names, workflow IDs, or project and issue types.

[**Permissions**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#permissions) **required:**

* _Administer Jira_ global permission to access all, including project-scoped, workflows
* At least one of the _Administer projects_ and _View (read-only) workflow_ project permissions to access project-scoped workflows

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := &models.WorkflowSearchCriteria{
		WorkflowIDs:   nil,                                         // The list of workflow IDs to query.
		WorkflowNames: []string{"PV: Project Management Workflow"}, // The list of workflow names to query.
	}

	expand := []string{"workflows.usages", "statuses.usages"}

	workflows, response, err := atlassian.Workflow.Search(context.Background(), options, expand, true)
	if err != nil {
		log.Println("Unable to search for workflows: ", err)
		log.Println(response.Bytes.String())
		log.Println(response.StatusCode)
		log.Fatal(err)
	}

	for _, workflow := range workflows.Workflows {
		workflowBuffer, _ := json.MarshalIndent(workflow, "", "\t")
		fmt.Println(string(workflowBuffer))
	}

	for _, status := range workflows.Statuses {
		statusBuffer, _ := json.MarshalIndent(status, "", "\t")
		fmt.Println(string(statusBuffer))
	}
}
```
{% endcode %}

## Get Workflow Capabilities

`GET /rest/api/{2-3}/workflows/capabilities`

Get the list of workflow capabilities for a specific workflow using either the workflow ID, or the project and issue type ID pair.&#x20;

The response includes the scope of the workflow, defined as **global/project-based**, and a list of project types that the workflow is scoped to. It also includes all rules organised into their broad categories `(conditions, validators, actions, triggers, screens)` as well as the source location (Atlassian-provided, Connect, Forge).

### **Validators**

A validator rule that checks if a user has the required permissions to execute the transition in the workflow.

{% tabs %}
{% tab title="Permission validator" %}
A validator rule that checks if a user has the required permissions to execute the transition in the workflow.

```json
{
   "ruleKey": "system:check-permission-validator",
   "parameters": {
     "permissionKey": "ADMINISTER_PROJECTS"
   }
 }
```

**Parameters**:

* `permissionKey` The permission required to perform the transition. Allowed values: [built-in Jira permissions](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-permission-schemes/#built-in-permissions).
{% endtab %}

{% tab title="Parent or child blocking validator" %}
A validator to block the child issue\u2019s transition depending on the parent issue\u2019s status.

```json
{
   "ruleKey" : "system:parent-or-child-blocking-validator"
   "parameters" : {
     "blocker" : "PARENT"
     "statusIds" : "1,2,3"
   }
}
```
{% endtab %}

{% tab title="Previous status validator" %}
A validator that checks if an issue has transitioned through specified previous status(es) before allowing the current transition to occur.

```json
{
   "ruleKey": "system:previous-status-validator",
   "parameters": {
     "previousStatusIds": "10014",
     "mostRecentStatusOnly": "true"
   }
 }
```

**Parameters**:

* `previousStatusIds` a comma-separated list of status IDs, currently only support one ID.
* `mostRecentStatusOnly` when `true` only considers the most recent status for the condition evaluation. Allowed values: `true`, `false`.
{% endtab %}

{% tab title="Validate a field value" %}
A validation that ensures a specific field's value meets the defined criteria before allowing an issue to transition in the workflow.

Depending on the rule type, the result will vary:

**Field required**

```json

{
   "ruleKey": "system:validate-field-value",
   "parameters": {
     "ruleType": "fieldRequired",
     "fieldsRequired": "assignee",
     "ignoreContext": "true",
     "errorMessage": "An assignee must be set!"
   }
 }
```

**Parameters**:

* `fieldsRequired` the ID of the field that is required. For a custom field, it would look like `customfield_123`.
* `ignoreContext` controls the impact of context settings on field validation. When set to `true`, the validator doesn't check a required field if its context isn't configured for the current issue. When set to `false`, the validator requires a field even if its context is invalid. Allowed values: `true`, `false`.
* `errorMessage` is the error message to display if the user does not provide a value during the transition. A default error message will be shown if you don't provide one (Optional).
{% endtab %}
{% endtabs %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	capabilities, response, err := atlassian.Workflow.Capabilities(context.Background(), "workflow-id", "project-id", "issuetype-id")
	if err != nil {
		log.Println(response.Bytes.String())
		log.Println(response.StatusCode)
		log.Fatal(err)
	}

	capabilitiesBuffer, _ := json.MarshalIndent(capabilities, "", "\t")
	fmt.Println(string(capabilitiesBuffer))
}
```
{% endcode %}

## Bulk Create Workflows

`POST /rest/api/{2-3}/workflows/create`

Create workflows and related statuses. This method allows you to create a workflow by defining transition rules using the shapes detailed in the Atlassian **REST API** documentation. If no transition rules are specified, the default system transition rules will be used.

[**Permissions**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#permissions) **required:**

* _Administer Jira_ project permission to create all, including global-scoped, workflows
* _Administer projects_ project permissions to create project-scoped workflows

### Key Helper Methods

#### AddStatus() <a href="#add-status" id="add-status"></a>

The `AddStatus()` method is used to add a status to the `WorkflowCreatesPayload`. This method ensures that any status used in the workflow is explicitly included in the payload under the `statuses` tag.

```go
payload.AddStatus(&models.WorkflowStatusUpdateScheme{
    ID:              "10012",
    Name:            "To Do",
    StatusCategory:  "TODO",
    StatusReference: "f0b24de5-25e7-4fab-ab94-63d81db6c0c0",
})
```

In this example, the status **"To Do"** with ID "10012" is added to the workflow payload. The `StatusReference` is a unique identifier used later when associating statuses with workflow steps.

#### AddTransition()

The `AddTransition()` method is used to add a transition between statuses in a workflow. Transitions define the movement between statuses in a workflow, such as moving from "To Do" to "In Progress".

```go
// Adding a transition to the workflow
err := epicWorkflow.AddTransition(&models.TransitionUpdateDTOScheme{
    ID:   "1",
    Type: "INITIAL",
    Name: "Create",
    To: &models.StatusReferenceAndPortScheme{
        StatusReference: "f0b24de5-25e7-4fab-ab94-63d81db6c0c0",
    },
})
```

This example adds an "INITIAL" transition called "Create" that moves the issue to the "To Do" status. The `StatusReference` is used to link the transition to the appropriate status.

#### AddWorkflow()

The `AddWorkflow()` method is used to add a workflow to the `WorkflowCreatesPayload`. This method is crucial for building the final payload that will be sent to the JIRA API.

```go
// Adding the workflow to the payload
err := payload.AddWorkflow(epicWorkflow)
if err != nil {
    log.Fatal(err)
}
```

In this example, the `epicWorkflow` (previously populated with statuses and transitions) is added to the payload. This payload can include multiple workflows, making it possible to create several workflows in a single API request.

### Considerations

* Any status used within a workflow (through transitions or initial states) **must** be included in the `statuses` tag under the `WorkflowCreatesPayload` struct.&#x20;
  * This ensures that JIRA recognizes and correctly maps the statuses during workflow creation. Failure to include a status may result in errors or incomplete workflow creation.

{% code fullWidth="true" %}
```go

package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	// Create the workflow creation payload
	payload := &models.WorkflowCreatesPayload{
		Scope: &models.WorkflowScopeScheme{Type: "GLOBAL"},
	}

	// Add the status references on the payload
	statuses := []struct {
		ID, Name, StatusCategory, StatusReference string
	}{
		{"10012", "To Do", "TODO", "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{"3", "In Progress", "IN_PROGRESS", "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{"10002", "Done", "DONE", "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, status := range statuses {
		payload.AddStatus(&models.WorkflowStatusUpdateScheme{
			ID:              status.ID,
			Name:            status.Name,
			StatusCategory:  status.StatusCategory,
			StatusReference: status.StatusReference,
		})
	}

	epicWorkflow := &models.WorkflowCreateScheme{
		Description: "This workflow represents the process of software development related to epics.",
		Name:        "Epic Software Development Workflow V4",
	}

	// Add the statuses to the workflow using the referenceID and the layout
	layouts := []struct {
		X, Y            float64
		StatusReference string
	}{
		{114.99993896484375, -16, "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{317.0000915527344, -16, "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{508.000244140625, -16, "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, layout := range layouts {
		epicWorkflow.AddStatus(&models.StatusLayoutUpdateScheme{
			Layout:          &models.WorkflowLayoutScheme{X: layout.X, Y: layout.Y},
			StatusReference: layout.StatusReference,
		})
	}

	// Add the transitions to the workflow
	transitions := []struct {
		ID, Type, Name, StatusReference string
	}{
		{"1", "INITIAL", "Create", "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{"21", "GLOBAL", "In Progress", "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{"31", "GLOBAL", "Done", "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, transition := range transitions {
		err = epicWorkflow.AddTransition(&models.TransitionUpdateDTOScheme{
			ID:   transition.ID,
			Type: transition.Type,
			Name: transition.Name,
			To: &models.StatusReferenceAndPortScheme{
				StatusReference: transition.StatusReference,
			},
		})

		if err != nil {
			log.Fatal(err)
		}
	}

	// You can multiple workflows on the same payload
	if err := payload.AddWorkflow(epicWorkflow); err != nil {
		log.Fatal(err)
	}

	workflowsCreated, response, err := atlassian.Workflow.Creates(context.Background(), payload)
	if err != nil {
		log.Println("Unable to create workflows: ", err)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	workflowBuffer, _ := json.MarshalIndent(workflowsCreated, "", "\t")
	fmt.Println(string(workflowBuffer))
}
```
{% endcode %}

## Validate Create Workflows

`POST /rest/api/{2-3}/workflows/create/validation`

Validates workflows before creating them. This method allows you to validate the configuration of one or more workflows before they are created in Jira.&#x20;

It helps ensure that the workflows adhere to the defined rules and constraints. The validation checks will include all aspects of the workflows, such as transitions, statuses, and any related conditions or validators.

[**Permissions**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#permissions) **required:**

* _Administer Jira_ project permission to create all, including global-scoped, workflows
* _Administer projects_ project permissions to create project-scoped workflows

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	// Create the workflow creation payload
	payload := &models.WorkflowCreatesPayload{
		Scope: &models.WorkflowScopeScheme{Type: "GLOBAL"},
	}

	// Add the status references on the payload
	statuses := []struct {
		ID, Name, StatusCategory, StatusReference string
	}{
		{"10012", "To Do", "TODO", "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{"3", "In Progress", "IN_PROGRESS", "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{"10002", "Done", "DONE", "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, status := range statuses {
		payload.AddStatus(&models.WorkflowStatusUpdateScheme{
			ID:              status.ID,
			Name:            status.Name,
			StatusCategory:  status.StatusCategory,
			StatusReference: status.StatusReference,
		})
	}

	epicWorkflow := &models.WorkflowCreateScheme{
		Description: "This workflow represents the process of software development related to epics.",
		Name:        "Epic Software Development Workflow V4",
	}

	// Add the statuses to the workflow using the referenceID and the layout
	layouts := []struct {
		X, Y            float64
		StatusReference string
	}{
		{114.99993896484375, -16, "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{317.0000915527344, -16, "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{508.000244140625, -16, "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, layout := range layouts {
		epicWorkflow.AddStatus(&models.StatusLayoutUpdateScheme{
			Layout:          &models.WorkflowLayoutScheme{X: layout.X, Y: layout.Y},
			StatusReference: layout.StatusReference,
		})
	}

	// Add the transitions to the workflow
	transitions := []struct {
		ID, Type, Name, StatusReference string
	}{
		{"1", "INITIAL", "Create", "f0b24de5-25e7-4fab-ab94-63d81db6c0c0"},
		{"21", "GLOBAL", "In Progress", "c7a35bf0-c127-4aa6-869f-4033730c61d8"},
		{"31", "GLOBAL", "Done", "6b3fc04d-3316-46c5-a257-65751aeb8849"},
	}

	for _, transition := range transitions {
		err = epicWorkflow.AddTransition(&models.TransitionUpdateDTOScheme{
			ID:   transition.ID,
			Type: transition.Type,
			Name: transition.Name,
			To: &models.StatusReferenceAndPortScheme{
				StatusReference: transition.StatusReference,
			},
		})

		if err != nil {
			log.Fatal(err)
		}
	}

	// You can multiple workflows on the same payload
	if err := payload.AddWorkflow(epicWorkflow); err != nil {
		log.Fatal(err)
	}

	validationOptions := &models.ValidationOptionsForCreateScheme{
		Payload: payload,
		Options: &models.ValidationOptionsLevelScheme{
			Levels: []string{"ERROR", "WARNING"},
		},
	}

	validationErrors, response, err := atlassian.Workflow.ValidateCreateWorkflows(context.Background(), validationOptions)
	if err != nil {
		log.Println("Unable to create workflows: ", err)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, validationError := range validationErrors.Errors {
		buffer, _ := json.MarshalIndent(validationError, "", "\t")
		fmt.Println(string(buffer))
	}
}
```
{% endcode %}

## Bulk Update Workflows

`POST /rest/api/{2-3}/workflows/update`

Updates workflows. This method allows you to update workflows by providing a payload containing the details of the updates. You can expand specific fields using the 'expand' parameter.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.WorkflowUpdatesPayloadScheme{
		Statuses: []*models.WorkflowStatusUpdateScheme{
			{
				ID:              "10012",
				Name:            "To Do",
				StatusCategory:  "TODO",
				StatusReference: "f0b24de5-25e7-4fab-ab94-63d81db6c0c0",
			},
			{
				ID:              "3",
				Name:            "In Progress",
				StatusCategory:  "IN_PROGRESS",
				StatusReference: "c7a35bf0-c127-4aa6-869f-4033730c61d8",
			},
			{
				ID:              "10002",
				Name:            "Done",
				StatusCategory:  "DONE",
				StatusReference: "6b3fc04d-3316-46c5-a257-65751aeb8849",
			},
		},
		Workflows: []*models.WorkflowUpdateScheme{
			{
				DefaultStatusMappings: nil,
				ID:                    "6c91af3d-2ae1-46c1-9009-eb93a10a54d1",
				StartPointLayout:      nil,
				Version: &models.WorkflowDocumentVersionScheme{
					ID:            "c44b423d-89c9-458d-abb7-89ac4fefebd4",
					VersionNumber: 0,
				},
			},
		},
	}

	expand := []string{"workflows.usages", "statuses.usages"}
	workflowsUpdated, response, err := atlassian.Workflow.Updates(context.Background(), payload, expand)
	if err != nil {
		log.Println("Unable to update the workflows(s): ", err)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	workflowBuffer, _ := json.MarshalIndent(workflowsUpdated, "", "\t")
	fmt.Println(string(workflowBuffer))
}
```
{% endcode %}

## Validate Update Workflows

`POST /rest/api/{2-3}/workflows/update/validation`

Validates the update of one or more workflows. This method allows you to validate changes to workflows before they are applied.&#x20;

The validation checks for potential issues that could prevent the workflows from being updated successfully. The validation process will check for conditions such as:&#x20;

* Whether the transitions are valid.&#x20;
* Whether the status and transition names are unique within the workflow.&#x20;
* If the validation fails, a list of validation errors is returned, which should be resolved before applying the changes.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("SITE")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.WorkflowUpdatesPayloadScheme{
		Statuses: []*models.WorkflowStatusUpdateScheme{
			{
				ID:              "10012",
				Name:            "To Do",
				StatusCategory:  "TODO",
				StatusReference: "f0b24de5-25e7-4fab-ab94-63d81db6c0c0",
			},
			{
				ID:              "3",
				Name:            "In Progress",
				StatusCategory:  "IN_PROGRESS",
				StatusReference: "c7a35bf0-c127-4aa6-869f-4033730c61d8",
			},
			{
				ID:              "10002",
				Name:            "Done",
				StatusCategory:  "DONE",
				StatusReference: "6b3fc04d-3316-46c5-a257-65751aeb8849",
			},
		},
		Workflows: []*models.WorkflowUpdateScheme{
			{
				DefaultStatusMappings: nil,
				ID:                    "6c91af3d-2ae1-46c1-9009-eb93a10a54d1",
				StartPointLayout:      nil,
				Version: &models.WorkflowDocumentVersionScheme{
					ID:            "c44b423d-89c9-458d-abb7-89ac4fefebd4",
					VersionNumber: 0,
				},
			},
		},
	}

	payload2 := &models.ValidationOptionsForUpdateScheme{
		Payload: payload,
		Options: &models.ValidationOptionsLevelScheme{
			Levels: []string{"ERROR", "WARNING"},
		},
	}

	messages, response, err := atlassian.Workflow.ValidateUpdateWorkflows(context.Background(), payload2)
	if err != nil {
		log.Println("Unable to update the workflows(s): ", err)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	workflowBuffer, _ := json.MarshalIndent(messages, "", "\t")
	fmt.Println(string(workflowBuffer))
}
```
{% endcode %}

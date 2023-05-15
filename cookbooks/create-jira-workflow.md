# 🚦 Create Jira Workflow

In this article, I would be showing you how to create Jira workflow and append transitions using `go-atlassian`

## Step 1: Create a new Go project

Create a new directory for your project and navigate to it in your terminal or command prompt. Initialize a new Go module using the following command:

```bash
go mod init your-module-name
```

## Step 2: Install the "go-atlassian" library&#x20;

To use the "go-atlassian" library, you need to install it as a dependency in your project. Run the following command:

```bash
go get github.com/ctreminiom/go-atlassian
```

## Step 3: Import the required packages&#x20;

Create a new Go file, e.g., `main.go`, and import the necessary packages:

```go
package main

import (
	"fmt"
	"log"

	jira "github.com/ctreminiom/go-atlassian/jira/v2"
)
```

## Step 4: Set up Jira API client&#x20;

Initialize the Jira API client with your Jira base URL and API token:

```go
func main() {
	jiraClient, err := jira.New(nil, "https://your-jira-instance.atlassian.net")
	if err != nil {
		log.Fatal(err)
	}

	// Set API token for authentication
	jiraClient.Auth.SetBasicAuth(mail, token)
}
```

## Step 5: Create a workflow&#x20;

To create a new workflow, we need to the create the `models.WorkflowPayloadScheme` payload struct with the following information.

1. Worfklow Name.
2. Workflow Description.
3. Workflow Statuses.
4. Workflow Transitions.

Let's try to create a workflow with **directed** transitions and **all-to-all** transitions, something like this:

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

### Step 5.1: Extract the status ID's

The first step to create a Jira workflow is recognize what's gonna be the statuses you want to use.

{% hint style="info" %}
<mark style="color:blue;">**Statuses**</mark> represent the different stages that an issue can go through in a workflow.
{% endhint %}

In this particular example, we're needed to use the following statuses:

* Open
* In Progress
* QA
* Waiting for approval
* Escalated
* Closed
* Resolved

```go
var statusesNamesAsSlice = []string{
	"Open", "In Progress", "QA",
	"Waiting for approval", "Escalated",
	"Closed", "Resolved",
}

var statusesAsMap = make(map[string]*models.WorkflowStatusDetailScheme)
for _, statusName := range statusesNamesAsSlice {

	options := &models.WorkflowStatusSearchParams{
		SearchString: statusName,
	}

	page, response, err := instance.Workflow.Status.Search(context.Background(), options, 0, 1)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	var wasFound bool
	for _, status := range page.Values {

		if status.Name == statusName {
			statusesAsMap[status.Name] = status
			wasFound = true
			break
		}
	}

	if !wasFound {

		// If the workflow is not found, it's required to create a new global status
		statusPayload := &models.WorkflowStatusPayloadScheme{
			Statuses: []*models.WorkflowStatusNodeScheme{
				{
					Name:           statusName,
					StatusCategory: "IN_PROGRESS",
				},
			},
			Scope: &models.WorkflowStatusScopeScheme{
				Type: "GLOBAL",
			},
		}

		statuses, response, err := instance.Workflow.Status.Create(context.Background(), statusPayload)
		if err != nil {
			if response != nil {
				log.Println(response.Bytes.String())
				log.Println(response.Code)
			}

			log.Fatal(err)
		}

		for _, status := range statuses {

			if status.Name == statusName {
				statusesAsMap[status.Name] = status
				break
			}
		}
	}
}

for name, data := range statusesAsMap {
	fmt.Println(name, data.ID)
}
```

&#x20;The previously code extracts the status ID's from the Jira instance and if one status is not available on the instance, it'll automatically create the statuses and append the information on the `statusesAsMap` variable.

<div align="left">

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

</div>

With the status ID's, we can proceed with the creation of the workflow statuses payload

```go
var workflowStatuses []*models.WorkflowTransitionScreenScheme
for _, status := range statusesAsMap {

	workflowStatuses = append(workflowStatuses, &models.WorkflowTransitionScreenScheme{
		ID:         status.ID,
		Properties: nil,
	})
}
```

### Step 5.2: Create the workflow transitions

With the statuses id's extracted, we can create a workflow transitions. The transitions define the paths that an issue can take from one status to another.&#x20;

**For example:** an issue in the "**Open**" status can transition to the "**In Progress**" status when work begins on it.&#x20;

There're the conditional validations needed to create a valid workflow transition:

* include one _initial_ transition.
* not use the same name for a _global_ and _directed_ transition.
* have a unique name for each _global_ transition.
* have a unique 'to' status for each _global_ transition.
* have unique names for each transition from a status.
* not have a 'from' status on _initial_ and _global_ transitions.
* have a 'from' status on _directed_ transitions.

{% tabs %}
{% tab title="Transition" %}
<figure><img src="../.gitbook/assets/image (7).png" alt="" width="245"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Code Sample" %}
```go
var workflowTransitions []*models.WorkflowTransitionPayloadScheme
workflowTransitions = append(workflowTransitions, &models.WorkflowTransitionPayloadScheme{
   Name: "Create",
   To:   "1",
   Type: "initial",
})
```
{% endtab %}
{% endtabs %}

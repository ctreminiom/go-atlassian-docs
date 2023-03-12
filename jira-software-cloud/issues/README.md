---
description: This resource represents Jira issues
---

# 🐞 Issues

## Overview

Need help working with issues? In Jira Software, issues help you manage code, estimate workload, and keep track of your team. On this page, you'll find a quick overview of everything that you can do with an issue.

![Issue View](<../../.gitbook/assets/image (8) (1).png>)

{% embed url="https://support.atlassian.com/jira-software-cloud/docs/what-is-an-issue/" %}

This resource represents Jira issues. Use it to:

* create or edit issues, individually or in bulk.
* retrieve metadata about the options for creating or editing issues.
* delete an issue.
* assign a user to an issue.
* get issue changelogs.
* send notifications about an issue.
* get details of the transitions available for an issue.
* transition an issue.

## Custom Fields

In the Jira REST API, custom fields are uniquely identified by the field ID, as the display names are not unique within a Jira instance. For example, you could have two fields named "Escalation date", one with an ID of "12221" and one with an ID of "12222". A custom field is actually referenced by `customfield\_` + the field ID, rather than just the field ID.

{% hint style="info" %}
**Note:** For example, the "Story points" custom field with ID = "10000" is referenced as customfield\_10000 for REST calls. You can get this reference identifier by requesting the create metadata for the issue type.
{% endhint %}

{% hint style="success" %}
We support the following custom field types.

* Group(s) Picker
* User(s) Picker
* URL's
* Text&#x20;
* Number
* Date
* DateTime
* Select
* MultiSelect
* RadioButton
* CheckBox
* Cascading Multiselect
{% endhint %}

The examples below show how to set the values for different types of custom fields in the input data.

```go
// The CustomFields struct is used on the Create(s), Edit methods
var customFields = jira.CustomFields{}

//Groups Picker customfield
err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
if err != nil {
	log.Fatal(err)
}

//Number customfield
err = customFields.Number("customfield_10043", 1000.3232)
if err != nil {
	log.Fatal(err)
}

//User picker customfield
err = customFields.User("customfield_10044", "92c50fd7-4336-4761-abbc-bdd31ecbc380")
if err != nil {
	log.Fatal(err)
}

//Users picker customfield
err = customFields.Users("customfield_10045", []string{"727b5300-78dc-4b92-961b-082676dea3f2", "697c6d52-993d-4ae3-84b2-1842bdb5b7c0"})
if err != nil {
	log.Fatal(err)
}

//Group picker customfield
err = customFields.Group("customfield_10046", "scrum-masters")
if err != nil {
	log.Fatal(err)
}

//URL customfield
err = customFields.URL("customfield_10047", "https://docs.go-atlassian.io/jira-software-cloud/issues")
if err != nil {
	log.Fatal(err)
}

//Text customfield
err = customFields.Text("customfield_10048", "sample text")
if err != nil {
	log.Fatal(err)
}

//Date customfield
err = customFields.Date("customfield_10049", time.Now().AddDate(0, -1, 0))
if err != nil {
	log.Fatal(err)
}

//DateTime customfield
err = customFields.DateTime("customfield_10050", time.Now())
if err != nil {
	log.Fatal(err)
}

//Select customfield
err = customFields.Select("customfield_10051", "Option 1")
if err != nil {
	log.Fatal(err)
}

//MultiSelect customfield 
err = customFields.MultiSelect("customfield_10052", []string{"Option 1", "Option 2", "Option 3"})
if err != nil {
	log.Fatal(err)
}

//RadioButton customfield 
err = customFields.RadioButton("customfield_10053", "Option 1")
if err != nil {
	log.Fatal(err)
}

//CheckBox customfield
err = customFields.CheckBox("customfield_10054", []string{"Option 1", "Option 2"})
if err != nil {
	log.Fatal(err)
}

//Cascading customfield
err = customFields.Cascading("customfield_10056", "America", "Costa Rica")
if err != nil {
	log.Fatal(err)
}
```

## Create Issue

Creates an issue or, where the option to create subtasks is enabled in Jira, a subtask

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var payload = models.IssueSchemeV2{
		Fields: &models.IssueFieldsSchemeV2{
			Summary:   "New summary test",
			Project:   &models.ProjectScheme{ID: "10000"},
			IssueType: &models.IssueTypeScheme{Name: "Story"},
		},
	}

	//CustomFields
	var customFields = models.CustomFields{}
	err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
	if err != nil {
		log.Fatal(err)
	}

	err = customFields.Number("customfield_10043", 1000.3232)
	if err != nil {
		log.Fatal(err)
	}

	newIssue, response, err := atlassian.Issue.Create(context.Background(), &payload, &customFields)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The new issue %v has been created with the ID %v", newIssue.Key, newIssue.ID)
}

```

## Bulk create issue

Creates issues and, where the option to create subtasks is enabled in Jira, subtasks.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	//CustomFields
	var customFields = models.CustomFields{}
	err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
	if err != nil {
		log.Fatal(err)
	}

	err = customFields.Number("customfield_10043", 1000.3232)
	if err != nil {
		log.Fatal(err)
	}

	var issue1 = models.IssueBulkSchemeV2{
		Payload: &models.IssueSchemeV2{
			Fields: &models.IssueFieldsSchemeV2{
				Summary:   "New summary test",
				Project:   &models.ProjectScheme{ID: "10000"},
				IssueType: &models.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var issue2 = models.IssueBulkSchemeV2{
		Payload: &models.IssueSchemeV2{
			Fields: &models.IssueFieldsSchemeV2{
				Summary:   "New summary test",
				Project:   &models.ProjectScheme{ID: "10000"},
				IssueType: &models.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var issue3 = models.IssueBulkSchemeV2{
		Payload: &models.IssueSchemeV2{
			Fields: &models.IssueFieldsSchemeV2{
				Summary:   "New summary test",
				Project:   &models.ProjectScheme{ID: "10000"},
				IssueType: &models.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var payload []*models.IssueBulkSchemeV2
	payload = append(payload, &issue1, &issue2, &issue3)

	newIssues, response, err := atlassian.Issue.Creates(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range newIssues.Issues {
		log.Printf(issue.Key)
	}

	for _, apiError := range newIssues.Errors {
		log.Println(apiError.Status, apiError.Status)
	}
}
```

## Get issue

Returns the details for an issue.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	issue, response, err := atlassian.Issue.Get(context.Background(), "KP-2", nil, []string{"transitions"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issue.Key)
	log.Println(issue.Fields.Reporter.AccountID)

	for _, transition := range issue.Transitions {
		log.Println(transition.Name, transition.ID, transition.To.ID, transition.HasScreen)
	}

	// Check if the issue contains sub-tasks
	if issue.Fields.Subtasks != nil {
		for _, subTask := range issue.Fields.Subtasks {
			log.Println("Sub-Task: ", subTask.Key, subTask.Fields.Summary)
		}
	}
}
```

## Edit issue

Edits an issue. The edits to the issue's fields are defined using `update` and `fields`. There're two ways to edit an issue on Jira, **implicit** and **explicit**.

### Simple update (implicit set via "fields") <a href="#simple-update-implicit-set-via-fields" id="simple-update-implicit-set-via-fields"></a>

The simple way to update an issue is to do a GET, update the values inside "fields", and then PUT back.

* If you PUT back exactly what you GOT, then you are also sending "names", "self", "key", etc. These are ignored.
* You do not need to send all the fields inside "fields". You can just send the fields you want to update. Absent fields are left unchanged. For example, to update the summary:

```go
var payload = jira.IssueScheme{
		Fields: &jira.IssueFieldsScheme{
			Summary: "New summary test test",
		},
	}
```

* Some fields cannot be updated this way (for example, comments). Instead you must use explicit-verb updates (see below). You can tell if a field cannot be implicitly set by the fact it doesn't have a SET verb.
* If you do send along such un-SET-able fields in this manner, they are ignored. This means that you can PUT what you GET, but not all of the document is taken into consideration.

### Operation (verb) based update (explicit verb via "update") <a href="#operation-verb-based-update-explicit-verb-via-update" id="operation-verb-based-update-explicit-verb-via-update"></a>

Each field supports a set of operations (verbs) for mutating the field. You can retrieve the editable fields and the operations they support using the "editmeta" resource.&#x20;

{% hint style="warning" %}
The _editmeta_ endpoint is not mapped, yet refer to this [**ticket**](https://github.com/ctreminiom/go-atlassian/issues/18)
{% endhint %}

The basic operations are defined below (but custom fields can define their own).

The general shape of an update is field, array of verb-value pairs, for example:

```go
//Issue Update Operations
var operations = &jira.UpdateOperations{}

err = operations.AddArrayOperation("custom_field_id", map[string]string{
	"value":   "verb",
	"value": "verb",
	"value": "verb",
	"value":   "verb",
})

if err != nil {
	log.Fatal(err)
}
```

* **SET:** Sets the value of the field. The incoming value must be the same shape as the value of the field from a GET. For example, a string for "summary", and array of strings for "labels".
* **ADD:** Adds an element to a field that is an array. The incoming value must be the same shape as the items of the array in a GET. For example, to add a label:
* **REMOVE:** Removes an element from a field that is an array. The incoming value must be the same shape as the items of the array in a GET (although you can remove parts of the object that are not needed to uniquely identify the object).
* **EDIT:** Edits an element in a field that is an array. The element is indexed/identified by the value itself (usually by id/name/key).

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	var payload = models.IssueSchemeV2{
		Fields: &models.IssueFieldsSchemeV2{
			//		Summary: "New summary test",
		},
	}

	//CustomFields
	var customFields = models.CustomFields{}
	err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
	if err != nil {
		log.Fatal(err)
	}

	err = customFields.Number("customfield_10043", 9000)
	if err != nil {
		log.Fatal(err)
	}

	//Issue Update Operations
	var operations = &models.UpdateOperations{}

	err = operations.AddArrayOperation("labels", map[string]string{
		"triaged":   "remove",
		"triaged-2": "remove",
		"triaged-1": "remove",
		"blocker":   "remove",
	})

	if err != nil {
		log.Fatal(err)
	}

	err = operations.AddStringOperation("summary", "set", "new summary using operation")
	if err != nil {
		log.Fatal(err)
	}

	response, err := atlassian.Issue.Update(context.Background(), "KP-2", false, &payload, &customFields, operations)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Delete issue

Deletes an issue.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	response, err := atlassian.Issue.Delete(context.Background(), "KP-6", false)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Assign issue

Assigns an issue to a user. Use this operation when the calling user does not have the _Edit Issues_ permission but has the _Assign issue_ permission for the project that the issue is in.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	response, err := atlassian.Issue.Assign(context.Background(), "KP-2", "bedc0e56-c9d1-4f5d-b380-cbced9125849")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Send notification for issue

Creates an email notification for an issue and adds it to the mail queue.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	var userRecipients []*models.IssueNotifyUserScheme
	userRecipients = append(userRecipients, &models.IssueNotifyUserScheme{AccountID: "87dde939-73be-465f-83c5-2217fb9dd9de"})
	userRecipients = append(userRecipients, &models.IssueNotifyUserScheme{AccountID: "8abc0d5f-5eb9-48af-bd8d-b83451828a40"})

	var groupsRecipients []*models.IssueNotifyGroupScheme
	groupsRecipients = append(groupsRecipients, &models.IssueNotifyGroupScheme{Name: "jira-users"})
	groupsRecipients = append(groupsRecipients, &models.IssueNotifyGroupScheme{Name: "scrum-masters"})

	opts := &models.IssueNotifyOptionsScheme{

		// The HTML body of the email notification for the issue.
		HTMLBody: "The <strong>latest</strong> test results for this ticket are now available.",

		// The subject of the email notification for the issue.
		// If this is not specified, then the subject is set to the issue key and summary.
		Subject: "SUBJECT EMAIL EXAMPLE",

		// The plain text body of the email notification for the issue.
		// TextBody: "lorem",

		// The recipients of the email notification for the issue.
		To: &models.IssueNotifyToScheme{
			Reporter: true,
			Assignee: true,
			Watchers: true,
			Voters:   true,
			Users:    userRecipients,
			Groups:   groupsRecipients,
		},

		// Restricts the notifications to users with the specified permissions.
		Restrict: nil,
	}

	response, err := atlassian.Issue.Notify(context.Background(), "KP-2", opts)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Get transitions

Returns either all transitions or a transition that can be performed by the user on an issue, based on the issue's status.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	transitions, response, err := atlassian.Issue.Transitions(context.Background(), "KP-2")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, transition := range transitions.Transitions {
		log.Println(transition.ID, transition.Name)
	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTransitionsScheme struct {
	Expand      string                   `json:"expand,omitempty"`
	Transitions []*IssueTransitionScheme `json:"transitions,omitempty"`
}

type IssueTransitionScheme struct {
	ID            string              `json:"id,omitempty"`
	Name          string              `json:"name,omitempty"`
	To            *TransitionToScheme `json:"to,omitempty"`
	HasScreen     bool                `json:"hasScreen,omitempty"`
	IsGlobal      bool                `json:"isGlobal,omitempty"`
	IsInitial     bool                `json:"isInitial,omitempty"`
	IsAvailable   bool                `json:"isAvailable,omitempty"`
	IsConditional bool                `json:"isConditional,omitempty"`
	IsLooped      bool                `json:"isLooped,omitempty"`
}

type TransitionToScheme struct {
	Self           string                `json:"self,omitempty"`
	Description    string                `json:"description,omitempty"`
	IconURL        string                `json:"iconUrl,omitempty"`
	Name           string                `json:"name,omitempty"`
	ID             string                `json:"id,omitempty"`
	StatusCategory *StatusCategoryScheme `json:"statusCategory,omitempty"`
}

type StatusCategoryScheme struct {
	Self      string `json:"self,omitempty"`
	ID        int    `json:"id,omitempty"`
	Key       string `json:"key,omitempty"`
	ColorName string `json:"colorName,omitempty"`
	Name      string `json:"name,omitempty"`
}
```

## Transition issue

Performs an issue transition.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	var payload = &models.IssueSchemeV2{
		Fields: &models.IssueFieldsSchemeV2{
			Summary: "New summary test test",
		},
	}

	//CustomFields
	var customFields = &models.CustomFields{}
	err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
	if err != nil {
		log.Fatal(err)
	}

	//Issue Update Operations
	var operations = &models.UpdateOperations{}

	err = operations.AddArrayOperation("labels", map[string]string{
		"triaged":   "add",
		"triaged-2": "add",
		"triaged-1": "add",
		"blocker":   "add",
	})

	if err != nil {
		log.Fatal(err)
	}

	options := &models.IssueMoveOptionsV2{
		Fields:       payload,
		Operations:   operations,
		CustomFields: customFields,
	}

	response, err := atlassian.Issue.Move(context.Background(), "KP-7", "41", options)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

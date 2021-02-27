---
description: This resource represents Jira issues
---

# 🐛 Issues

## Custom Fields

In the Jira REST API, custom fields are uniquely identified by the field ID, as the display names are not unique within a Jira instance. For example, you could have two fields named "Escalation date", one with an ID of "12221" and one with an ID of "12222". A custom field is actually referenced by `customfield\_` + the field ID, rather than just the field ID.

> For example, the "Story points" custom field with ID = "10000" is referenced as `customfield\_10000` for REST calls. You can get this reference identifier by requesting the create metadata for the issue type.

{% hint style="success" %}
We support the following custom field types.

* Group\(s\) Picker
* User\(s\) Picker
* URL's
* Text 
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

//User picker customfield
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
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var payload = jira.IssueScheme{
		Fields: &jira.IssueFieldsScheme{
			Summary:   "New summary test",
			Project:   &jira.ProjectScheme{ID: "10000"},
			IssueType: &jira.IssueTypeScheme{Name: "Story"},
		},
	}

	//CustomFields
	var customFields = jira.CustomFields{}
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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
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
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}
	
	atlassian.Auth.SetBasicAuth(mail, token)

	//CustomFields
	var customFields = jira.CustomFields{}
	err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
	if err != nil {
		log.Fatal(err)
	}

	err = customFields.Number("customfield_10043", 1000.3232)
	if err != nil {
		log.Fatal(err)
	}

	var issue1 = jira.IssueBulkScheme{
		Payload: &jira.IssueScheme{
			Fields: &jira.IssueFieldsScheme{
				Summary:   "New summary test",
				Project:   &jira.ProjectScheme{ID: "10000"},
				IssueType: &jira.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var issue2 = jira.IssueBulkScheme{
		Payload: &jira.IssueScheme{
			Fields: &jira.IssueFieldsScheme{
				Summary:   "New summary test",
				Project:   &jira.ProjectScheme{ID: "10000"},
				IssueType: &jira.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var issue3 = jira.IssueBulkScheme{
		Payload: &jira.IssueScheme{
			Fields: &jira.IssueFieldsScheme{
				Summary:   "New summary test",
				Project:   &jira.ProjectScheme{ID: "10000"},
				IssueType: &jira.IssueTypeScheme{Name: "Story"},
			},
		},
		CustomFields: &customFields,
	}

	var payload []*jira.IssueBulkScheme
	payload = append(payload, &issue1, &issue2, &issue3)

	newIssues, response, err := atlassian.Issue.Creates(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
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
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issue, response, err := atlassian.Issue.Get(context.Background(), "KP-12", []string{"status"}, []string{"transitions"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issue)
}

```

## Edit issue

Edits an issue. A transition may be applied and issue properties updated as part of the edit.

## Delete issue

Deletes an issue.

## Assign issue

Assigns an issue to a user. Use this operation when the calling user does not have the _Edit Issues_ permission but has the _Assign issue_ permission for the project that the issue is in.

## Send notification for issue

Creates an email notification for an issue and adds it to the mail queue.

## Get transitions

Returns either all transitions or a transition that can be performed by the user on an issue, based on the issue's status.

## Transition issue

Performs an issue transition.

{% hint style="danger" %}
Screen updates are not supported, yet
{% endhint %}


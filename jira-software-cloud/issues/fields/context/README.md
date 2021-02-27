---
description: This resource represents issue custom field contexts.
---

# 🍳 Context

## Get custom field contexts

 Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of [contexts](https://confluence.atlassian.com/adminjiracloud/what-are-custom-field-contexts-991923859.html)

 for a custom field. Contexts can be returned as follows:

* With no other parameters set, all contexts.
* By defining `id` only, all contexts from the list of IDs.
* By defining `isAnyIssueType`, limit the list of contexts returned to either those that apply to all issue types \(true\) or those that apply to only a subset of issue types \(false\)
* By defining `isGlobalContext`, limit the list of contexts return to either those that apply to all projects \(global contexts\) \(true\) or those that apply to only a subset of projects \(false\)

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

	var fieldID = "customfield_10038"

	options := jira.FieldContextOptionsScheme{
		IsAnyIssueType:  true,
		IsGlobalContext: false,
		ContextID:       nil,
	}

	contexts, response, err := atlassian.Issue.Field.Context.Gets(context.Background(), fieldID, &options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, fieldContext := range contexts.Values {
		log.Println(fieldContext)
	}

}

```

## Create custom field context

Creates a custom field context. If `projectIds` is empty, a global context is created. A global context is one that applies to all project. If `issueTypeIds` is empty, the context applies to all issue types, the method returns the following information:

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

	var fieldID = "customfield_10038"

	payload := jira.FieldContextPayloadScheme{
		IssueTypeIDs: []int{10004},
		ProjectIDs:   []int{10000},
		Name:         "Bug fields context",
		Description:  "A context used to define the custom field options for bugs.",
	}

	fmt.Println(payload)

	response, err := atlassian.Issue.Field.Context.Create(context.Background(), fieldID, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

}

```


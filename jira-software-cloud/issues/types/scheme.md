---
description: This resource represents issue type schemes in classic projects
---

# 🎴 Scheme

## Get all issue type schemes

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type schemes.

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueTypeSchemes, response, err := atlassian.Issue.Type.Scheme.Gets(context.Background(), nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issueTypeScheme := range issueTypeSchemes.Values {
		log.Println(issueTypeScheme)
	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTypeSchemePageScheme struct {
   Self       string                   `json:"self,omitempty"`
   NextPage   string                   `json:"nextPage,omitempty"`
   MaxResults int                      `json:"maxResults,omitempty"`
   StartAt    int                      `json:"startAt,omitempty"`
   Total      int                      `json:"total,omitempty"`
   IsLast     bool                     `json:"isLast,omitempty"`
   Values     []*IssueTypeSchemeScheme `json:"values,omitempty"`
}

type IssueTypeSchemeScheme struct {
   ID                 string `json:"id,omitempty"`
   Name               string `json:"name,omitempty"`
   Description        string `json:"description,omitempty"`
   DefaultIssueTypeID string `json:"defaultIssueTypeId,omitempty"`
   IsDefault          bool   `json:"isDefault,omitempty"`
}
```

## Create issue type scheme

Creates an issue type scheme.

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

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	payload := models.IssueTypeSchemePayloadScheme{
		DefaultIssueTypeID: "10001",
		IssueTypeIds:       []string{"10001", "10002", "10005"},
		Name:               "Kanban Issue Type Scheme 1",
		Description:        "A collection of issue types suited to use in a kanban style project.",
	}

	issueTypeSchemeID, response, err := atlassian.Issue.Type.Scheme.Create(context.Background(), &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("issueTypeSchemeID", issueTypeSchemeID)

}
```

## Get issue type scheme items

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue-type scheme items. Only issue type scheme items used in classic projects are returned.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	items, response, err := atlassian.Issue.Type.Scheme.Items(context.Background(), nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range items.Values {
		log.Println(item)
	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTypeSchemeItemPageScheme struct {
   MaxResults int  `json:"maxResults,omitempty"`
   StartAt    int  `json:"startAt,omitempty"`
   Total      int  `json:"total,omitempty"`
   IsLast     bool `json:"isLast,omitempty"`
   Values     []*IssueTypeSchemeMappingScheme `json:"values,omitempty"`
}

type IssueTypeSchemeMappingScheme struct {
   IssueTypeSchemeID string `json:"issueTypeSchemeId,omitempty"`
   IssueTypeID       string `json:"issueTypeId,omitempty"`
}
```

## Get issue type schemes for projects

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type schemes and, for each issue type scheme, a list of the projects that use it. Only issue type schemes used in classic projects are returned.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	issueTypesSchemes, response, err := atlassian.Issue.Type.Scheme.Projects(context.Background(), []int{10000}, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issueTypeScheme := range issueTypesSchemes.Values {
		log.Println(issueTypeScheme.IssueTypeScheme.Name, issueTypeScheme.ProjectIds)
	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type ProjectIssueTypeSchemePageScheme struct {
   MaxResults int                              `json:"maxResults"`
   StartAt    int                              `json:"startAt"`
   Total      int                              `json:"total"`
   IsLast     bool                             `json:"isLast"`
   Values     []*IssueTypeSchemeProjectsScheme `json:"values"`
}

type IssueTypeSchemeProjectsScheme struct {
   IssueTypeScheme *IssueTypeSchemeScheme `json:"issueTypeScheme,omitempty"`
   ProjectIds      []string               `json:"projectIds,omitempty"`
}

type IssueTypeSchemeScheme struct {
	ID                 string `json:"id,omitempty"`
	Name               string `json:"name,omitempty"`
	Description        string `json:"description,omitempty"`
	DefaultIssueTypeID string `json:"defaultIssueTypeId,omitempty"`
	IsDefault          bool   `json:"isDefault,omitempty"`
}
```

## Assign issue type scheme to project

Assigns an issue type scheme to a project. If any issues in the project are assigned issue types not present in the new scheme, the operation will fail. To complete the assignment those issues must be updated to use issue types in the new scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	response, err := atlassian.Issue.Type.Scheme.Assign(context.Background(), "schemeID", "projectID")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Update issue type scheme

Updates an issue type scheme.

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

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	payload := models.IssueTypeSchemePayloadScheme{
		Name:        "Kanban Issue Type Scheme - UPDATED",
		Description: "A collection of issue types suited to use in a kanban style project.- UPDATED",
	}

	response, err := atlassian.Issue.Type.Scheme.Update(context.Background(), 1000, &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Delete issue type scheme

Deletes an issue type scheme. Only issue type schemes used in classic projects can be deleted. Any projects assigned to the scheme are reassigned to the default issue type scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	response, err := atlassian.Issue.Type.Scheme.Delete(context.Background(), 1001)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Add issue types to issue type scheme

Adds issue types to an issue type scheme. The added issue types are appended to the issue types list. If any of the issue types exist in the issue type scheme, the operation fails and no issue types are added.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	response, err := atlassian.Issue.Type.Scheme.Append(context.Background(), 10182, []int{10003, 10000})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Remove issue type from issue type scheme

Removes an issue type from an issue type scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	response, err := atlassian.Issue.Type.Scheme.Remove(context.Background(), 10182, 10003)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

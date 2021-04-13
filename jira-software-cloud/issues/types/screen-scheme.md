---
description: This resource represents issue type screen schemes.
---

# 🛅 Screen Scheme

## Get issue type screen schemes

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type screen schemes. Only issue type screen schemes used in classic projects are returned.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	screenSchemes, response, err := atlassian.Issue.Type.ScreenScheme.Gets(context.Background(), []int{10001, 10002}, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, screenScheme := range screenSchemes.Values {
		log.Println(screenScheme)
	}
}

```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTypeScreenSchemePageScheme struct {
   Self       string                         `json:"self,omitempty"`
   NextPage   string                         `json:"nextPage,omitempty"`
   MaxResults int                            `json:"maxResults,omitempty"`
   StartAt    int                            `json:"startAt,omitempty"`
   Total      int                            `json:"total,omitempty"`
   IsLast     bool                           `json:"isLast,omitempty"`
   Values     []*IssueTypeScreenSchemeScheme `json:"values,omitempty"`
}

type IssueTypeScreenSchemeScheme struct {
   ID          string `json:"id,omitempty"`
   Name        string `json:"name,omitempty"`
   Description string `json:"description,omitempty"`
}
```

## Create issue type screen scheme

Creates an issue-type screen scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := jira.IssueTypeScreenSchemePayloadScheme{
		Name:              "FX 2 Issue Type Screen Scheme",
		IssueTypeMappings: []*jira.IssueTypeScreenSchemeMappingPayloadScheme{
			{
				IssueTypeID:    "default",
				ScreenSchemeID: "10000",
			},
			{
				IssueTypeID:    "10004", // Bug
				ScreenSchemeID: "10002",
			},
		},
	}

	issueTypeScreenSchemeID, response, err := atlassian.Issue.Type.ScreenScheme.Create(context.Background(), &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueTypeScreenSchemeID)
}


```

## Get issue type screen scheme items

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue-type screen scheme items. Only issue type screen schemes used in classic projects are returned.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main()  {

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

	mapping, response, err := atlassian.Issue.Type.ScreenScheme.Mapping(context.Background(), nil, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range mapping.Values {
		log.Println(item)
	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTypeScreenSchemeMappingScheme struct {
	Self       string                             `json:"self,omitempty"`
	NextPage   string                             `json:"nextPage,omitempty"`
	MaxResults int                                `json:"maxResults,omitempty"`
	StartAt    int                                `json:"startAt,omitempty"`
	Total      int                                `json:"total,omitempty"`
	IsLast     bool                               `json:"isLast,omitempty"`
	Values     []*IssueTypeScreenSchemeItemScheme `json:"values,omitempty"`
}

type IssueTypeScreenSchemeItemScheme struct {
	IssueTypeScreenSchemeID string `json:"issueTypeScreenSchemeId,omitempty"`
	IssueTypeID             string `json:"issueTypeId,omitempty"`
	ScreenSchemeID          string `json:"screenSchemeId,omitempty"`
}
```

## Assign issue type screen scheme to project

Assigns an issue-type screen scheme to a project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10002"
		projectID               = "10002"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Assign(context.Background(), issueTypeScreenSchemeID, projectID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Get issue type screen schemes for projects

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type screen schemes and, for each issue type screen scheme, a list of the projects that use it. Only issue type screen schemes used in classic projects are returned.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueTypeScreenSchemes, response, err := atlassian.Issue.Type.ScreenScheme.Projects(context.Background(), []int{10001}, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issueTypeScreenScheme := range issueTypeScreenSchemes.Values {
		log.Println(issueTypeScreenScheme.ProjectIds, issueTypeScreenScheme.IssueTypeScreenScheme.ID)
	}
}

```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type IssueTypeProjectScreenSchemePageScheme struct {
	Self       string                                 `json:"self,omitempty"`
	NextPage   string                                 `json:"nextPage,omitempty"`
	MaxResults int                                    `json:"maxResults,omitempty"`
	StartAt    int                                    `json:"startAt,omitempty"`
	Total      int                                    `json:"total,omitempty"`
	IsLast     bool                                   `json:"isLast,omitempty"`
	Values     []*IssueTypeScreenSchemesProjectScheme `json:"values,omitempty"`
}


type IssueTypeScreenSchemesProjectScheme struct {
	IssueTypeScreenScheme *IssueTypeScreenSchemeScheme `json:"issueTypeScreenScheme,omitempty"`
	ProjectIds            []string                     `json:"projectIds,omitempty"`
}

type IssueTypeScreenSchemeScheme struct {
	ID          string `json:"id,omitempty"`
	Name        string `json:"name,omitempty"`
	Description string `json:"description,omitempty"`
}
```

## Update issue type screen scheme

Updates an issue type screen scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10002"
		name                    = "FX Issue Type Screen Scheme - UPDATED"
		description             = ""
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Update(context.Background(), issueTypeScreenSchemeID, name, description)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Delete issue type screen scheme

Deletes an issue type screen scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10004"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Delete(context.Background(), issueTypeScreenSchemeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Append mappings to issue type screen scheme

Appends issue type to screen scheme mappings to an issue type screen scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var payload = &jira.IssueTypeScreenSchemePayloadScheme{
		IssueTypeMappings: []*jira.IssueTypeScreenSchemeMappingPayloadScheme{
			{
				IssueTypeID:    "10000", // Epic Issue Type
				ScreenSchemeID: "10002",
			},
			{
				IssueTypeID:    "10002", // Task Issue Type
				ScreenSchemeID: "10002",
			},
		},
	}

	var issueTypeScreenSchemeID = "10005"

	response, err := atlassian.Issue.Type.ScreenScheme.Append(context.Background(), issueTypeScreenSchemeID, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Update issue type screen scheme default screen scheme

Updates the default screen scheme of an issue-type screen scheme. The default screen scheme is used for all unmapped issue types.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10005"
		screenSchemeID          = "10002"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.UpdateDefault(context.Background(), issueTypeScreenSchemeID, screenSchemeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Remove mappings from issue type screen scheme

Removes issue type to screen scheme mappings from an issue type screen scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10005"
		issueTypesIDs           = []string{"10002", "10000"}
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Remove(context.Background(), issueTypeScreenSchemeID, issueTypesIDs)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```


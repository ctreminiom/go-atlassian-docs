---
description: This resource represents permission schemes for a project.
---

# 🚧 Permission Schemes

## Get assigned permission scheme

 Gets the [permission scheme](https://confluence.atlassian.com/x/yodKLg) associated with the project.

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

	scheme, response, err := atlassian.Project.Permission.Get(context.Background(), "KP", []string{"all"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(scheme)
}

```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type PermissionSchemeScheme struct {
   Expand      string                       `json:"expand,omitempty"`
   ID          int                          `json:"id,omitempty"`
   Self        string                       `json:"self,omitempty"`
   Name        string                       `json:"name,omitempty"`
   Description string                       `json:"description,omitempty"`
   Permissions []*PermissionGrantScheme     `json:"permissions,omitempty"`
   Scope       *TeamManagedProjectScopeScheme `json:"scope,omitempty"`
}

type PermissionScopeItemScheme struct {
   Self            string                 `json:"self,omitempty"`
   ID              string                 `json:"id,omitempty"`
   Key             string                 `json:"key,omitempty"`
   Name            string                 `json:"name,omitempty"`
   ProjectTypeKey  string                 `json:"projectTypeKey,omitempty"`
   Simplified      bool                   `json:"simplified,omitempty"`
   ProjectCategory *ProjectCategoryScheme `json:"projectCategory,omitempty"`
}
```

## Assign permission scheme

Assigns a permission scheme with a project

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
		projectKeyOrID     = "DUM"
		permissionSchemeID = 10000
	)

	scheme, response, err := atlassian.Project.Permission.Assign(context.Background(), projectKeyOrID, permissionSchemeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(scheme)
}

```

## Get project issue security levels

 Returns all [issue security](https://confluence.atlassian.com/x/J4lKLg) levels for the project that the user has access to.

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

	levels, response, err := atlassian.Project.Permission.SecurityLevels(context.Background(), "KP")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, level := range levels.Levels {
		log.Println(level)
	}
}

```


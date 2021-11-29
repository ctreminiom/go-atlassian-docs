---
description: >-
  This resource represents project components. Uses to get, create, update, and
  delete project components. Also get components for project and get a count of
  issues by component.
---

# 🔮 Components

## Create component

Creates a component. Use components to provide containers for issues within a project.

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

	payload := &models.ComponentPayloadScheme{
		IsAssigneeTypeValid: false,
		Name:                "Component 2",
		Description:         "This is a Jira component",
		Project:             "KP",
		AssigneeType:        "PROJECT_LEAD",
		LeadAccountID:       "5b86be50b8e3cb5895860d6d",
	}

	newComponent, response, err := atlassian.Project.Component.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The new component has been created with the ID %v", newComponent.ID)
}
```

## Get component

Returns a component.

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

	component, response, err := atlassian.Project.Component.Get(context.Background(), "10006")
	if err != nil {
		log.Fatal(err)
	}

	log.Println(component)
	log.Println(response.Endpoint)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ComponentScheme struct {
	Self                string      `json:"self,omitempty"`
	ID                  string      `json:"id,omitempty"`
	Name                string      `json:"name,omitempty"`
	Description         string      `json:"description,omitempty"`
	Lead                *UserScheme `json:"lead,omitempty"`
	LeadUserName        string      `json:"leadUserName,omitempty"`
	AssigneeType        string      `json:"assigneeType,omitempty"`
	Assignee            *UserScheme `json:"assignee,omitempty"`
	RealAssigneeType    string      `json:"realAssigneeType,omitempty"`
	RealAssignee        *UserScheme `json:"realAssignee,omitempty"`
	IsAssigneeTypeValid bool        `json:"isAssigneeTypeValid,omitempty"`
	Project             string      `json:"project,omitempty"`
	ProjectID           int         `json:"projectId,omitempty"`
}
```

## Update component

Updates a component. Any fields included in the request are overwritten. If `leadAccountId` is an empty string ("") the component lead is removed.

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

	payload := &models.ComponentPayloadScheme{
		IsAssigneeTypeValid: false,
		Name:                "Component 1 - UPDATED",
		Description:         "This is a Jira component - UPDATED",
	}

	componentUpdated, response, err := atlassian.Project.Component.Update(context.Background(), "10006", payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println(response.Endpoint)
	log.Println(componentUpdated)
}
```

## Delete component

Deletes a component.

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

	response, err := atlassian.Project.Component.Delete(context.Background(), "10006")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Get component issues count

Returns the counts of issues assigned to the component.

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

	count, response, err := atlassian.Project.Component.Count(context.Background(), "10005")
	if err != nil {
		log.Fatal(err)
	}

	log.Println(count)
	log.Println(response.Endpoint)
}
```

## Get project components paginated

&#x20;Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all components in a project

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

	var projectKey = "KP"
	components, response, err := atlassian.Project.Component.Gets(context.Background(), projectKey)
	if err != nil {
		log.Fatal(err)
	}

	log.Println(response.Endpoint)
	for _, component := range components {
		log.Println(component)
	}
}
```

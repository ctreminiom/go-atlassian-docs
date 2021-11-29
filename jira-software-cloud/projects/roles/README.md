---
description: >-
  This resource represents the roles that users can play in projects. Use this
  resource to get, create, update, and delete project roles.
---

# 💼 Roles

## Get project roles for project

&#x20;Returns a list of [project roles](https://confluence.atlassian.com/x/3odKLg) for the project returning the name and self URL for each role.

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

	roles, response, err := atlassian.Project.Role.Gets(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for key, value := range *roles {
		log.Println(key, value)
	}
}
```

## Get project role for project

Returns a project role's details and actors associated with the project. The list of actors is sorted by display name.

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

	role, response, err := atlassian.Project.Role.Get(context.Background(), "KP", 10005)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(role.ID)
	log.Println(role.Name)
	log.Println(role.Description)
	log.Println(role.Self)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ProjectRoleScheme struct {
   Self             string                         `json:"self,omitempty"`
   Name             string                         `json:"name,omitempty"`
   ID               int                            `json:"id,omitempty"`
   Description      string                         `json:"description,omitempty"`
   Actors           []*RoleActorScheme             `json:"actors,omitempty"`
   Scope            *TeamManagedProjectScopeScheme `json:"scope,omitempty"`
   TranslatedName   string                         `json:"translatedName,omitempty"`
   CurrentUserRole  bool                           `json:"currentUserRole,omitempty"`
   Admin            bool                           `json:"admin,omitempty"`
   RoleConfigurable bool                           `json:"roleConfigurable,omitempty"`
   Default          bool                           `json:"default,omitempty"`
}

type RoleActorScheme struct {
   ID          int    `json:"id,omitempty"`
   DisplayName string `json:"displayName,omitempty"`
   Type        string `json:"type,omitempty"`
   Name        string `json:"name,omitempty"`
   AvatarURL   string `json:"avatarUrl,omitempty"`
   ActorUser   struct {
      AccountID string `json:"accountId,omitempty"`
   } `json:"actorUser,omitempty"`
   ActorGroup *GroupScheme `json:"actorGroup,omitempty"`
}
```

## Get project role details

&#x20;Returns all [project roles](https://confluence.atlassian.com/x/3odKLg) and the details for each role. Note that the list of project roles is common to all projects.

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

	rolesDetails, response, err := atlassian.Project.Role.Details(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, role := range rolesDetails {
		log.Println(role.Name)
		log.Println(role.ID)
		log.Println(role.Admin)
		log.Println(role.TranslatedName)
	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ProjectRoleDetailScheme struct {
   Self             string                         `json:"self,omitempty"`
   Name             string                         `json:"name,omitempty"`
   ID               int                            `json:"id,omitempty"`
   Description      string                         `json:"description,omitempty"`
   Admin            bool                           `json:"admin,omitempty"`
   Scope            *TeamManagedProjectScopeScheme `json:"scope,omitempty"`
   RoleConfigurable bool                           `json:"roleConfigurable,omitempty"`
   TranslatedName   string                         `json:"translatedName,omitempty"`
   Default          bool                           `json:"default,omitempty"`
}
```

## Get all project roles

Gets a list of all project roles, complete with project role details and default actors.

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

	roles, response, err := atlassian.Project.Role.Global(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, role := range roles {
		log.Println(role)
	}
}
```

## Create project role

Creates a new project role with no [default actors](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-resolutions/#api-rest-api-3-resolution-get).&#x20;

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

	var payload = &models.ProjectRolePayloadScheme{
		Name:        "Developers",
		Description: "A project role that represents developers in a project",
	}

	newRole, response, err := atlassian.Project.Role.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(newRole)
}
```

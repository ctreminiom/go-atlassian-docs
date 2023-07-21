---
cover: ../../../.gitbook/assets/leadership-principles_1120x545@2x-1560x760.png
coverY: 0
---

# 💼 Roles

## Get project roles for project

`GET /rest/api/{2-3}/project/{projectIdOrKey}/role`

Returns a list of [project roles](https://confluence.atlassian.com/x/3odKLg) for the project returning the name and self URL for each role.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
{% endcode %}

## Get project role for project

`GET /rest/api/{2-3}/project/{projectIdOrKey}/role/{id}`

Returns a project role's details and actors associated with the project. The list of actors is sorted by display name.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
{% endcode %}

## Get project role details

`GET /rest/api/{2-3}/project/{projectIdOrKey}/roledetails`

Returns all [project roles](https://confluence.atlassian.com/x/3odKLg) and the details for each role. Note that the list of project roles is common to all projects.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
{% endcode %}

## Get all project roles

`GET /rest/api/{2-3}/role`

Gets a list of all project roles, complete with project role details and default actors.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
{% endcode %}

## Create project role

`POST /rest/api/{2-3}/role`

Creates a new project role with no [default actors](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-resolutions/#api-rest-api-3-resolution-get).&#x20;

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
{% endcode %}

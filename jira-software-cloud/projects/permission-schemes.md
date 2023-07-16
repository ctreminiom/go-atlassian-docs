---
description: This resource represents permission schemes for a project.
cover: ../../.gitbook/assets/mb-personalities_1120x545-@2xcompressed-1560x760.png
coverY: 0
---

# 🚧 Permission Schemes

## Get assigned permission scheme

`GET /rest/api/{2-3}/project/{projectKeyOrId}/permissionscheme`&#x20;

Gets the [permission scheme](https://confluence.atlassian.com/x/yodKLg) associated with the project.

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

	scheme, response, err := atlassian.Project.Permission.Get(context.Background(), "KP", []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(scheme)
}
```
{% endcode %}

## Assign permission scheme

`PUT /rest/api/{2-3}/project/{projectKeyOrId}/permissionscheme`

Assigns a permission scheme with a project

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

	var (
		projectKeyOrID     = "KP"
		permissionSchemeID = 10000
	)

	scheme, response, err := atlassian.Project.Permission.Assign(context.Background(), projectKeyOrID, permissionSchemeID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(scheme)
}
```
{% endcode %}

## Get project issue security levels

`GET /rest/api/{2-3}/project/{projectKeyOrId}/securitylevel`

Returns all [issue security](https://confluence.atlassian.com/x/J4lKLg) levels for the project that the user has access to.

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

	levels, response, err := atlassian.Project.Permission.SecurityLevels(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, level := range levels.Levels {
		log.Println(level)
	}
}
```
{% endcode %}

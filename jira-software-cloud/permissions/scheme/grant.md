---
cover: ../../../.gitbook/assets/homegrown-innovation_1120x545@2x-1560x760.png
coverY: -38
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🔑 Grant

## Get permission scheme grants

`GET /rest/api/{2-3}/permissionscheme/{schemeId}/permission`

Returns all permission grants for a permission scheme.

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

	var permissionSchemeID = 10002

	grants, response, err := atlassian.Permission.Scheme.Grant.Gets(context.Background(), permissionSchemeID, []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, grant := range grants.Permissions {
		log.Println(grant)
	}
}
```
{% endcode %}

## Create permission grant

`POST /rest/api/{2-3}/permissionscheme/{schemeId}/permission`

Creates a permission grant in a permission scheme.

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

	var permissionSchemeID = 10001

	grant := &models.PermissionGrantPayloadScheme{
		Holder: &models.PermissionGrantHolderScheme{
			Parameter: "jira-administrators-system",
			Type:      "group",
		},
		Permission: "EDIT_ISSUES",
	}

	permissionGrant, response, err := atlassian.Permission.Scheme.Grant.Create(context.Background(), permissionSchemeID, grant)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(permissionGrant)
}
```
{% endcode %}

## Get permission scheme grant

`GET /rest/api/{2-3}/permissionscheme/{schemeId}/permission/{permissionId}`

Returns a permission grant.

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
		permissionSchemeID = 10002
		permissionGrantID  = 10517
	)

	grant, response, err := atlassian.Permission.Scheme.Grant.Get(context.Background(), permissionSchemeID, permissionGrantID, []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(grant)
}
```
{% endcode %}

## Delete permission scheme grant

`DELETE /rest/api/{2-3}/permissionscheme/{schemeId}/permission/{permissionId}`

Deletes a permission grant from a permission scheme

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
		permissionSchemeID = 10002
		permissionGrantID  = 10517
	)

	response, err := atlassian.Permission.Scheme.Grant.Delete(context.Background(), permissionSchemeID, permissionGrantID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

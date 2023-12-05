---
cover: ../../../.gitbook/assets/growthgauntletcoverillo.jpg
coverY: 0
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

# 📜 Scheme

A permission scheme is a collection of permission grants. A permission grant consists of a `holder` and a `permission`.

The `holder` object contains information about the user or group being granted the permission. For example, the _Administer projects_ permission is granted to a group named _Teams in space administrators_. In this case, the type is `"type": "group"`, and the parameter is the group name, `"parameter": "Teams in space administrators"`. The `holder` object is defined by the following properties:

* `type` Identifies the user or group (see the list of types below).
* `parameter` The value of this property depends on the `type`. For example, if the `type` is a group, then you need to specify the group name.

The following `types` are available. The expected values for the `parameter` are given in parenthesis (some `types` may not have a `parameter`):

<table data-view="cards" data-full-width="true"><thead><tr><th>name</th><th>description</th></tr></thead><tbody><tr><td>name</td><td>description</td></tr><tr><td> <code>anyone</code></td><td>Grant for anonymous users.</td></tr><tr><td> <code>applicationRole</code></td><td> Grant for users with access to the specified application (application name). See <a href="https://confluence.atlassian.com/x/3YxjL">Update product access settings</a> for more information.</td></tr><tr><td> <code>assignee</code></td><td>Grant for the user currently assigned to an issue.</td></tr><tr><td> <code>group</code></td><td>Grant for the specified group (group name).</td></tr><tr><td> <code>groupCustomField</code></td><td>Grant for a user in the group selected in the specified custom field (custom field ID).</td></tr><tr><td> <code>projectLead</code></td><td>Grant for a project lead.</td></tr><tr><td> <code>projectRole</code></td><td>Grant for the specified project role (project role ID).</td></tr><tr><td> <code>reporter</code></td><td>Grant for the user who reported the issue.</td></tr><tr><td> <code>sd.customer.portal.only</code></td><td>Jira Service Desk only. Grants customers permission to access the customer portal but not Jira</td></tr><tr><td> <code>user</code></td><td>Grant for the specified user (user ID - historically this was the userkey but that is deprecated and the account ID should be used).</td></tr><tr><td> <code>userCustomField</code></td><td>Grant for a user selected in the specified custom field (custom field ID).</td></tr></tbody></table>

## Get all permission schemes

`GET /rest/api/{2-3}/permissionscheme`

Returns all permission schemes.

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

	permissionSchemes, response, err := atlassian.Permission.Scheme.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, permissionScheme := range permissionSchemes.PermissionSchemes {
		log.Println(permissionScheme.ID, permissionScheme.Name)
	}
}
```
{% endcode %}

## Get permission scheme

`GET /rest/api/{2-3}/permissionscheme/{schemeId}`

Returns a permission scheme.

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
		permissionSchemeID = 10001
		expand = []string{"field", "group", "permissions", "projectRole", "user"}
	)
	permissionScheme, response, err := atlassian.Permission.Scheme.Get(context.Background(), permissionSchemeID, expand)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(permissionScheme)
}
```
{% endcode %}

## Create permission scheme

`POST /rest/api/{2-3}/permissionscheme`

Creates a new permission scheme. You can create a permission scheme with or without defining a set of permission grants.

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

	payload := &models.PermissionSchemeScheme{
		Name:        "EF Permission Scheme",
		Description: "EF Permission Scheme description",

		Permissions: []*models.PermissionGrantScheme{
			{
				Permission: "ADMINISTER_PROJECTS",
				Holder: &models.PermissionGrantHolderScheme{
					Parameter: "jira-administrators-system",
					Type:      "group",
				},
			},
			{
				Permission: "CLOSE_ISSUES",
				Holder: &models.PermissionGrantHolderScheme{
					Type: "assignee",
				},
			},
		},
	}

	permissionScheme, response, err := atlassian.Permission.Scheme.Create(context.Background(), payload)

	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(permissionScheme)
}
```
{% endcode %}

## Delete permission scheme

`DELETE /rest/api/{2-3}/permissionscheme/{schemeId}`

Deletes a permission scheme.

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

	var permissionSchemeID = 10004
	response, err := atlassian.Permission.Scheme.Delete(context.Background(), permissionSchemeID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Update permission scheme

`PUT /rest/api/{2-3}/permissionscheme/{schemeId}`

Updates a permission scheme

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

	payload := &models.PermissionSchemeScheme{
		Name:        "EF Permission Scheme - UPDATED",
		Description: "EF Permission Scheme description - UPDATED",

		Permissions: []*models.PermissionGrantScheme{
			{
				Permission: "CLOSE_ISSUES",
				Holder: &models.PermissionGrantHolderScheme{
					Parameter: "jira-administrators-system",
					Type:      "group",
				},
			},
		},
	}

	permissionScheme, response, err := atlassian.Permission.Scheme.Update(context.Background(), 10004, payload)

	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(permissionScheme.Name)
	log.Println(permissionScheme.ID)
	log.Println(permissionScheme.Description)
	log.Println(permissionScheme.Self)

	for _, permissionGrant := range permissionScheme.Permissions {
		log.Println(permissionGrant.ID, permissionGrant.Permission)
	}
}
```
{% endcode %}

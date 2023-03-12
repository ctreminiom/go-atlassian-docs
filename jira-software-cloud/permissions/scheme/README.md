# 📜 Scheme

## Overview

A permission scheme is a collection of permission grants. A permission grant consists of a `holder` and a `permission`.

### Holder Object

&#x20;The `holder` object contains information about the user or group being granted the permission. For example, the _Administer projects_ permission is granted to a group named _Teams in space administrators_. In this case, the type is `"type": "group"`, and the parameter is the group name, `"parameter": "Teams in space administrators"`. The `holder` object is defined by the following properties:

* `type` Identifies the user or group (see the list of types below).
* `parameter` The value of this property depends on the `type`. For example, if the `type` is a group, then you need to specify the group name.

The following `types` are available. The expected values for the `parameter` are given in parenthesis (some `types` may not have a `parameter`):

| name                       | description                                                                                                                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  `anyone`                  | Grant for anonymous users.                                                                                                                                                         |
|  `applicationRole`         |  Grant for users with access to the specified application (application name). See [Update product access settings](https://confluence.atlassian.com/x/3YxjL) for more information. |
|  `assignee`                | Grant for the user currently assigned to an issue.                                                                                                                                 |
|  `group`                   | Grant for the specified group (group name).                                                                                                                                        |
|  `groupCustomField`        | Grant for a user in the group selected in the specified custom field (custom field ID).                                                                                            |
|  `projectLead`             | Grant for a project lead.                                                                                                                                                          |
|  `projectRole`             | Grant for the specified project role (project role ID).                                                                                                                            |
|  `reporter`                | Grant for the user who reported the issue.                                                                                                                                         |
|  `sd.customer.portal.only` | Jira Service Desk only. Grants customers permission to access the customer portal but not Jira                                                                                     |
|  `user`                    | Grant for the specified user (user ID - historically this was the userkey but that is deprecated and the account ID should be used).                                               |
|  `userCustomField`         | Grant for a user selected in the specified custom field (custom field ID).                                                                                                         |

## Get all permission schemes

Returns all permission schemes.

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

## Get permission scheme

Returns a permission scheme.

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

## Create permission scheme

Creates a new permission scheme. You can create a permission scheme with or without defining a set of permission grants.

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



## Delete permission scheme

Deletes a permission scheme.

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

## Update permission scheme

Updates a permission scheme

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

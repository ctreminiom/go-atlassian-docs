---
description: >-
  This resource represents permissions. Use it to obtain details of all
  permissions and determine whether the user has certain permissions.
cover: ../../.gitbook/assets/change-management_1120x545@2x-1560x760.png
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

# 🔓 Permissions

## Get my permissions

`GET /rest/api/{2-3}/mypermissions`

Returns a list of permissions indicating which permissions the user has. Details of the user's permissions can be obtained in a global, project, or issue context.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	permissions, response, err := atlassian.Permission.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, permission := range permissions {
		log.Println(permission)
	}
}
```
{% endcode %}

## Check permissions

`POST /rest/api/{2-3}/permissions/check`

* for a list of global permissions, the global permissions are granted to a user.
* for a list of project permissions and lists of projects and issues, for each project permission a list of the projects and issues a user can access or manipulate.

If no account ID is provided, the operation returns details for the logged-in user.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	payload := &models.PermissionCheckPayload{
		GlobalPermissions: []string{"ADMINISTER"},
		AccountID:         "", //
		ProjectPermissions: []*models.BulkProjectPermissionsScheme{
			{
				Issues:      nil,
				Projects:    []int{10000},
				Permissions: []string{"EDIT_ISSUES"},
			},
		},
	}

	grants, response, err := atlassian.Permission.Check(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, permission := range grants.ProjectPermissions {
		log.Println(permission.Permission, permission.Issues)
	}
}
```
{% endcode %}

## Get permitted projects

`POST /rest/api/{2-3}/permissions/project`

Returns all the projects where the user is granted a list of project permissions.

This operation can be accessed anonymously.

{% hint style="warning" %}
CREATE THE CODE SAMPLE
{% endhint %}

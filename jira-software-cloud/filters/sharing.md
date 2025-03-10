---
cover: ../../.gitbook/assets/conways-law_1120x545@2x-1560x760 (1).jpg
coverY: 101
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

# 🤝 Sharing

This resource represents options for sharing filters. Use it to get share scopes as well as add and remove share scopes from filters.

## Get default share scope

`GET /rest/api/3/filter/defaultShareScope`

Returns the default sharing settings for new filters and dashboards for a user, the method returns the following information:

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
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	scope, response, err := atlassian.Filter.Share.Scope(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		return
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Scope", scope)
}
```
{% endcode %}

## Get share permissions

`PUT /rest/api/3/filter/defaultShareScope`

Returns the share permissions for a filter. A filter can be shared with groups, projects, all logged-in users, or the public. Sharing with all logged-in users or the public is known as global share permission, the method returns the following information:

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
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	permissions, response, err := atlassian.Filter.Share.Gets(context.Background(), 1)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for index, permission := range permissions {
		log.Println(index, permission.Type, permission.Type)
	}
}
```
{% endcode %}

## Add share permission

`POST /rest/api/3/filter/{id}/permission`

Add a share permissions to a filter. If you add global share permission (one for all logged-in users or the public) it will overwrite all share permissions for the filter, the method returns the following information:

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

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	/*
		We can add different share permissions, for example:

		---- Project ID only
		payload := jira.PermissionFilterBodyScheme{
				Type:      "project",
				ProjectID: "10000",
			}

		---- Project ID and role ID
		payload := jira.PermissionFilterBodyScheme{
				Type:          "project",
				ProjectID:     "10000",
				ProjectRoleID: "222222",
			}

		==== Group Name
		payload := jira.PermissionFilterBodyScheme{
				Type:          "group",
				GroupName: "jira-users",
			}
	*/

	payload := models.PermissionFilterPayloadScheme{
		Type:      "project",
		ProjectID: "10000",
	}

	permissions, response, err := atlassian.Filter.Share.Add(context.Background(), 1001, &payload)
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for index, permission := range permissions {
		log.Println(index, permission.ID, permission.Type)
	}
}
```
{% endcode %}

## Get share permission

`GET /rest/api/3/filter/{id}/permission`

Returns share permission for a filter. A filter can be shared with groups, projects, all logged-in users, or the public.&#x20;

Sharing with all logged-in users or the public is known as global share permission, the method returns the following information:

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

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	permission, response, err := atlassian.Filter.Share.Get(context.Background(), 10001, 100012)
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(permission)
}
```
{% endcode %}

## Delete share permission

`DELETE /rest/api/3/filter/{id}/permission/{permissionId}`

Deletes share permission from a filter.

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

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Filter.Share.Delete(context.Background(), 11111, 11111)
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Set default share scope

`PUT /rest/api/3/filter/defaultShareScope`

Sets the default sharing for new filters and dashboards for a user.

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

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Filter.Share.SetScope(context.Background(), "GLOBAL")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

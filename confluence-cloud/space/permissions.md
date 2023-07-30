---
cover: ../../.gitbook/assets/team_personality_tests_1120x545@2x-1560x760.jpeg
coverY: 0
---

# 🛡 Permissions

## Add new permission to space

`POST /wiki/rest/api/space/{spaceKey}/permission`

Adds new permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/http"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var (
		spaceKey = "DUMMY"
		payload  = &models.SpacePermissionPayloadScheme{
			Subject: &models.PermissionSubjectScheme{
				Type:       "group",
				Identifier: "confluence-users",
			},
			Operation: &models.SpacePermissionOperationScheme{
				Target: "page",
				Key:    "archive",
			},
		}
	)

	permission, response, err := instance.Space.Permission.Add(context.Background(), spaceKey, payload)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	log.Println(permission.Operation)
	log.Println(permission.Subject)
}
```
{% endcode %}

## Add new custom content permission to space

`POST /wiki/rest/api/space/{spaceKey}/permission/custom-content`

Adds new custom content permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/http"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var (
		spaceKey = "DUMMY"
		payload  = &models.SpacePermissionArrayPayloadScheme{
			Subject: &models.PermissionSubjectScheme{
				Type:       "jira-users-22",
				Identifier: "confluence-users",
			},
			Operations: []*models.SpaceOperationPayloadScheme{
				{
					Key:    "read",
					Target: "space",
					Access: true,
				},
				{
					Key:    "export",
					Target: "space",
					Access: true,
				},
			},
		}
	)

	response, err := instance.Space.Permission.Bulk(context.Background(), spaceKey, payload)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}
```
{% endcode %}

## Remove a space permission

`DELETE /wiki/rest/api/space/{spaceKey}/permission/{id}`

Removes a space permission. Note that removing Read Space permission for a user or group will remove all the space permissions for that user or group.

{% hint style="info" %}
Note: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"net/http"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var (
		spaceKey = "DUMMY"
		permissionID = 10001
	)

	response, err := instance.Space.Permission.Remove(context.Background(), spaceKey, permissionID)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}
```
{% endcode %}

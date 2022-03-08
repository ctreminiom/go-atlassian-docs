# 🛡 Permissions

## Add new permission to space

Adds new permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-post" %}
Official Documentation
{% endembed %}

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
				log.Println(response.API)
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

## Add new custom content permission to space

Adds new custom content permission to space. If the permission to be added is a group permission, the group can be identified by its group name or group id.

{% hint style="info" %}
**Note**: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-custom-content-post" %}
Official Documentation
{% endembed %}

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
				log.Println(response.API)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}

```

## Remove a space permission

Removes a space permission. Note that removing Read Space permission for a user or group will remove all the space permissions for that user or group.

{% hint style="info" %}
Note: Apps cannot access this REST resource - including when utilizing user impersonation.
{% endhint %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-space-permissions#api-wiki-rest-api-space-spacekey-permission-id-delete" %}
Official Documentation
{% endembed %}

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
				log.Println(response.API)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}

```

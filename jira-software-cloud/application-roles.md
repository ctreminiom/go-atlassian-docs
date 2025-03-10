---
cover: ../.gitbook/assets/in-defense-of-meetings-2240x1090-1-1560x760.jpg
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

# 🔐 Application Roles

Jira application roles are a way of managing user permissions and access in Jira. Jira comes with a set of predefined roles that define common types of users, such as administrators, developers, and project managers.&#x20;

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

Each Jira application role has a set of permissions associated with it, which determine what actions a user in that role is allowed to perform. For example, the "Admin" role has permission to manage Jira system settings and users, while the "Developer" role has permission to create and edit issues and view source code.

## Get all application roles

`GET /rest/api/{2-3}/applicationrole`

This endpoint is used to retrieve a list of all application roles that are defined in a Jira instance. This endpoint can be used to get information about the roles that are available in Jira, and to help manage user access and permissions.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	applicationRoles, response, err := jira.Role.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, role := range applicationRoles {
		log.Println(role.Key, role.Name)
	}
}
```
{% endcode %}

## Get application role

`GET /rest/api/{2-3}/applicationrole/{key}`

This method allows you to retrieve information about a specific application role in Jira using the key name as reference.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	role, response, err := jira.Role.Get(context.Background(), "jira-core")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("Status HTTP Response", response.Status)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Application Role Name: %v", role.Name)
	log.Printf("Application Role Key: %v", role.Key)
	log.Printf("Application Role User Count: %v", role.UserCount)
}
```
{% endcode %}

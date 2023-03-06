# 🔐 Application Roles

Jira application roles are a way of managing user permissions and access in Jira. Jira comes with a set of predefined roles that define common types of users, such as administrators, developers, and project managers.&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Each Jira application role has a set of permissions associated with it, which determine what actions a user in that role is allowed to perform. For example, the "Admin" role has permission to manage Jira system settings and users, while the "Developer" role has permission to create and edit issues and view source code.

## Get all application roles

&#x20;This <mark style="color:blue;">**Jira API**</mark> endpoint is used to retrieve a list of all application roles that are defined in a Jira instance. This endpoint can be used to get information about the roles that are available in Jira, and to help manage user access and permissions.

The response from the API is a JSON object that contains information about all of the roles in Jira. The JSON object includes an array of roles, where each role is represented by an object with the following fields:

* `id`: The unique identifier for the role.
* `name`: The name of the role.
* `description`: A description of the role.

For example, the response might look something like this:

<details>

<summary>Response Example</summary>

```json
{
  "size": 2,
  "items": [
    {
      "id": 10002,
      "name": "Administrators",
      "description": "Jira administrators"
    },
    {
      "id": 10001,
      "name": "Users",
      "description": "Jira users"
    }
  ]
}
```

</details>

{% code lineNumbers="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	return
}

```
{% endcode %}

## Get application role

This method allows you to retrieve information about a specific application role in Jira using the key name as reference.

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	return
}
```

---
cover: ../../.gitbook/assets/f7d10368-eaf9-4640-9298-935babada43c-1560x760.jpeg
coverY: 120
---

# 👥 User

The user management REST API lets you manage users (managed accounts) in an organization. Only an organization admin can edit the details of a managed account. As an organization admin with verified domains, you can use the user management REST API to perform operations including:

* Get a list of your permissions to manage a user
* Get information about a user
* Update a user profile or set a user's email address
* Enable or disable a user

![](<../../.gitbook/assets/image (15).png>)

## Get user management permissions

`GET /users/{account_id}/manage`

Returns the set of permissions you have for managing the specified Atlassian account

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var accountID = "606a7ff396e8d60068a5c652"

	permissions, response, err := cloudAdmin.User.Permissions(context.Background(), accountID, nil)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(permissions.APITokenDelete.Allowed)
	log.Println(permissions.EmailSet.Allowed)
}
```
{% endcode %}

## Get profile

`GET /users/{account_id}/manage/profile`

Returns information about a single Atlassian account by ID

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var accountID = "606a7ff396e8d60068a5c652"

	user, response, err := cloudAdmin.User.Get(context.Background(), accountID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(user.Account.Name, user.Account.AccountType)
}
```
{% endcode %}

## Update profile

`PATCH /users/{account_id}/manage/profile`

Updates fields in a user account. The `profile.write` privilege details which fields you can change.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var accountID = "5e5f6a63157ed50cd2b9eaca"

	/*
		-------------- NOTE --------------
		The fields the endpoint can edit depends how you configured the user provisioning, for example:
		If the provisioning is made using the GSuite integration or the SCIM connector, you won't be able to
		edit that field using this endpoint because that field is blocked.

		You can check the availability using the Permission method and searching on the tag permissions.EmailSet.Allowed
		-------------- NOTE --------------
	*/
	var payload = make(map[string]interface{})
	payload["nickname"] = "marshmallow"

	userUpdated, response, err := cloudAdmin.User.Update(context.Background(), accountID, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("New NickName, ", userUpdated.Account.Nickname)
}
```
{% endcode %}

## Disable a user

`POST /users/{account_id}/manage/lifecycle/disable`

Disables the specified user account. The permission to make use of this resource is exposed by the `lifecycle.enablement` privilege.&#x20;

You can optionally set a message associated with the block that will be shown to the user on attempted authentication. If none is supplied, a default message will be used.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	/*
		--------- NOTE ---------
		You can only disable an account if it's not related to a SCIM integration (GSUITE)
		--------- NOTE ---------
	*/

	var accountID = "5e5f6a63157ed50cd2b9eaca"

	response, err := cloudAdmin.User.Disable(context.Background(), accountID, "Sample message")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Enable a user

`POST /users/{account_id}/manage/lifecycle/enable`

Enables the specified user account. The permission to make use of this resource is exposed by the `lifecycle.enablement` privilege.&#x20;

You can optionally set a message associated with the block that will be shown to the user on attempted authentication. If none is supplied, a default message will be used.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	/*
		--------- NOTE ---------
		You can only enable an account if it's not related to a SCIM integration (GSUITE)
		--------- NOTE ---------
	*/

	var accountID = "5e5f6a63157ed50cd2b9eaca"

	response, err := cloudAdmin.User.Enable(context.Background(), accountID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

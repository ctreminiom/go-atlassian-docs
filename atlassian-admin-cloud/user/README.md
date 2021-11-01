# 🙍‍♂️ User

The [user management REST API](https://developer.atlassian.com/cloud/admin/user-management/rest/intro/) lets you manage users (managed accounts) in an organization. Only an organization admin can edit the details of a managed account. As an organization admin with verified domains, you can use the user management REST API to perform operations including:

* Get a list of your permissions to manage a user
* Get information about a user
* Update a user profile or set a user's email address
* Enable or disable a user

![](<../../.gitbook/assets/image (13).png>)

## Get user management permissions

Returns the set of permissions you have for managing the specified Atlassian account

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

## Get profile

Returns information about a single Atlassian account by ID

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type UserScheme struct {
	Account struct {
		AccountID       string `json:"account_id"`
		Name            string `json:"name"`
		Nickname        string `json:"nickname"`
		Zoneinfo        string `json:"zoneinfo"`
		Locale          string `json:"locale"`
		Email           string `json:"email"`
		Picture         string `json:"picture"`
		ExtendedProfile struct {
			JobTitle string `json:"job_title"`
			TeamType string `json:"team_type"`
		} `json:"extended_profile"`
		AccountType     string `json:"account_type"`
		AccountStatus   string `json:"account_status"`
		EmailVerified   bool   `json:"email_verified"`
		PrivacySettings struct {
			Name                        string `json:"name"`
			Nickname                    string `json:"nickname"`
			Picture                     string `json:"picture"`
			ExtendedProfileJobTitle     string `json:"extended_profile.job_title"`
			ExtendedProfileDepartment   string `json:"extended_profile.department"`
			ExtendedProfileOrganization string `json:"extended_profile.organization"`
			ExtendedProfileLocation     string `json:"extended_profile.location"`
			ZoneInfo                    string `json:"zoneinfo"`
			Email                       string `json:"email"`
			ExtendedProfilePhoneNumber  string `json:"extended_profile.phone_number"`
			ExtendedProfileTeamType     string `json:"extended_profile.team_type"`
		} `json:"privacy_settings"`
	} `json:"account"`
}
```

## Update profile

Updates fields in a user account. The `profile.write` privilege details which fields you can change.

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

## Disable a user

Disables the specified user account. The permission to make use of this resource is exposed by the `lifecycle.enablement` privilege. You can optionally set a message associated with the block that will be shown to the user on attempted authentication. If none is supplied, a default message will be used.

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

## Enable a user

Enables the specified user account. The permission to make use of this resource is exposed by the `lifecycle.enablement` privilege. You can optionally set a message associated with the block that will be shown to the user on attempted authentication. If none is supplied, a default message will be used.

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

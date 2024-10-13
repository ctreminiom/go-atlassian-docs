---
cover: ../../.gitbook/assets/2240x1090-1-1560x760.jpg
coverY: 105
---

# 🧙‍♂️ Users

{% hint style="info" %}
The following user attributes can be updated through the user provisioning API.
{% endhint %}

<table data-header-hidden><thead><tr><th align="center">User profile field</th><th align="center">SCIM field</th><th width="262" align="center">Attribute type</th><th>Required<select><option value="b0a9be476af645f5930b232987185685" label="true" color="blue"></option><option value="746f2fa7b62645c786d6dae757ebc6ed" label="false" color="blue"></option></select></th></tr></thead><tbody><tr><td align="center">User profile field</td><td align="center">SCIM field</td><td align="center">Attribute type</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Display name</td><td align="center">displayName</td><td align="center">Singular</td><td><span data-option="b0a9be476af645f5930b232987185685">true</span></td></tr><tr><td align="center">Email address</td><td align="center">emails</td><td align="center">Multi-Valued</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Organization</td><td align="center">organization</td><td align="center">Singular</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Job title</td><td align="center">title</td><td align="center">Singular</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Timezone</td><td align="center">timezone</td><td align="center">Singular</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Department</td><td align="center">department</td><td align="center">Singular</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr><tr><td align="center">Preferred language</td><td align="center">preferredLanguage</td><td align="center">Singular</td><td><span data-option="746f2fa7b62645c786d6dae757ebc6ed">false</span></td></tr></tbody></table>

## Get a user by ID

`GET /scim/directory/{directoryId}/Users/{userId}`

Get a user from a directory by `userId`.

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
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		userID      = "ef5ff80e-9ca6-449c-8cca-5b621085c6c9"
	)

	user, response, err := cloudAdmin.SCIM.User.Get(context.Background(), directoryID, userID, nil, nil)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(user.UserName)
	log.Println(user.Name)

}
```
{% endcode %}

## Update user via user attributes

`PUT /scim/directory/{directoryId}/Users/{userId}`

Updates a user's information in a directory by `userId` via user attributes.&#x20;

* Existing values of unspecified attributes are cleaned.
* User information is replaced attribute-by-attribute, with the exception of immutable and read-only attributes.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		userID      = "ef5ff80e-9ca6-449c-8cca-5b621085c6c9"
	)

	payload := &models.SCIMUserScheme{
		UserName:    "username-updated-with-overwrite-method",
		DisplayName: "AA",
		NickName:    "AA",
		Title:       "AA",
		Department:  "President",
		Emails: []*models.SCIMUserEmailScheme{
			{
				Value:   "carlos@go-atlassian.io",
				Type:    "work",
				Primary: true,
			},
		},
	}
	userUpdated, response, err := cloudAdmin.SCIM.User.Update(context.Background(), directoryID, userID, payload, nil, nil)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(userUpdated.DisplayName)
	log.Println(userUpdated.Active)
	log.Println(userUpdated.Department)

}
```
{% endcode %}

## Deactivate a user

`DELETE /scim/directory/{directoryId}/Users/{userId}`

Deactivate a user by `userId`. The user is not available for future requests until activated again. Any future operation for the deactivated user returns the 404 (resource not found) error.

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
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		userID      = "ef5ff80e-9ca6-449c-8cca-5b621085c6c9"
	)

	response, err := cloudAdmin.SCIM.User.Deactivate(context.Background(), directoryID, userID)
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

## Update user by ID (PATCH)

`PATCH /scim/directory/{directoryId}/Users/{userId}`

This operation updates a user's information in a directory by `userId` via `PATCH`

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		userID      = "ef5ff80e-9ca6-449c-8cca-5b621085c6c9"
	)

	payload := &models.SCIMUserToPathScheme{
		Schemas: []string{"urn:ietf:params:scim:api:messages:2.0:PatchOp"},
	}

	if err = payload.AddStringOperation("replace", "displayName", "Docs Atlassian DisplayName 2"); err != nil {
		log.Fatal(err)
	}

	if err = payload.AddStringOperation("replace", "userName", "user-name-updated2"); err != nil {
		log.Fatal(err)
	}

	if err = payload.AddBoolOperation("replace", "active", false); err != nil {
		log.Fatal(err)
	}

	if err = payload.AddComplexOperation("add", "emails", []*models.SCIMUserComplexOperationScheme{
		{
			Value:     "primary@go-atlassian.io",
			ValueType: "work",
			Primary:   true,
		},
		{
			Value:     "second@go-atlassian.io",
			ValueType: "other",
			Primary:   false,
		},
	}); err != nil {
		log.Fatal(err)
	}

	/*
		payload := &admin.SCIMUserToPathScheme{
			Schemas: []string{"urn:ietf:params:scim:api:messages:2.0:PatchOp"},
			Operations: []*admin.SCIMUserToPathOperationScheme{
				{
					Op:    "replace",
					Path:  "displayName",
					Value: "Docs Atlassian DisplayName",
				},

				{
					Op:    "replace",
					Path:  "userName",
					Value: "user-name-updated",
				},
				{
					Op:    "replace",
					Path:  "name.formatted",
					Value: "Ms. Barbara J Jensen, III",
				},
				{
					Op:    "replace",
					Path:  "name.familyName",
					Value: "Jensen",
				},
				{
					Op:    "replace",
					Path:  "name.givenName",
					Value: "Barbara",
				},
				{
					Op:    "replace",
					Path:  "name.middleName",
					Value: "Jane",
				},
				{
					Op:    "replace",
					Path:  "name.honorificPrefix",
					Value: "Ms.",
				},
				{
					Op:    "replace",
					Path:  "name.honorificSuffix",
					Value: "III",
				},

				{
					Op:    "replace",
					Path:  "nickName",
					Value: "Bobby",
				},

				{
					Op:    "replace",
					Path:  "title",
					Value: "Vice President.",
				},

				{
					Op:    "replace",
					Path:  "title",
					Value: "Vice President.",
				},
			},
		}
	*/

	userUpdated, response, err := cloudAdmin.SCIM.User.Path(context.Background(), directoryID, userID, payload, nil, nil)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(userUpdated.DisplayName)
	log.Println(userUpdated.Active)

	for _, mail := range userUpdated.Emails {
		log.Println(mail)
	}
}
```
{% endcode %}

## Get users

`GET /scim/directory/{directoryId}/Users`

Get users from the specified directory. Filtering is supported with a single exact match (`eq`) against the `userName` and `externalId` attributes.&#x20;

* Pagination is supported.&#x20;
* Sorting is not supported.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	options := &models.SCIMUserGetsOptionsScheme{
		Attributes:         nil,
		ExcludedAttributes: nil,
		Filter:             "",
	}

	users, response, err := cloudAdmin.SCIM.User.Gets(context.Background(), directoryID, options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, user := range users.Resources {
		log.Println(user.ID, user.UserName)
	}

	log.Println(users.ItemsPerPage, users.TotalResults)
}
```
{% endcode %}

## Create a user

`POST /scim/directory/{directoryId}/Users`

Create a user in a directory. An attempt to create an existing user fails with a 409 (Conflict) error. A user account can only be created if it has an email address on a verified domain.&#x20;

If a managed Atlassian account already exists on the Atlassian platform for the specified email address, the user in your identity provider is linked to the user in your Atlassian organization.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	var payload = &models.SCIMUserScheme{
		UserName: "Example Username 4",
		Emails: []*models.SCIMUserEmailScheme{
			{
				Value:   "example-4@go-atlassian.io",
				Type:    "work",
				Primary: true,
			},
		},
		Name: &models.SCIMUserNameScheme{
			Formatted:       "Example Full Name with Last Name",
			FamilyName:      "Example Family Name",
			GivenName:       "Example Name",
			MiddleName:      "Name",
			HonorificPrefix: "",
			HonorificSuffix: "",
		},

		DisplayName:       "Example Display Name 3",
		NickName:          "Example NickName",
		Title:             "Atlassian Administrator",
		PreferredLanguage: "en-US",
		Active:            true,
	}

	newUser, response, err := cloudAdmin.SCIM.User.Create(context.Background(), directoryID, payload, nil, nil)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(newUser.ID)
}
```
{% endcode %}

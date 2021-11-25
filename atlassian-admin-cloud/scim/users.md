# 🤹‍♂️ Users

{% hint style="info" %}
The following user attributes can be updated through the user provisioning API.
{% endhint %}

<table data-header-hidden><thead><tr><th align="center">User profile field</th><th align="center">SCIM field</th><th align="center">Attribute type</th><th data-type="select">Required</th></tr></thead><tbody><tr><td align="center">User profile field</td><td align="center">SCIM field</td><td align="center">Attribute type</td><td></td></tr><tr><td align="center">Display name</td><td align="center">displayName</td><td align="center">Singular</td><td></td></tr><tr><td align="center">Email address</td><td align="center">emails</td><td align="center">Multi-Valued</td><td></td></tr><tr><td align="center">Organization</td><td align="center">organization</td><td align="center">Singular</td><td></td></tr><tr><td align="center">Job title</td><td align="center">title</td><td align="center">Singular</td><td></td></tr><tr><td align="center">Timezone</td><td align="center">timezone</td><td align="center">Singular</td><td></td></tr><tr><td align="center">Department</td><td align="center">department</td><td align="center">Singular</td><td></td></tr><tr><td align="center">Preferred language</td><td align="center">preferredLanguage</td><td align="center">Singular</td><td></td></tr></tbody></table>

## Get a user by ID

Get a user from a directory by `userId`.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMUserScheme struct {
	ID                string                        `json:"id"`
	ExternalID        string                        `json:"externalId"`
	Meta              *SCIMUserMetaScheme           `json:"meta,omitempty"`
	Groups            []*SCIMUserGroupScheme        `json:"groups,omitempty"`
	UserName          string                        `json:"userName,omitempty"`
	Emails            []*SCIMUserEmailScheme        `json:"emails,omitempty"`
	Name              *SCIMUserNameScheme           `json:"name,omitempty"`
	DisplayName       string                        `json:"displayName,omitempty"`
	NickName          string                        `json:"nickName,omitempty"`
	Title             string                        `json:"title,omitempty"`
	PreferredLanguage string                        `json:"preferredLanguage,omitempty"`
	Department        string                        `json:"department,omitempty"`
	Organization      string                        `json:"organization,omitempty"`
	Timezone          string                        `json:"timezone,omitempty"`
	PhoneNumbers      []*SCIMUserPhoneNumberScheme  `json:"phoneNumbers,omitempty"`
	Active            bool                          `json:"active,omitempty"`
	EnterpriseInfo    *SCIMEnterpriseUserInfoScheme `json:"urn:ietf:params:scim:schemas:extension:enterprise:2.1:User,omitempty"`
	SCIMExtension     *SCIMExtensionScheme          `json:"urn:scim:schemas:extension:atlassian-external:1.1,omitempty"`
}
type SCIMUserEmailScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserNameScheme struct {
	Formatted       string `json:"formatted,omitempty"`
	FamilyName      string `json:"familyName,omitempty"`
	GivenName       string `json:"givenName,omitempty"`
	MiddleName      string `json:"middleName,omitempty"`
	HonorificPrefix string `json:"honorificPrefix,omitempty"`
	HonorificSuffix string `json:"honorificSuffix,omitempty"`
}

type SCIMUserPhoneNumberScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserMetaScheme struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}

type SCIMUserGroupScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type SCIMEnterpriseUserInfoScheme struct {
	Organization string `json:"organization,omitempty"`
	Department   string `json:"department,omitempty"`
}

type SCIMExtensionScheme struct {
	AtlassianAccountID string `json:"atlassianAccountId,omitempty"`
}
```

## Update user via user attributes

Updates a user's information in a directory by `userId` via user attributes. User information is replaced attribute-by-attribute, with the exception of immutable and read-only attributes. Existing values of unspecified attributes are cleaned.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMUserScheme struct {
	ID                string                        `json:"id"`
	ExternalID        string                        `json:"externalId"`
	Meta              *SCIMUserMetaScheme           `json:"meta,omitempty"`
	Groups            []*SCIMUserGroupScheme        `json:"groups,omitempty"`
	UserName          string                        `json:"userName,omitempty"`
	Emails            []*SCIMUserEmailScheme        `json:"emails,omitempty"`
	Name              *SCIMUserNameScheme           `json:"name,omitempty"`
	DisplayName       string                        `json:"displayName,omitempty"`
	NickName          string                        `json:"nickName,omitempty"`
	Title             string                        `json:"title,omitempty"`
	PreferredLanguage string                        `json:"preferredLanguage,omitempty"`
	Department        string                        `json:"department,omitempty"`
	Organization      string                        `json:"organization,omitempty"`
	Timezone          string                        `json:"timezone,omitempty"`
	PhoneNumbers      []*SCIMUserPhoneNumberScheme  `json:"phoneNumbers,omitempty"`
	Active            bool                          `json:"active,omitempty"`
	EnterpriseInfo    *SCIMEnterpriseUserInfoScheme `json:"urn:ietf:params:scim:schemas:extension:enterprise:2.1:User,omitempty"`
	SCIMExtension     *SCIMExtensionScheme          `json:"urn:scim:schemas:extension:atlassian-external:1.1,omitempty"`
}
type SCIMUserEmailScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserNameScheme struct {
	Formatted       string `json:"formatted,omitempty"`
	FamilyName      string `json:"familyName,omitempty"`
	GivenName       string `json:"givenName,omitempty"`
	MiddleName      string `json:"middleName,omitempty"`
	HonorificPrefix string `json:"honorificPrefix,omitempty"`
	HonorificSuffix string `json:"honorificSuffix,omitempty"`
}

type SCIMUserPhoneNumberScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserMetaScheme struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}

type SCIMUserGroupScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type SCIMEnterpriseUserInfoScheme struct {
	Organization string `json:"organization,omitempty"`
	Department   string `json:"department,omitempty"`
}

type SCIMExtensionScheme struct {
	AtlassianAccountID string `json:"atlassianAccountId,omitempty"`
}
```

## Deactivate a user

Deactivate a user by `userId`. The user is not available for future requests until activated again. Any future operation for the deactivated user returns the 404 (resource not found) error.

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

## Update user by ID (PATCH)

This operation updates a user's information in a directory by `userId` via `PATCH`

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMUserScheme struct {
	ID                string                        `json:"id"`
	ExternalID        string                        `json:"externalId"`
	Meta              *SCIMUserMetaScheme           `json:"meta,omitempty"`
	Groups            []*SCIMUserGroupScheme        `json:"groups,omitempty"`
	UserName          string                        `json:"userName,omitempty"`
	Emails            []*SCIMUserEmailScheme        `json:"emails,omitempty"`
	Name              *SCIMUserNameScheme           `json:"name,omitempty"`
	DisplayName       string                        `json:"displayName,omitempty"`
	NickName          string                        `json:"nickName,omitempty"`
	Title             string                        `json:"title,omitempty"`
	PreferredLanguage string                        `json:"preferredLanguage,omitempty"`
	Department        string                        `json:"department,omitempty"`
	Organization      string                        `json:"organization,omitempty"`
	Timezone          string                        `json:"timezone,omitempty"`
	PhoneNumbers      []*SCIMUserPhoneNumberScheme  `json:"phoneNumbers,omitempty"`
	Active            bool                          `json:"active,omitempty"`
	EnterpriseInfo    *SCIMEnterpriseUserInfoScheme `json:"urn:ietf:params:scim:schemas:extension:enterprise:2.1:User,omitempty"`
	SCIMExtension     *SCIMExtensionScheme          `json:"urn:scim:schemas:extension:atlassian-external:1.1,omitempty"`
}
type SCIMUserEmailScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserNameScheme struct {
	Formatted       string `json:"formatted,omitempty"`
	FamilyName      string `json:"familyName,omitempty"`
	GivenName       string `json:"givenName,omitempty"`
	MiddleName      string `json:"middleName,omitempty"`
	HonorificPrefix string `json:"honorificPrefix,omitempty"`
	HonorificSuffix string `json:"honorificSuffix,omitempty"`
}

type SCIMUserPhoneNumberScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserMetaScheme struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}

type SCIMUserGroupScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type SCIMEnterpriseUserInfoScheme struct {
	Organization string `json:"organization,omitempty"`
	Department   string `json:"department,omitempty"`
}

type SCIMExtensionScheme struct {
	AtlassianAccountID string `json:"atlassianAccountId,omitempty"`
}
```

## Get users

Get users from the specified directory. Filtering is supported with a single exact match (`eq`) against the `userName` and `externalId` attributes. Pagination is supported. Sorting is not supported.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMUserPageScheme struct {
   Schemas      []string          `json:"schemas,omitempty"`
   TotalResults int               `json:"totalResults,omitempty"`
   StartIndex   int               `json:"startIndex,omitempty"`
   ItemsPerPage int               `json:"itemsPerPage,omitempty"`
   Resources    []*SCIMUserScheme `json:"Resources,omitempty"`
}
```

## Create a user

Create a user in a directory. An attempt to create an existing user fails with a 409 (Conflict) error. A user account can only be created if it has an email address on a verified domain. If a managed Atlassian account already exists on the Atlassian platform for the specified email address, the user in your identity provider is linked to the user in your Atlassian organization.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMUserScheme struct {
	ID                string                        `json:"id"`
	ExternalID        string                        `json:"externalId"`
	Meta              *SCIMUserMetaScheme           `json:"meta,omitempty"`
	Groups            []*SCIMUserGroupScheme        `json:"groups,omitempty"`
	UserName          string                        `json:"userName,omitempty"`
	Emails            []*SCIMUserEmailScheme        `json:"emails,omitempty"`
	Name              *SCIMUserNameScheme           `json:"name,omitempty"`
	DisplayName       string                        `json:"displayName,omitempty"`
	NickName          string                        `json:"nickName,omitempty"`
	Title             string                        `json:"title,omitempty"`
	PreferredLanguage string                        `json:"preferredLanguage,omitempty"`
	Department        string                        `json:"department,omitempty"`
	Organization      string                        `json:"organization,omitempty"`
	Timezone          string                        `json:"timezone,omitempty"`
	PhoneNumbers      []*SCIMUserPhoneNumberScheme  `json:"phoneNumbers,omitempty"`
	Active            bool                          `json:"active,omitempty"`
	EnterpriseInfo    *SCIMEnterpriseUserInfoScheme `json:"urn:ietf:params:scim:schemas:extension:enterprise:2.1:User,omitempty"`
	SCIMExtension     *SCIMExtensionScheme          `json:"urn:scim:schemas:extension:atlassian-external:1.1,omitempty"`
}
type SCIMUserEmailScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserNameScheme struct {
	Formatted       string `json:"formatted,omitempty"`
	FamilyName      string `json:"familyName,omitempty"`
	GivenName       string `json:"givenName,omitempty"`
	MiddleName      string `json:"middleName,omitempty"`
	HonorificPrefix string `json:"honorificPrefix,omitempty"`
	HonorificSuffix string `json:"honorificSuffix,omitempty"`
}

type SCIMUserPhoneNumberScheme struct {
	Value   string `json:"value,omitempty"`
	Type    string `json:"type,omitempty"`
	Primary bool   `json:"primary,omitempty"`
}

type SCIMUserMetaScheme struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}

type SCIMUserGroupScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type SCIMEnterpriseUserInfoScheme struct {
	Organization string `json:"organization,omitempty"`
	Department   string `json:"department,omitempty"`
}

type SCIMExtensionScheme struct {
	AtlassianAccountID string `json:"atlassianAccountId,omitempty"`
}
```

# 🔑 Grant

## Get permission scheme grants

Returns all permission grants for a permission scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var permissionSchemeID = 10002

	grants, response, err := atlassian.Permission.Scheme.Grant.Gets(context.Background(), permissionSchemeID, []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, grant := range grants.Permissions {
		log.Println(grant)
	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type PermissionSchemeGrantsScheme struct {
	Permissions []*PermissionGrantScheme `json:"permissions,omitempty"`
	Expand      string                   `json:"expand,omitempty"`
}

type PermissionGrantScheme struct {
	ID         int                          `json:"id,omitempty"`
	Self       string                       `json:"self,omitempty"`
	Holder     *PermissionGrantHolderScheme `json:"holder,omitempty"`
	Permission string                       `json:"permission,omitempty"`
}

type PermissionGrantHolderScheme struct {
	Type      string `json:"type,omitempty"`
	Parameter string `json:"parameter,omitempty"`
	Expand    string `json:"expand,omitempty"`
}

type PermissionGrantPayloadScheme struct {
	Holder     *PermissionGrantHolderScheme `json:"holder,omitempty"`
	Permission string                       `json:"permission,omitempty"`
}
```

## Create permission grant

Creates a permission grant in a permission scheme.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var permissionSchemeID = 10001

	grant := &jira.PermissionGrantPayloadScheme{
		Holder: &jira.PermissionGrantHolderScheme{
			Parameter: "jira-administrators-system",
			Type:      "group",
		},
		Permission: "EDIT_ISSUES",
	}

	permissionGrant, response, err := atlassian.Permission.Scheme.Grant.Create(context.Background(), permissionSchemeID, grant)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(permissionGrant)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type PermissionGrantScheme struct {
   ID         int                          `json:"id,omitempty"`
   Self       string                       `json:"self,omitempty"`
   Holder     *PermissionGrantHolderScheme `json:"holder,omitempty"`
   Permission string                       `json:"permission,omitempty"`
}

type PermissionGrantHolderScheme struct {
   Type      string `json:"type,omitempty"`
   Parameter string `json:"parameter,omitempty"`
   Expand    string `json:"expand,omitempty"`
}

type PermissionGrantPayloadScheme struct {
   Holder     *PermissionGrantHolderScheme `json:"holder,omitempty"`
   Permission string                       `json:"permission,omitempty"`
}
```

## Get permission scheme grant

Returns a permission grant.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		permissionSchemeID = 10002
		permissionGrantID  = 10517
	)

	grant, response, err := atlassian.Permission.Scheme.Grant.Get(context.Background(), permissionSchemeID, permissionGrantID, []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(grant)
}
```

## Delete permission scheme grant

Deletes a permission grant from a permission scheme

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		permissionSchemeID = 10002
		permissionGrantID  = 10517
	)

	response, err := atlassian.Permission.Scheme.Grant.Delete(context.Background(), permissionSchemeID, permissionGrantID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```


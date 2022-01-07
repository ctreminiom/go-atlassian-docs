---
description: This resource represent users.
---

# 🤓 Users

## Get user

Returns a user.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		accountID = "5b86be50b8e3cb5895860d6d"
		expands   = []string{"groups", "applicationRoles"}
	)

	user, response, err := atlassian.User.Get(context.Background(), accountID, expands)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(user)

}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type UserScheme struct {
   Self             string                      `json:"self,omitempty"`
   Key              string                      `json:"key,omitempty"`
   AccountID        string                      `json:"accountId,omitempty"`
   AccountType      string                      `json:"accountType,omitempty"`
   Name             string                      `json:"name,omitempty"`
   EmailAddress     string                      `json:"emailAddress,omitempty"`
   AvatarUrls       *AvatarURLScheme            `json:"avatarUrls,omitempty"`
   DisplayName      string                      `json:"displayName,omitempty"`
   Active           bool                        `json:"active,omitempty"`
   TimeZone         string                      `json:"timeZone,omitempty"`
   Locale           string                      `json:"locale,omitempty"`
   Groups           *UserGroupsScheme           `json:"groups,omitempty"`
   ApplicationRoles *UserApplicationRolesScheme `json:"applicationRoles,omitempty"`
   Expand           string                      `json:"expand,omitempty"`
}

type UserApplicationRolesScheme struct {
   Size       int                               `json:"size,omitempty"`
   Items      []*UserApplicationRoleItemsScheme `json:"items,omitempty"`
   MaxResults int                               `json:"max-results,omitempty"`
}

type UserApplicationRoleItemsScheme struct {
   Key                  string   `json:"key,omitempty"`
   Groups               []string `json:"groups,omitempty"`
   Name                 string   `json:"name,omitempty"`
   DefaultGroups        []string `json:"defaultGroups,omitempty"`
   SelectedByDefault    bool     `json:"selectedByDefault,omitempty"`
   Defined              bool     `json:"defined,omitempty"`
   NumberOfSeats        int      `json:"numberOfSeats,omitempty"`
   RemainingSeats       int      `json:"remainingSeats,omitempty"`
   UserCount            int      `json:"userCount,omitempty"`
   UserCountDescription string   `json:"userCountDescription,omitempty"`
   HasUnlimitedSeats    bool     `json:"hasUnlimitedSeats,omitempty"`
   Platform             bool     `json:"platform,omitempty"`
}

type UserGroupsScheme struct {
   Size       int                `json:"size,omitempty"`
   Items      []*UserGroupScheme `json:"items,omitempty"`
   MaxResults int                `json:"max-results,omitempty"`
}
```

## Create user

Creates a user. This resource is retained for legacy compatibility. As soon as a more suitable alternative is available this resource will be deprecated.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.UserPayloadScheme{
		EmailAddress: "example1@go-atlassian.io",
		DisplayName:  "Example DisplayName",
		Notification: false,
	}

	newUser, response, err := atlassian.User.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("The new user has been created", newUser.AccountID)
}
```

## Delete user

Deletes a user.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.User.Delete(context.Background(), "607b98df2ad11c0072664322")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Bulk get users

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of the users specified by one or more account IDs.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		accountIDs = []string{"5b86be50b8e3cb5895860d6d"}
		startAt    = 0
		maxResults = 50
	)

	users, response, err := atlassian.User.Find(context.Background(), accountIDs, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, user := range users.Values {
		log.Println(user.DisplayName)
	}
}
```

## Get user groups

Returns the groups to which a user belongs.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var accountID = "5b86be50b8e3cb5895860d6d"
	groups, response, err := atlassian.User.Groups(context.Background(), accountID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, group := range groups {
		log.Println(group)
	}

}

```

## Get all users

Returns a list of all (active and inactive) users.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		startAt    = 0
		maxResults = 50
	)

	users, response, err := atlassian.User.Gets(context.Background(), startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, user := range users {
		log.Println(user)
	}
}
```

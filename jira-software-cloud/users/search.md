---
cover: ../../.gitbook/assets/growthgauntletcoverillo.jpg
coverY: 0
---

# 📠 Search

## Find users assignable to projects

`GET /rest/api/{2-3}/user/assignable/multiProjectSearch`

Returns a list of users who can be assigned issues in one or more projects. The list may be restricted to users whose attributes match a string.

{% code fullWidth="true" %}
```go
package main
import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
		accountID   = ""
		projectKeys = []string{"KP"}
		startAt     = 0
		maxResults  = 50
	)
	users, response, err := atlassian.User.Search.Projects(context.Background(), accountID, projectKeys, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, user := range users {
		log.Println(user)
	}
}
```
{% endcode %}

## Find users

`GET /rest/api/{2-3}/user/search`

Returns a list of users that match the search string and property.

{% code fullWidth="true" %}
```go
package main
import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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
		accountID  = ""
		query      = ""
		startAt    = 0
		maxResults = 50
	)
	users, response, err := atlassian.User.Search.Do(context.Background(), accountID, query, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	for _, user := range users {
		log.Println(user)
	}
}
```
{% endcode %}

## Find users with permissions

`GET /rest/api/{2-3}/user/viewissue/search`

Returns a list of users who fulfill these criteria:

* their user attributes match a search string.
* they have a set of permissions for a project or issue.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	options := &models.UserPermissionCheckParamsScheme{
		Query:      "",
		AccountID:  "5b86be50b8e3cb5895860d6d",
		IssueKey:   "",
		ProjectKey: "KP",
	}

	users, response, err := jira.User.Search.Check(context.Background(), "EDIT_ISSUE", options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	for _, user := range users {
		fmt.Println(user)
	}
}
```
{% endcode %}

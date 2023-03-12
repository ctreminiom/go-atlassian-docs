---
description: >-
  This resource represents information about the current user, such as basic
  details, group membership, application roles, preferences, and locale. Use it
  to get, create, update, and delete.
---

# ⚖ Myself

## Get Current User

Returns details for the current user.

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

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	currentUser, response, err := atlassian.MySelf.Details(context.Background(), nil)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(currentUser.DisplayName, currentUser.Active, currentUser.AccountID)
}
```


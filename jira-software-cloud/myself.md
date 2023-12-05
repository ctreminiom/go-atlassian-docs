---
cover: >-
  ../.gitbook/assets/vanessa_lovegrove_cognitive_overload_1120x545@2x-1560x760.jpeg
coverY: 186
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

# ⚖ Myself

## Get Current User

`GET /rest/api/{2-3}/myself`

Returns details for the current user.

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
{% endcode %}


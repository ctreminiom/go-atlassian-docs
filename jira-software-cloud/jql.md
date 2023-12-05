---
cover: ../.gitbook/assets/f7d10368-eaf9-4640-9298-935babada43c-1560x760.jpeg
coverY: 0
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

# 🔩 JQL

Use it to obtain JQL search auto-complete data and suggestions for use in programmatic construction of queries or custom query builders. It also provides operations to:

* convert one or more JQL queries with user identifiers (username or user key) to equivalent JQL queries with account IDs.
* convert readable details in one or more JQL queries to IDs where a user doesn't have permission to view the entity whose details are readable.

## Parse JQL Query

`POST /rest/api/{2-3}/jql/parse`

Parses and validates JQL queries. The validation is performed in context of the current user.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	queries, response, err := jira.JQL.Parse(context.Background(), "", []string{"project = KP"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	for _, query := range queries.Queries {
		fmt.Println(query)
	}
}
```
{% endcode %}

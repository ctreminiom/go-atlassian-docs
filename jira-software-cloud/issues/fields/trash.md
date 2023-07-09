---
cover: ../../../.gitbook/assets/adaptive_leadership_1120x545px@2x_ok-1560x760.jpg
coverY: 0
---

# 🗑 Trash

## Search Fields In Trash

`GET /rest/api/{2-3}/field/search/trashed`

Search returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of fields in the trash. The list may be restricted to fields whose field name or description partially match a string.&#x20;

Only custom fields can be queried, `type` must be set to `custom`.

{% code fullWidth="true" %}
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

	options := &models.FieldSearchOptionsScheme{
		IDs: nil,
		//Query:   "Application Name",
		OrderBy: "name",
		Expand:  []string{"name", "-trashDate"},
	}

	fields, response, err := atlassian.Issue.Field.Trash.Search(context.Background(), options, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range fields.Values {
		log.Println(field)
	}
}
```
{% endcode %}

## Move Field To Trash

`POST /rest/api/{2-3}/field/{id}/trash`

Move moves a custom field to trash

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	response, err := atlassian.Issue.Field.Trash.Move(context.Background(), "customfield_10020")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endcode %}

## Restore Field

`POST /rest/api/{2-3}/field/{id}/restore`

POST /rest/api/3/field/{id}/restoreRestore restores a custom field from trash

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	response, err := atlassian.Issue.Field.Trash.Restore(context.Background(), "customfield_10020")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

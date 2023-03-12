# 🃏 Fields

## Get fields

Returns system and custom issue fields according to the following rules:

* Fields that cannot be added to the issue navigator are always returned.
* Fields that cannot be placed on an issue screen are always returned.
* Fields that depend on global Jira settings are only returned if the setting is enabled. That is, timetracking fields, subtasks, votes, and watches.

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

	fields, response, err := atlassian.Issue.Field.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range fields {
		log.Println(field)
	}

}
```

## Create custom field

Creates a custom field, the method returns the following information:

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

	fieldNewCreate := models.CustomFieldScheme{
		Name:        "Alliance 2",
		Description: "this is the alliance description field",
		FieldType:   "cascadingselect",
		SearcherKey: "cascadingselectsearcher",
	}

	field, response, err := atlassian.Issue.Field.Create(context.Background(), &fieldNewCreate)
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("field", field)
}
```

## Get fields paginated

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of fields for Classic Jira projects. The list can include:

* all fields.
* specific fields, by defining `id`.
* fields that contain a string in the field name or description, by defining `query`.
* specific fields that contain a string in the field name or description, by defining `id` and `query`.

Only custom fields can be queried, `type` must be set to `custom`. the method returns the following information:

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

	options := models.FieldSearchOptionsScheme{
		Types:   []string{"custom"},
		OrderBy: "lastUsed",
		Expand:  []string{"screensCount", "lastUsed"},
	}

	fields, response, err := atlassian.Issue.Field.Search(context.Background(), &options, 0, 50)
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(fields.IsLast, fields.Total, fields.MaxResults, fields.StartAt)

	for _, field := range fields.Values {
		log.Println(field.ID, field.Description)
	}
}
```

## Delete Field

Deletes a custom field. The custom field is deleted whether it is in the trash or not.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	task, response, err := atlassian.Issue.Field.Delete(context.Background(), "customfield_103034")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(task.Status, task.ID, task.Finished)
}
```

---
description: >-
  This resource represents issue fields, both system and custom fields. Use it
  to get fields, field configurations, and create custom fields.
---

# 💬 Fields

## Get fields

Returns system and custom issue fields according to the following rules:

* Fields that cannot be added to the issue navigator are always returned.
* Fields that cannot be placed on an issue screen are always returned.
* Fields that depend on global Jira settings are only returned if the setting is enabled. That is, timetracking fields, subtasks, votes, and watches.

the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `FieldScheme`struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |

### Example

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	fields, response, err := atlassian.Issue.Field.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range *fields {
		log.Println(field.ID, field.Name, field.Schema.Type)
	}

}

```

### FieldScheme

```go
type FieldScheme struct {
   ID          string   `json:"id"`
   Key         string   `json:"key"`
   Name        string   `json:"name"`
   Custom      bool     `json:"custom"`
   Orderable   bool     `json:"orderable"`
   Navigable   bool     `json:"navigable"`
   Searchable  bool     `json:"searchable"`
   ClauseNames []string `json:"clauseNames"`
   Schema      struct {
      Type   string `json:"type"`
      System string `json:"system"`
   } `json:"schema,omitempty"`
   UntranslatedName string `json:"untranslatedName,omitempty"`
}
```

## Create custom field

Creates a custom field, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FieldScheme` struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| paylod | a `CustomFieldScheme` struct. |

### Example

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	fieldNewCreate := jira.CustomFieldScheme{
		Name:        "Alliance",
		Description: "this is the alliance description field",
		FieldType:   "cascadingselect",
		SearcherKey: "cascadingselectsearcher",
	}

	field, response, err := atlassian.Issue.Field.Create(context.Background(), &fieldNewCreate)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
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

| variable | description |
| :--- | :--- |
| result | A `FieldSearchScheme` struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| opts | A `FieldSearchOptionsScheme` struct. |
| startAt | the pagination start point. |
| maxResult | the maximum values permitted per pagination. |

### Example

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := jira.FieldSearchOptionsScheme{
		Types:   []string{"custom"},
		OrderBy: "lastUsed",
		Expand:  []string{"screensCount", "lastUsed"},
	}

	fields, response, err := atlassian.Issue.Field.Search(context.Background(), &options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(fields.IsLast, fields.Total, fields.MaxResults, fields.StartAt)

	for _, field := range fields.Values {
		log.Println(field.ID, field.Description)
	}

}

```

### FieldSearchOptionsScheme

```go
type FieldSearchOptionsScheme struct {
	Types   []string
	IDs     []string
	Query   string
	OrderBy string
	Expand  []string
}
```

### FieldSearchScheme

```go
type FieldSearchScheme struct {
   MaxResults int  `json:"maxResults"`
   StartAt    int  `json:"startAt"`
   Total      int  `json:"total"`
   IsLast     bool `json:"isLast"`
   Values     []struct {
      ID     string `json:"id"`
      Name   string `json:"name"`
      Schema struct {
         Type   string `json:"type"`
         Items  string `json:"items"`
         System string `json:"system"`
      } `json:"schema,omitempty"`
      Description   string `json:"description"`
      Key           string `json:"key"`
      IsLocked      bool   `json:"isLocked"`
      ScreensCount  int    `json:"screensCount"`
      ContextsCount int    `json:"contextsCount"`
      LastUsed      struct {
         Type string `json:"type"`
      } `json:"lastUsed"`
   } `json:"values"`
}
```


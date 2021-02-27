---
description: >-
  This resource represents custom issue field select list options created in
  Jira or using the REST API. This resource supports the following field types:
---

# 🕧 Option

## Get custom field options

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all custom field option for a context. Options are returned first then cascading options, in the order they display in Jira, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FieldContextOptionScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| opt | A `FieldOptionContextParams` struct |
| startAt | The index of the first item to return in a page of results \(page offset\). |
| maxResults | The maximum number of items to return per page. |

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

	var (
		fieldID   = "customfield_10038"
		contextID = 10138
	)

	options := jira.FieldOptionContextParams{
		FieldID:     fieldID,
		ContextID:   contextID,
		OnlyOptions: false,
	}

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Gets(context.Background(), &options, 0, 60)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, fieldOption := range fieldOptions.Values {
		log.Println(fieldOption)
	}
}

```

### FieldContextOptionScheme

```go
type FieldContextOptionScheme struct {
	MaxResults int  `json:"maxResults,omitempty"`
	StartAt    int  `json:"startAt,omitempty"`
	Total      int  `json:"total,omitempty"`
	IsLast     bool `json:"isLast,omitempty"`
	Values     []FieldContextOptionValueScheme `json:"values,omitempty"`
}

type FieldContextOptionValueScheme struct {
	ID       string `json:"id,omitempty"`
	Value    string `json:"value,omitempty"`
	Disabled bool   `json:"disabled,omitempty"`
	OptionID string `json:"optionId,omitempty"`
}
```

### FieldOptionContextParams

```go
type FieldOptionContextParams struct {
   FieldID     string
   ContextID   int
   OptionID    int
   OnlyOptions bool
}
```

## Create custom field options 

Creates options and, where the custom select field is of the type Select List \(cascading\), cascading options for a custom select field. The options are added to a context of the field. 

The maximum number of options that can be created per request is 1000 and each field can have a maximum of **10000** options, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A FieldContextOptionListScheme struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| fieldID | The ID of the custom field. |
| contextID | The ID of the context. |
| payload | A `CreateCustomFieldOptionPayloadScheme` struct |

### Example 1: Single Choice

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

	var valuesToAdd []jira.FieldContextOptionValueScheme

	value00 := jira.FieldContextOptionValueScheme{
		Value:    "Scranton 1",
		Disabled: false,
	}

	valuesToAdd = append(valuesToAdd, value00)

	var payload = jira.CreateCustomFieldOptionPayloadScheme{Options: valuesToAdd}
	var fieldID = "customfield_10038"
	var contextID = 10140

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Create(context.Background(), fieldID, contextID, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range fieldOptions.Options {
		log.Println(option)
	}
}

```

### Example 1: Cascading Choice

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

	var valuesToAdd []jira.FieldContextOptionValueScheme

	argentinaOption := jira.FieldContextOptionValueScheme{
		OptionID: "10027",
		Value:    "Argentina",
		Disabled: false,
	}

	uruguayOption := jira.FieldContextOptionValueScheme{
		OptionID: "10027",
		Value:    "Uruguay",
		Disabled: false,
	}

	valuesToAdd = append(valuesToAdd, argentinaOption, uruguayOption)

	var payload = jira.CreateCustomFieldOptionPayloadScheme{Options: valuesToAdd}
	var fieldID = "customfield_10037"
	var contextID = 10141

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Create(context.Background(), fieldID, contextID, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range fieldOptions.Options {
		log.Println(option)
	}
}

```

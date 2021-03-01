---
description: >-
  This resource represents custom issue field select list options created in
  Jira or using the REST API. This resource supports the following field types:
---

# 🕧 Option

## Get custom field options

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all custom field option for a context. Options are returned first then cascading options, in the order they display in Jira, the method returns the following information:

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

## Create custom field options 

Creates options and, where the custom select field is of the type Select List \(cascading\), cascading options for a custom select field. The options are added to a context of the field. 

The maximum number of options that can be created per request is 1000 and each field can have a maximum of **10000** options, the method returns the following information:

```go
// EXAMPLE => SINGLE CHOICE
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

```go
// EXAMPLE => CASCADING CHOICE
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

## Update custom field options

Updates the options of a custom field. If any of the options are not found, no options are updated. Options where the values in the request match the current values aren't updated and aren't reported in the response.

{% hint style="warning" %}
Note that this operation **only works for issue field select list options created in Jira or using operations from the** [**Issue custom field options**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-custom-field-options/#api-group-issue-custom-field-options) **resource**, it cannot be used with issue field select list options created by Connect apps.
{% endhint %}

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var optionsToUpdate []jira.FieldContextOptionValueScheme

	Option1 := jira.FieldContextOptionValueScheme{
		ID:       "10058",
		Value:    "Scranton 1",
		Disabled: false,
	}

	Option2 := jira.FieldContextOptionValueScheme{
		ID:       "10059",
		Disabled: true,
	}

	optionsToUpdate = append(optionsToUpdate, Option1, Option2)

	var payload = jira.CustomFieldOptionPayloadScheme{Options: optionsToUpdate}
	var fieldID = "customfield_10047"
	var contextID = 10175

	contextOptions, response, err := atlassian.Issue.Field.Context.Option.Update(context.Background(), fieldID, contextID, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range contextOptions.Options {
		log.Println(option)
	}
}

```

## Delete custom field options

Deletes a custom field option. Options with cascading options cannot be deleted without deleting the cascading options first.

{% hint style="warning" %}
 This operation works for custom field options created in Jira or the operations from this resource. **To work with issue field select list options created for Connect apps use the** [**Issue custom field options \(apps\)**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-custom-field-options/#api-rest-api-3-field-fieldid-context-contextid-option-optionid-delete) **operations.**
{% endhint %}

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var fieldID = "customfield_10047"
	var contextID = 10175
	var optionID = 10061

	response, err := atlassian.Issue.Field.Context.Option.Delete(context.Background(), fieldID, contextID, optionID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```


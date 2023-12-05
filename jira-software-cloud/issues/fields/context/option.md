---
cover: ../../../../.gitbook/assets/f7d10368-eaf9-4640-9298-935babada43c-1560x760.jpeg
coverY: 153
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

# 🕧 Option

This resource represents custom issue field select list options created in Jira or using the REST API. This resource supports the following field types:

* Checkboxes.
* Radio Buttons.
* Select List (single choice).
* Select List (multiple choices).
* Select List (cascading).

## Get custom field options

`GET /rest/api/{2-3}/field/{fieldId}/context/{contextId}/option`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all custom field option for a context. Options are returned first then cascading options, in the order they display in Jira, the method returns the following information:

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

	var (
		fieldID   = "customfield_10038"
		contextID = 10180
	)

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Gets(context.Background(), fieldID, contextID, nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range fieldOptions.Values {
		log.Println(option)
	}
}
```
{% endcode %}

## Create custom field options&#x20;

`POST /rest/api/{2-3}/field/{fieldId}/context/{contextId}/option`

Creates options and, where the custom select field is of the type Select List (cascading), cascading options for a custom select field. The options are added to a context of the field.&#x20;

The maximum number of options that can be created per request is 1000 and each field can have a maximum of **10000** options, the method returns the following information:

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
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

	var (
		fieldID   = "customfield_10038"
		contextID = 10180

		payload = &#x26;models.FieldContextOptionListScheme{
			Options: []*models.CustomFieldContextOptionScheme{

				// Single/Multiple Choice example
				{
					Value:    "Option 3",
					Disabled: false,
				},
				{
					Value:    "Option 4",
					Disabled: false,
				},

				///////////////////////////////////////////
				/*
					// Cascading Choice example
					{
						OptionID: "1027",
						Value:    "Argentina",
						Disabled: false,
					},
					{
						OptionID: "1027",
						Value:    "Uruguay",
						Disabled: false,
					},
				*/

			}}
	)

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Create(context.Background(), fieldID, contextID, payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range fieldOptions.Options {
		log.Println(option)
	}
}
</code></pre>

## Update custom field options

`PUT /rest/api/{2-3}/field/{fieldId}/context/{contextId}/option`

Updates the options of a custom field. If any of the options are not found, no options are updated. Options where the values in the request match the current values aren't updated and aren't reported in the response.

{% hint style="warning" %}
Note that this operation **only works for issue field select list options created in Jira or using operations from the** [**Issue custom field options**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-custom-field-options/#api-group-issue-custom-field-options) **resource**, it cannot be used with issue field select list options created by Connect apps.
{% endhint %}

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

	var (
		fieldID   = "customfield_10038"
		contextID = 10180

		payload = &models.FieldContextOptionListScheme{
			Options: []*models.CustomFieldContextOptionScheme{

				// Single/Multiple Choice example
				{
					ID:       "10064",
					Value:    "Option 3 - Updated",
					Disabled: false,
				},
				{
					ID:       "10065",
					Value:    "Option 4 - Updated",
					Disabled: true,
				},

				///////////////////////////////////////////
				/*
					// Cascading Choice example
					{
						OptionID: "1027",
						Value:    "Argentina",
						Disabled: false,
					},
					{
						OptionID: "1027",
						Value:    "Uruguay",
						Disabled: false,
					},
				*/

			}}
	)

	fieldOptions, response, err := atlassian.Issue.Field.Context.Option.Update(context.Background(), fieldID, contextID, payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, option := range fieldOptions.Options {
		log.Println(option)
	}
}
```
{% endcode %}

## Delete custom field options

`DELETE /rest/api/{2-3}/field/{fieldId}/context/{contextId}/option/{optionId}`

Deletes a custom field option. Options with cascading options cannot be deleted without deleting the cascading options first.

{% hint style="warning" %}
&#x20;This operation works for custom field options created in Jira or the operations from this resource. **To work with issue field select list options created for Connect apps use the** [**Issue custom field options (apps)**](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issue-custom-field-options/#api-rest-api-3-field-fieldid-context-contextid-option-optionid-delete) **operations.**
{% endhint %}

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

	var (
		fieldID             = "customfield_10038"
		contextID, optionID = 10180, 10064
	)

	response, err := atlassian.Issue.Field.Context.Option.Delete(context.Background(), fieldID, contextID, optionID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Reorder custom field options

`PUT /rest/api/{2-3}/field/{fieldId}/context/{contextId}/option/move`

Changes the order of custom field options or cascading options in a context.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
	"sort"
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

	var (
		fieldID   = "customfield_10038"
		contextID = 10180
	)

	log.Println("Getting the field context options")
	options, response, err := atlassian.Issue.Field.Context.Option.Gets(context.Background(), fieldID, contextID, nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	var (
		optionsAsMap = make(map[string]string)
		optionsAsList []string
	)

	for _, option := range options.Values {
		optionsAsList = append(optionsAsList, option.Value)
		optionsAsMap[option.Value] = option.ID
	}

	log.Println("Sorting the fields")
	sort.Strings(optionsAsList)

	log.Println("Creating the new option ID's payload to order")
	var optionsIDsAsList []string
	for _, option := range optionsAsList {
		optionsIDsAsList = append(optionsIDsAsList, optionsAsMap[option])
	}

	var payload = &models.OrderFieldOptionPayloadScheme{
		Position:             "First",
		CustomFieldOptionIds: optionsIDsAsList,
	}

	log.Println("Ordering the options")
	response, err = atlassian.Issue.Field.Context.Option.Order(context.Background(), fieldID, contextID, payload)
	if err != nil {
		if response != nil {
			log.Println("HTTP Endpoint Used", response.Endpoint)
			log.Fatal(err)
		}
		return
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

# 🍤 Items

## Get Field Configuration Items

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all fields for a configuration.

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
{% endhint %}

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	fieldConfigurationID, startAt, maxResults := 10000, 0, 50
	items, response, err := atlassian.Issue.Field.Configuration.Item.Gets(context.Background(), fieldConfigurationID, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range items.Values {

		log.Println("----------------------")
		log.Println(item.ID)
		log.Println(item.Description)
		log.Println(item.IsRequired)
		log.Println(item.IsRequired)
		log.Println(item.Renderer)
		log.Println("----------------------")
	}
}
```

## Update Field Configuration Items

Updates fields in a field configuration. The properties of the field configuration fields provided override the existing values.

* The operation can set the renderer for text fields to the default text renderer (`text-renderer`) or wiki style renderer (`wiki-renderer`). However, the renderer cannot be updated for fields using the autocomplete renderer (`autocomplete-renderer`).

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
{% endhint %}

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main()  {

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

	payload := &models.UpdateFieldConfigurationItemPayloadScheme{
		FieldConfigurationItems: []*models.FieldConfigurationItemScheme{
			{
				ID:          "components",
				IsRequired:  true,
			},
		},
	}


	response, err := atlassian.Issue.Field.Configuration.Item.Update(context.Background(), 10000, payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

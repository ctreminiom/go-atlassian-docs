---
description: >-
  This resource represents issue field configurations. Use it to get, set, and
  delete field configurations and field configuration schemes.
cover: ../../../../.gitbook/assets/adaptive_leadership_1120x545px@2x_ok-1560x760.jpg
coverY: 421
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

# 🖼 Configuration

The Jira Issue Field Configurations define the behavior of fields within Jira issues. A field configuration is a set of rules that determines which fields are available for a particular issue type or project, and whether those fields are required, read-only, or hidden. Field configurations can be customized to meet the specific needs of your organization.

## Get all Field Configurations

`GET /rest/api/{2-3}/fieldconfiguration`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configurations. The list can be for all field configurations or a subset determined by any combination of these criteria:

* a list of field configuration item IDs.
* whether the field configuration is a default.
* whether the field configuration name or description contains a query string.

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
{% endhint %}

This method uses the following parameters:

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><code>startAt</code></td><td>The index of the first item to return in a page of results (page offset).</td></tr><tr><td><code>maxResults</code></td><td>The maximum number of items to return per page.</td></tr><tr><td><code>id's</code></td><td>The list of field configuration IDs.</td></tr><tr><td><code>isDefault</code></td><td>If <em>true</em> returns default field configurations only.</td></tr><tr><td><code>query</code></td><td>The query string used to match against field configuration names and descriptions.</td></tr></tbody></table>

{% code fullWidth="true" %}
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

	fieldConfigurations, response, err := atlassian.Issue.Field.Configuration.Gets(
		context.Background(),
		nil,
		false,
		0,
		50,
	)

	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, configuration := range fieldConfigurations.Values {
		log.Println(configuration)
	}

}
```
{% endcode %}

## Create Field Configuration

`POST /rest/api/{2-3}/fieldconfiguration`

Creates a field configuration. The field configuration is created with the same field properties as the default configuration, with all the fields being optional.

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
{% endhint %}

This method uses the following parameters:

<table data-view="cards"><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><code>description</code></td><td>The description of the field configuration.</td></tr><tr><td><code>name</code></td><td>The name of the field configuration. Must be unique.</td></tr></tbody></table>

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	newFieldConfiguration, response, err := atlassian.Issue.Field.Configuration.Create(
		context.Background(),
		"Story DUMMY Field Configuration",
		"description sample")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	fmt.Println(newFieldConfiguration.ID)
	fmt.Println(newFieldConfiguration.Name)
	fmt.Println(newFieldConfiguration.Description)
	fmt.Println(newFieldConfiguration.IsDefault)
}
```
{% endcode %}

## Update Field Configuration

`PUT /rest/api/{2-3}/fieldconfiguration/{id}`

Updates a field configuration. The name and the description provided in the request override the existing values.

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
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

	response, err := atlassian.Issue.Field.Configuration.Update(context.Background(), 10002, "name updated", "")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Delete Field Configuration

`DELETE /rest/api/{2-3}/fieldconfiguration/{id}`

Deletes a field configuration.

{% hint style="info" %}
Only field configurations used in company-managed (classic) projects are returned
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

	response, err := atlassian.Issue.Field.Configuration.Delete(context.Background(), 10002)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

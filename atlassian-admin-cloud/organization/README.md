---
cover: ../../.gitbook/assets/header.png
coverY: 212
---

# ℹ Organization

When you create a new instance of an Atlassian cloud product, you can manage it from your organization. These products that can be part of your organization include Jira products (Jira Software, Jira Service Management, and Jira Work Management), Confluence, Statuspage, Trello, and Opsgenie.&#x20;

This page will address you about the Organization REST-API that brings you the capability to the following actions:

* A list of organizations.
* Information about an organization.
* Users or domains associated with an organization.
* An audit log of events from an organization.

<div data-full-width="true">

<img src="../../.gitbook/assets/atlassian-admin-orgs.gif" alt="">

</div>

When you manage products from this central location, you have access to all administration settings and billing details. With an Atlassian.

## Get organizations

`GET /admin/v1/orgs`

Returns a list of your organizations (based on your API key).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	organizations, response, err := cloudAdmin.Organization.Gets(context.Background(), "")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, organization := range organizations.Data {
		log.Println(organization.ID, organization.Attributes.Name)
	}
}

```
{% endcode %}

## Get an organization by ID

`GET /admin/v1/orgs/{orgId}`

Returns information about a single organization by ID.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"

	organization, response, err := cloudAdmin.Organization.Get(context.Background(), organizationID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(organization.Data.ID)
}

```
{% endcode %}

## Get users in an organization

`GET /admin/v1/orgs/{orgId}/users`

Returns a list of users in an organization.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		cursor         string
		userChunks     []*models.OrganizationUserPageScheme
	)

	for {

		users, response, err := cloudAdmin.Organization.Users(context.Background(), organizationID, cursor)
		if err != nil {
			if response != nil {
				log.Println("Response HTTP Response", response.Bytes.String())
			}
			log.Fatal(err)
		}

		log.Println("Response HTTP Code", response.Code)
		log.Println("HTTP Endpoint Used", response.Endpoint)

		userChunks = append(userChunks, users)

		if len(users.Links.Next) == 0 {
			break
		}

		//extract the next cursor pagination
		nextAsURL, err := url.Parse(users.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		cursor = nextAsURL.Query().Get("cursor")
	}

	for _, chunk := range userChunks {

		for _, user := range chunk.Data {
			log.Println(user.Email, user.Name)
		}
	}
}
```
{% endcode %}

## Get domains in an organization

`GET /admin/v1/orgs/{orgId}/domains`

Returns a list of domains in an organization one page at a time.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		cursor         string
		domainChunks   []*models.OrganizationDomainPageScheme
	)

	for {

		domains, response, err := cloudAdmin.Organization.Domains(context.Background(), organizationID, cursor)
		if err != nil {
			if response != nil {
				log.Println("Response HTTP Response", response.Bytes.String())
			}
			log.Fatal(err)
		}

		log.Println("Response HTTP Code", response.Code)
		log.Println("HTTP Endpoint Used", response.Endpoint)
		domainChunks = append(domainChunks, domains)

		if len(domains.Links.Next) == 0 {
			break
		}

		//extract the next cursor pagination
		nextAsURL, err := url.Parse(domains.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		cursor = nextAsURL.Query().Get("cursor")
	}

	for _, chunk := range domainChunks {

		for _, domain := range chunk.Data {
			log.Println(domain.ID, domain.Attributes.Name, domain.Attributes.Claim.Status)
		}
	}
}
```
{% endcode %}

## Get domain by ID

`GET /admin/v1/orgs/{orgId}/domains/{domainId}`

Returns information about a single verified domain by ID.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		domainID       = "go-atlassian.io"
	)

	domain, response, err := cloudAdmin.Organization.Domain(context.Background(), organizationID, domainID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(domain.Data.Attributes.Name, domain.Data.Attributes.Claim.Status)
}
```
{% endcode %}

## Get an audit log of events

`GET /admin/v1/orgs/{orgId}/events`

Returns an audit log of events from an organization one page at a time.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
	"time"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		cursor         string
		eventChunks    []*models.OrganizationEventPageScheme
	)

	for {

		opts := &models.OrganizationEventOptScheme{
			Q:      "",
			From:   time.Now().Add(time.Duration(-24) * time.Hour),
			To:     time.Time{},
			Action: "",
		}

		events, response, err := cloudAdmin.Organization.Events(context.Background(), organizationID, opts, cursor)
		if err != nil {
			if response != nil {
				log.Println("Response HTTP Response", response.Bytes.String())
			}
			log.Fatal(err)
		}

		log.Println("Response HTTP Code", response.Code)
		log.Println("HTTP Endpoint Used", response.Endpoint)
		eventChunks = append(eventChunks, events)

		if len(events.Links.Next) == 0 {
			break
		}

		//extract the next cursor pagination
		nextAsURL, err := url.Parse(events.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		cursor = nextAsURL.Query().Get("cursor")
	}

	for _, chunk := range eventChunks {

		for _, event := range chunk.Data {
			log.Println(event.ID, event.Attributes.Action, event.Attributes.Time)
		}
	}
}
```
{% endcode %}

## Get an event by ID

`GET /admin/v1/orgs/{orgId}/events/{eventId}`

Returns information about a single event by ID.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		eventID        = "002ca68b-50b6-47c7-b985-566944bc89e8"
	)

	event, response, err := cloudAdmin.Organization.Event(context.Background(), organizationID, eventID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(event.Data.ID, event.Data.Attributes.Action, event.Data.Attributes.Time)
}
```
{% endcode %}

## Get list of event actions

`GET /admin/v1/orgs/{orgId}/event-actions`

Returns information localized event actions

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/kr/pretty"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"

	actions, response, err := cloudAdmin.Organization.Actions(context.Background(), organizationID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, action := range actions.Data {
		fmt.Printf("%# v\n", pretty.Formatter(action))
	}
}
```
{% endcode %}

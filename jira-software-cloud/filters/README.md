---
cover: >-
  ../../.gitbook/assets/csd-222-t1illustrationrefresh-5-signs-of-a-toxic-work-culture-v4a-1560x760.png
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

# 🗄 Filters

In Jira, a filter is a saved search query that you can use to retrieve a specific set of issues from your Jira instance. A filter can be based on various criteria such as issue type, priority, status, assignee, labels, and more.

Filters can be saved and shared with other users, allowing you to easily collaborate and work together on a specific set of issues. You can also use filters to create custom dashboards and reports to monitor the progress of your team's work.\


<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Create Filter

`POST /rest/api/{2-3}/filter`

This method creates a new filter. The filter is shared according to the [default share scope](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filters/#api-rest-api-3-filter-post). The filter is not selected as a favorite, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"github.com/google/uuid"
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

	newFilterBody := models.FilterPayloadScheme{
		Name:        fmt.Sprintf("Filter #%v", uuid.New().String()),
		Description: "Filter's description",
		JQL:         "issuetype = Bug",
		Favorite:    false,
		SharePermissions: []*models.SharePermissionScheme{
			{
				Type: "project",
				Project: &models.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &models.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	filter, response, err := atlassian.Filter.Create(context.Background(), &newFilterBody)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The filter has been created: %v - %v", filter.ID, filter.Name)
}
```
{% endcode %}

<div data-full-width="true">

<img src="../../.gitbook/assets/image (5) (1).png" alt="Filter permissions on the UI interface">

</div>

## Get Favorites

`GET /rest/api/3/filter/favourite`

This method returns the visible favorite filters of the user, the method returns the following information:

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

	filters, response, err := atlassian.Filter.Favorite(context.Background())
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("favorite filters", len(filters))

	for _, filter := range filters {
		log.Println(filter)
}
```
{% endcode %}

## Get My Filters

`GET /rest/api/{2-3}/filter/my`

Returns the filters owned by the user. If `includeFavourites` is `true`, the user's visible favorite filters are also returned, the method returns the following information:

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	myFilters, response, err := atlassian.Filter.My(context.Background(), false, []string{"sharedUsers", "subscriptions"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("my filters", len(myFilters))

	for _, filter := range myFilters {
		log.Println(filter.ID)

		for _, shareUser := range filter.ShareUsers.Items {
			log.Println(shareUser.Name, shareUser.DisplayName)
		}

		for _, subscription := range filter.Subscriptions.Items {
			log.Println(subscription.ID)
		}
	}
}
```
{% endcode %}

## Search Filters

`GET /rest/api/{2-3}/filter/search`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of filters. Use this operation to get:

* specific filters, by defining `id` only.
* filters that match all of the specified attributes. For example, all filters for a user with a particular word in their name. When multiple attributes are specified only filters matching all attributes are returned.
* 🔒 **Permissions required**:  None, however, only the following filters that match the query parameters are returned:
  * filters owned by the user.
  * filters shared with a group that the user is a member of.
  *   filters shared with a private project that the user has _Browse projects_ [project permission](https://confluence.atlassian.com/x/yodKLg)

      for.
  * filters shared with a public project.
  * filters shared with the public.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	options := models.FilterSearchOptionScheme{
		Name:      "",
		AccountID: "",
		Group:     "",
		ProjectID: 0,
		IDs:       nil,
		OrderBy:   "description",
		Expand:    nil,
	}

	filters, response, err := atlassian.Filter.Search(context.Background(), &options, 0, 10)
	if err != nil {
		if response != nil {
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Filters found", len(filters.Values))
}
```
{% endcode %}

## Get Filter

`GET /rest/api/{2-3}/filter/{id}`

This method returns a filter using the ID as a parameter, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	filter, response, err := atlassian.Filter.Get(context.Background(), 1, nil)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Get Filter result", filter.Name, filter.Name)
}
```
{% endcode %}

## Update Filter

`PUT /rest/api/{2-3}/filter/{id}`

This method updates a filter. Use this operation to update a filter's name, description, JQL, or sharing, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	payload := models.FilterPayloadScheme{
		JQL: "issuetype = Story",
	}

	filter, response, err := atlassian.Filter.Update(context.Background(), 1, &payload)
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("new JQL filter value", filter.Jql)
}
```
{% endcode %}

## Delete Filter

`DELETE /rest/api/{2-3}/filter/{id}`

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	response, err := atlassian.Filter.Delete(context.Background(), 1)
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Change filter owner

`PUT /rest/api/{2-3}/filter/{id}/owner`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Changes the owner of the filter.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
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

	response, err := atlassian.Filter.Change(context.Background(), 1, "asda03333")
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

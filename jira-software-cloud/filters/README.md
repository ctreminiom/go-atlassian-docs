---
description: >-
  This resource represents filters. Use it to get, create, update, or delete
  filters. Also use it to configure the columns for a filter and set favorite
  filters.
---

# ✂️ Filters

## Create Filter

This method creates a new filter. The filter is shared according to the [default share scope](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filters/#api-rest-api-3-filter-post). The filter is not selected as a favorite, the method returns the following information:

| variable | description |
| :--- | :--- |
| filter | The new filter created with the ID attached. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx  | a context.Context instance |
| payload | the payload represents the **`FilterBodyScheme`** struct needed to parse the new filter data _\(name, description, JQL, etc\)_ |

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

	newFilterBody := jira.FilterBodyScheme{
		Name:        fmt.Sprintf("Filter #%v", uuid.New().String()),
		Description: "Filter's description",
		JQL:         "issuetype = Bug",
		Favorite:    false,
	}

	filter, response, err := atlassian.Filter.Create(context.Background(), &newFilterBody)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The filter has been created: %v - %v", filter.ID, filter.Name)

}

```

### **FilterBodyScheme**

```go
type FilterBodyScheme struct {
   Name        string `json:"name,omitempty"`
   Description string `json:"description,omitempty"`
   JQL         string `json:"jql,omitempty"`
   Favorite    bool   `json:"favourite,omitempty"`
}
```

## Get Favorites

This method returns the visible favorite filters of the user, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `FilterScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |

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

	filters, response, err := atlassian.Filter.Favorite(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("favorite filters", len(*filters))

	for _, filter := range *filters {
		log.Println(filter)
	}

}

```

## Get My Filters

Returns the filters owned by the user. If `includeFavourites` is `true`, the user's visible favorite filters are also returned, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `FilterScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| favorites | select if you want to return the user's visible favorite filters |

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	myFilters, response, err := atlassian.Filter.My(context.Background(), false)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("my filters", len(*myFilters))


	for _, filter := range *myFilters {
		log.Println(filter)
	}

}

```

## Search Filters

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of filters. Use this operation to get:

* specific filters, by defining `id` only.
* filters that match all of the specified attributes. For example, all filters for a user with a particular word in their name. When multiple attributes are specified only filters matching all attributes are returned.

The method returns the following information:

| variable | description |
| :--- | :--- |
| result  | A **`FilterSearchScheme`** struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| options | a `FilterSearchOptionScheme` struct with the options to available |
| startAt | the current pagination start index |
| maxResults | the maximum result permitted |

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

	options := jira.FilterSearchOptionScheme{
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
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Filters found", len(filters.Values))
}

```

### **FilterSearchScheme** 

**TODO**

## Get Filter

This method returns a filter using the ID as a parameter, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FilterScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| filterID | The Filter ID  |
| expands | the expand values available on the API [documentation](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filters/#api-rest-api-3-filter-id-get). |

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

	filter, response, err := atlassian.Filter.Get(context.Background(), "$FILTER_ID", []string{})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Get Filter result", filter.Name, filter.Name)
}

```

## Update Filter

This method updates a filter. Use this operation to update a filter's name, description, JQL, or sharing, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FilterScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| filterID | The Filter ID  |
| payload | the payload represents the **`FilterBodyScheme`** struct needed to update the filter |

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

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := jira.FilterBodyScheme{
		JQL: "issuetype = Story",
	}

	filter, response, err := atlassian.Filter.Update(context.Background(), "$FILTER_ID", &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("new JQL filter value", filter.Jql)
}

```

## Delete Filter

This method deletes a filter, the method returns the following information:

| variable | description |
| :--- | :--- |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| FilterID | The filter ID |

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

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Filter.Delete(context.Background(), "$FILTER_ID")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```


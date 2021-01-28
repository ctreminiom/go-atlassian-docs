---
description: >-
  This resource represents issue field configurations. Use it to get, set, and
  delete field configurations and field configuration schemes.
---

# 👁️‍🗨️Configuration

## Get all field configurations

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all field configurations, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `FieldConfigSearchScheme` struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| IDs | The list of field configuration IDs. |
| isDefault | If _true_ returns the default field configuration only. |
| startAt | the pagination start point |
| maxResults | the maximum values permitted per pagination. |

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

	fieldConfigurations, response, err := atlassian.Issue.Field.Configuration.Gets(
		context.Background(),
		nil,
		false,
		0,
		50,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, fieldConfiguration := range fieldConfigurations.Values {
		log.Println(fieldConfiguration)
	}

}

```

### FieldConfigSearchScheme

```go
type FieldConfigSearchScheme struct {
   MaxResults int                 `json:"maxResults"`
   StartAt    int                 `json:"startAt"`
   Total      int                 `json:"total"`
   IsLast     bool                `json:"isLast"`
   Values     []FieldConfigScheme `json:"values"`
}

type FieldConfigScheme struct {
   ID          int    `json:"id"`
   Name        string `json:"name"`
   Description string `json:"description"`
   IsDefault   bool   `json:"isDefault,omitempty"`
}
```

## Get field configuration items

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all fields for a configuration, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FieldConfigItemSearchScheme` struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| fieldConfigID | The field configuration ID |
| startAt | the pagination start index |
| maxResults | the maximum values permitted per pagination. |

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

	configurationItems, response, err := atlassian.Issue.Field.Configuration.Items(context.Background(), "10000", 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range configurationItems.Values {
		log.Println(item)
	}

}

```

### FieldConfigItemSearchScheme

```go
type FieldConfigItemSearchScheme struct {
   MaxResults int  `json:"maxResults"`
   StartAt    int  `json:"startAt"`
   Total      int  `json:"total"`
   IsLast     bool `json:"isLast"`
   Values     []struct {
      ID          string `json:"id"`
      IsHidden    bool   `json:"isHidden"`
      IsRequired  bool   `json:"isRequired"`
      Description string `json:"description,omitempty"`
   } `json:"values"`
}
```

## Get all field configuration schemes

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration schemes, the method returns the following information:

{% hint style="warning" %}
Only field configuration schemes used in classic projects are returned.
{% endhint %}

| variable | description |
| :--- | :--- |
| result | A  `FieldConfigSchemeScheme` struct. |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| IDs | The list of field configuration scheme IDs. |
| startAt | the pagination start index |
| maxResults | the maximum values permitted per pagination. |

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

	schemes, response, err := atlassian.Issue.Field.Configuration.Schemes(context.Background(), nil, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, scheme := range schemes.Values {
		log.Println(scheme)
	}

}

```

### FieldConfigSchemeScheme

```go
type FieldConfigSchemeScheme struct {
   MaxResults int  `json:"maxResults"`
   StartAt    int  `json:"startAt"`
   Total      int  `json:"total"`
   IsLast     bool `json:"isLast"`
   Values     []struct {
      ID          string `json:"id"`
      Name        string `json:"name"`
      Description string `json:"description"`
   } `json:"values"`
}
```

## Get field configuration issue type items

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration issue type items, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `FieldConfigSchemeItemsScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| fieldConfigIDs | The list of field configuration scheme IDs. |
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

	configurationIssueTypeItems, response, err := atlassian.Issue.Field.Configuration.IssueTypeItems(context.Background(), nil, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issueTypeItem := range configurationIssueTypeItems.Values {
		log.Println(issueTypeItem)
	}

}

```

### FieldConfigSchemeItemsScheme

```go
type FieldConfigSchemeItemsScheme struct {
   MaxResults int  `json:"maxResults"`
   StartAt    int  `json:"startAt"`
   Total      int  `json:"total"`
   IsLast     bool `json:"isLast"`
   Values     []struct {
      FieldConfigurationSchemeID string `json:"fieldConfigurationSchemeId"`
      IssueTypeID                string `json:"issueTypeId"`
      FieldConfigurationID       string `json:"fieldConfigurationId"`
   } `json:"values"`
}
```

## Get field configuration schemes for projects

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration schemes and, for each scheme, a list of the projects that use it. The list is sorted by field configuration scheme ID. The first item contains the list of project IDs assigned to the default field configuration scheme, the method returns the following information:

| table | description |
| :--- | :--- |
| result | A `FieldProjectSchemeScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance. |
| projectIDs | The list of project IDs. |
| starAt | The index of the first item to return in a page of results \(page offset\). |
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

	schemes, response, err := atlassian.Issue.Field.Configuration.SchemesByProject(context.Background(), []int{10001, 10000}, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, scheme := range schemes.Values {
		log.Println(scheme)
	}

}

```

### FieldProjectSchemeScheme

```go
type FieldProjectSchemeScheme struct {
   MaxResults int  `json:"maxResults"`
   StartAt    int  `json:"startAt"`
   Total      int  `json:"total"`
   IsLast     bool `json:"isLast"`
   Values     []struct {
      ProjectIds               []string `json:"projectIds"`
      FieldConfigurationScheme struct {
         ID          string `json:"id"`
         Name        string `json:"name"`
         Description string `json:"description"`
      } `json:"fieldConfigurationScheme,omitempty"`
   } `json:"values"`
}
```


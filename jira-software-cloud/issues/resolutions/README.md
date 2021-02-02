---
description: >-
  This resource represents issue resolution values. Use it to obtain a list of
  all issue resolution values and the details of individual resolution values.
---

# 🍀 Resolutions

## Get resolutions

Returns a list of all issue resolution values, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of `IssueResolutionScheme` struct |
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

	resolutions, response, err := atlassian.Issue.Resolution.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for pos, resolution := range *resolutions {
		log.Println(pos, resolution)
	}
}

```

### IssueResolutionScheme

```go
type IssueResolutionScheme struct {
   Self        string `json:"self"`
   ID          string `json:"id"`
   Description string `json:"description"`
   Name        string `json:"name"`
}
```

## Get resolution

Returns an issue resolution value, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `IssueResolutionScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| resolutionID | The ID of the issue resolution value. |

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
	var resolutionID = "10000"

	resolution, response, err := atlassian.Issue.Resolution.Get(context.Background(), resolutionID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(resolution)
}

```


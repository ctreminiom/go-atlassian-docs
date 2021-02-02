---
description: >-
  This resource represents available labels. Use it to get available labels for
  the global label field.
---

# 🏷️ Labels

## Get all labels

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of labels, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `IssueLabelsScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
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

	labels, response, err := atlassian.Issue.Label.Gets(context.Background(), 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, label := range labels.Values {
		log.Println(label)
	}

}

```

### IssueLabelsScheme

```go
type IssueLabelsScheme struct {
   MaxResults int      `json:"maxResults"`
   StartAt    int      `json:"startAt"`
   Total      int      `json:"total"`
   IsLast     bool     `json:"isLast"`
   Values     []string `json:"values"`
}
```


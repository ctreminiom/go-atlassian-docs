---
description: >-
  This resource represents issue link types. Use it to get, create, update, and
  delete link issue types as well as get lists of all link issue types.
---

# 🗞️ Types

## Get issue link types

Returns a list of all issue link types, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `IssueLinkTypeSearchScheme` struct |
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

	linkTypes, response, err := atlassian.Issue.Link.Type.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, linkType := range linkTypes.IssueLinkTypes {
		log.Println(linkType.Name)
	}
}

```

## Create issue link type

Creates an issue link type. Use this operation to create descriptions of the reasons why issues are linked. The issue link type consists of a name and descriptions for a link's inward and outward relationships, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A IssueLinkTypeScheme struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| payload | A `IssueLinkTypePayloadScheme` struct |

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

	payload := jira.IssueLinkTypePayloadScheme{
		Inward:  "Clone/Duplicated by",
		Name:    "Clone/Duplicate",
		Outward: "Clone/Duplicates",
	}

	issueLinkType, response, err := atlassian.Issue.Link.Type.Create(context.Background(), &payload)
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

## Get issue link type

Returns an issue link type, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `IssueLinkTypeScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| issueLinkTypeID | The ID of the issue link type. |

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

	linkType, response, err := atlassian.Issue.Link.Type.Get(context.Background(), "$id")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(linkType)
}

```

## Update issue link type

Updates an issue link type, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `IssueLinkTypeScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| issueLinkTypeID | The ID of the issue link type. |
| payload | A `IssueLinkTypePayloadScheme` struct |

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

	payload := jira.IssueLinkTypePayloadScheme{
		Inward:  "Clone/Duplicated by - Updated",
		Name:    "Clone/Duplicate - Updated",
		Outward: "Clone/Duplicates - Updated",
	}

	issueLinkType, response, err := atlassian.Issue.Link.Type.Update(context.Background(), id, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueLinkType)
}

```

## Delete issue link type

Deletes an issue link type, the method returns the following information:

| variable | description |
| :--- | :--- |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| issueLinkTypeID | The ID of the issue link type. |

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

	response, err := atlassian.Issue.Link.Type.Delete(context.Background(), id)
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


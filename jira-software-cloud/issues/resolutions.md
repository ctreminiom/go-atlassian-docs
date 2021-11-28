---
description: >-
  This resource represents issue resolution values. Use it to obtain a list of
  all issue resolution values and the details of individual resolution values.
---

# 🍀 Resolutions

## Get resolutions

Returns a list of all issue resolution values, the method returns the following information:

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

	resolutions, response, err := atlassian.Issue.Resolution.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for pos, resolution := range resolutions {
		log.Println(pos, resolution)
	}
}
```

## Get resolution

Returns an issue resolution value, the method returns the following information:

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
	var resolutionID = "10000"

	resolution, response, err := atlassian.Issue.Resolution.Get(context.Background(), resolutionID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(resolution)
}
```

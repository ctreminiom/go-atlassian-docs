---
description: >-
  This resource represents issue resolution values. Use it to obtain a list of
  all issue resolution values and the details of individual resolution values.
cover: ../../.gitbook/assets/new-rules-of-productivity_1120x545@2x-1560x760.png
coverY: 0
---

# 🍀 Resolutions

## Get resolutions

`GET /rest/api/{2-3}/resolution`

Returns a list of all issue resolution values, the method returns the following information:

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
{% endcode %}

## Get resolution

`GET /rest/api/{2-3}/resolution/{id}`

Returns an issue resolution value

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
	var resolutionID = "10000"

	resolution, response, err := atlassian.Issue.Resolution.Get(context.Background(), resolutionID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(resolution)
}
```
{% endcode %}

## Create resolution

> No implemented, yet, feel free to open a PR or issue

## Set default resolution

> No implemented, yet, feel free to open a PR or issue

## Move resolutions

> No implemented, yet, feel free to open a PR or issue

## Search resolutions

> No implemented, yet, feel free to open a PR or issue

## Update resolution

> No implemented, yet, feel free to open a PR or issue

## Delete resolution

> No implemented, yet, feel free to open a PR or issue

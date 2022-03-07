---
description: >-
  This resource represents project features. Use it to get the list of features
  for a project and modify the state of a feature
---

# 🙌 Features

## Get project features

Returns the list of features for a project.

{% embed url="https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-project-features#api-group-project-features" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	features, response, err := atlassian.Project.Feature.Gets(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, feature := range features.Features {
		log.Printf("%#v", feature)
	}
}

```

## Set project feature state

Sets the state of a project feature.

{% embed url="https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-project-features#api-rest-api-2-project-projectidorkey-features-featurekey-put" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	projectKeyOrID := "KP"
	featureKey := "jsw.classic.deployments"
	state := "DISABLED"

	features, response, err := atlassian.Project.Feature.Set(context.Background(), projectKeyOrID, featureKey, state)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, feature := range features.Features {
		log.Printf("%#v", feature)
	}
}
```

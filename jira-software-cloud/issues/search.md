---
description: >-
  This resource represents various ways to search for issues. Use it to search
  for issues with a JQL query and find issues to populate an issue picker.
---

# 📌 Search

## Search for issues using JQL \(GET\)

 Searches for issues using [JQL](https://confluence.atlassian.com/x/egORLQ). If the JQL query expression is too large to be encoded as a query parameter, use the [POST](https://docs.go-atlassian.io/jira-software-cloud/issues/search#search-for-issues-using-jql-post) version of this resource.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		jql    = "order by created DESC"
		fields = []string{"status"}
		expand = []string{"changelog", "renderedFields", "names", "schema", "transitions", "operations", "editmeta"}
	)

	issues, response, err := atlassian.Issue.Search.Get(context.Background(), jql, fields, expand, 0, 50, "")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issues.Total)

	for _, issue := range issues.Issues {
		for _, history := range issue.Changelog.Histories {

			for _, item := range history.Items {
				log.Println(issue.Key, item.Field, history.Author.DisplayName)
			}
		}
	}

	for _, issue := range issues.Issues {
		for _, transition := range issue.Transitions {
			log.Println(issue.Key, transition.Name, transition.ID)
		}

	}
}

```

## Search for issues using JQL \(POST\)

 Searches for issues using [JQL](https://confluence.atlassian.com/x/egORLQ). There is a [GET](https://docs.go-atlassian.io/jira-software-cloud/issues/search#search-for-issues-using-jql-get) version of this resource that can be used for smaller JQL query expressions.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		jql    = "order by created DESC"
		fields = []string{"status"}
		expand = []string{"changelog", "renderedFields", "names", "schema", "transitions", "operations", "editmeta"}
	)

	issues, response, err := atlassian.Issue.Search.Post(context.Background(), jql, fields, expand, 0, 50, "")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issues.Total)
}

```


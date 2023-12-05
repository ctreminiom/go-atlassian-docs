---
description: >-
  This resource represents various ways to search for issues. Use it to search
  for issues with a JQL query and find issues to populate an issue picker.
cover: >-
  ../../.gitbook/assets/the-keystroke-that-changed-how-i-worked-forever-compressed-1560x760.gif
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

# 📌 Search

## Search for issues using JQL (GET)

`GET /rest/api/{2-3}/search`

Searches for issues using [JQL](https://confluence.atlassian.com/x/egORLQ). If the JQL query expression is too large to be encoded as a query parameter, use the [POST](https://docs.go-atlassian.io/jira-software-cloud/issues/search#search-for-issues-using-jql-post) version of this resource.

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

	var (
		jql    = "order by created DESC"
		fields = []string{"status"}
		expand = []string{"changelog", "renderedFields", "names", "schema", "transitions", "operations", "editmeta"}
	)

	issues, response, err := atlassian.Issue.Search.Get(context.Background(), jql, fields, expand, 0, 50, "")
	if err != nil {
		log.Fatal(err)
	}

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
{% endcode %}

## Search for issues using JQL (POST)

`POST /rest/api/{2-3}/search`

Searches for issues using [JQL](https://confluence.atlassian.com/x/egORLQ). There is a [GET](https://docs.go-atlassian.io/jira-software-cloud/issues/search#search-for-issues-using-jql-get) version of this resource that can be used for smaller JQL query expressions.

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

	var (
		jql    = "order by created DESC"
		fields = []string{"status"}
		expand = []string{"changelog", "renderedFields", "names", "schema", "transitions", "operations", "editmeta"}
	)

	issues, response, err := atlassian.Issue.Search.Post(context.Background(), jql, fields, expand, 0, 50, "")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issues.Total)
}
```
{% endcode %}

## Check issues against JQL

`POST /rest/api/{2-3}/jql/match`

Checks whether one or more issues would be returned by one or more JQL queries.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.IssueSearchCheckPayloadScheme{
		IssueIds: []int{10036},
		JQLs:     []string{"issuekey = KP-23"},
	}

	matches, response, err := atlassian.Issue.Search.Checks(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, match := range matches.Matches {
		fmt.Println(match)
	}
}

```
{% endcode %}

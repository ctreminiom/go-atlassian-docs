---
cover: ../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
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

# 📚 Knowledgebase

## Search articles

`GET /rest/servicedeskapi/knowledgebase/article`

Returns articles that match the given query string across all service desks.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		query = "login"
		start = 0
		limit = 50
	)

	articles, response, err := atlassian.Knowledgebase.Search(
		context.Background(),
		query,
		false,
		start, limit,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, article := range articles.Values {
		log.Println(article)
	}

}
```
{% endcode %}

## Get articles

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/knowledgebase/article`

Returns articles that match the given query and belong to the knowledge base linked to the service desk.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		serviceDeskID = 1
		query         = "login"
		start         = 0
		limit         = 50
	)

	articles, response, err := atlassian.Knowledgebase.Gets(
		context.Background(),
		serviceDeskID,
		query,
		false,
		start, limit,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, article := range articles.Values {
		log.Println(article)
	}
}
```
{% endcode %}

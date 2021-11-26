# 📚 Knowledgebase

## Search articles

Returns articles that match the given query string across all service desks.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ArticlePageScheme struct {
	Size       int                    `json:"size,omitempty"`
	Start      int                    `json:"start,omitempty"`
	Limit      int                    `json:"limit,omitempty"`
	IsLastPage bool                   `json:"isLastPage,omitempty"`
	Values     []*ArticleScheme       `json:"values,omitempty"`
	Expands    []string               `json:"_expands,omitempty"`
	Links      *ArticlePageLinkScheme `json:"_links,omitempty"`
}

type ArticlePageLinkScheme struct {
	Self    string `json:"self,omitempty"`
	Base    string `json:"base,omitempty"`
	Context string `json:"context,omitempty"`
	Next    string `json:"next,omitempty"`
	Prev    string `json:"prev,omitempty"`
}

type ArticleScheme struct {
	Title   string                `json:"title,omitempty"`
	Excerpt string                `json:"excerpt,omitempty"`
	Source  *ArticleSourceScheme  `json:"source,omitempty"`
	Content *ArticleContentScheme `json:"content,omitempty"`
}

type ArticleSourceScheme struct {
	Type string `json:"type,omitempty"`
}

type ArticleContentScheme struct {
	IframeSrc string `json:"iframeSrc,omitempty"`
}
```

## Get articles

Returns articles that match the given query and belong to the knowledge base linked to the service desk.

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

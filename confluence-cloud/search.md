# 🔎 Search

## Search Content

Searches for content using the [Confluence Query Language (CQL)](https://developer.atlassian.com/cloud/confluence/advanced-searching-using-cql/)

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-search#api-wiki-rest-api-search-get" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
	"strconv"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	ctx := context.Background()
	cql := "type = page"

	options := &models.SearchContentOptions{Limit:  2}

	for {

		fmt.Println("cursor ==>", options.Cursor)
		fmt.Println("startAt ==>", options.Start)

		contents, response, err := instance.Search.Content(ctx, cql, options)
		if err != nil {
			if response != nil {
				log.Println(response.Bytes.String())
				log.Println("Endpoint:", response.Endpoint)
			}
			log.Fatal(err)
		}

		for _, content := range contents.Results {

			fmt.Println(content.Content.ID, content.Content.Title)
			/*
				fmt.Println(content.Content.ID)
				fmt.Println(content.Content.Title)
				fmt.Println(content.Content.Type)
				fmt.Println(content.LastModified)
				fmt.Println(content.FriendlyLastModified)
			*/
		}

		if contents.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(contents.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		start, err := strconv.Atoi(values["start"][0])
		if err != nil {
			log.Fatal(err)
		}

		options.Cursor = values["cursor"][0]
		options.Start = start
	}

}
```

## Search Users

Searches for users using user-specific queries from the [Confluence Query Language (CQL)](https://developer.atlassian.com/cloud/confluence/advanced-searching-using-cql/).

Note that some user fields may be set to null depending on the user's privacy settings. These are: email, profilePicture, displayName, and timeZone.

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-search#api-wiki-rest-api-search-user-get" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	ctx := context.Background()
	cql := "type = user"


	users, response, err := instance.Search.Users(ctx, cql, 0, 50, nil)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, user := range users.Results {
		fmt.Println(user.User)
	}
}
```

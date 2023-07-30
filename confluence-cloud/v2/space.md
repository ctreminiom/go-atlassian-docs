---
cover: ../../.gitbook/assets/bb7f24ad-a935-415f-a11f-c1b2ffec7cb8-1560x760.jpeg
coverY: 0
---

# 🪟 Space

The Confluence Cloud V2 API space endpoints are a set of new features and functionalities that have been introduced in the Confluence Cloud platform to provide users with enhanced control and management of their Confluence spaces. These endpoints allow users to perform various space-related operations programmatically, such as creating new spaces, updating space properties, retrieving space information, and managing space permissions.

## Get spaces

`GET /wiki/api/v2/spaces`

Bulk returns all spaces. The results will be sorted by id ascending. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
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

	options := &models.GetSpacesOptionSchemeV2{
		IDs:               nil,
		Keys:              nil,
		Type:              "",
		Status:            "",
		Labels:            nil,
		Sort:              "",
		DescriptionFormat: "",
		SerializeIDs:      false,
	}

	var cursor string
	for {

		spaces, response, err := instance.Space.Bulk(context.Background(), options, cursor, 20)
		if err != nil {
			log.Fatal(err)
		}

		for _, space := range spaces.Results {
			fmt.Println(space)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if spaces.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(spaces.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```
{% endcode %}

## Get space by id

`GET /wiki/api/v2/spaces/{id}`

Get returns a specific space.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"os"
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

	space, response, err := instance.Space.Get(context.Background(), 196613, "plain")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}
	fmt.Println(space)
}
```
{% endcode %}

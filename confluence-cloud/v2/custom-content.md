---
cover: >-
  ../../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
coverY: 0
---

# 🗃 Custom Content

## Get custom content by type

Returns all custom content for a given type.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	options := &models.CustomContentOptionsScheme{
		IDs:        nil,
		SpaceIDs:   nil,
		Sort:       "",
		BodyFormat: "",
	}

	customContents, response, err := instance.CustomContent.Gets(context.Background(), "pages", options, "", 10)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	for _, customContent := range customContents.Results {
		fmt.Println(customContent.ID)
		fmt.Println(customContent.CustomContentID)
		fmt.Println(customContent.Status)
		fmt.Println(customContent.Title)
		fmt.Println(customContent.ID)
	}
}
```
{% endcode %}

## Create custom content

`POST /wiki/api/v2/custom-content`

Creates a new custom content in the given space, page, blogpost or other custom content.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.CustomContentPayloadScheme{
		Type:            "",
		Status:          "",
		SpaceID:         "",
		PageID:          "",
		BlogPostID:      "",
		CustomContentID: "",
		Title:           "",
		Body: &models.BodyTypeScheme{
			Representation: "",
			Value:          "",
		},
	}

	customContent, response, err := instance.CustomContent.Create(context.Background(), payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Println(customContent.ID)
	fmt.Println(customContent.CustomContentID)
	fmt.Println(customContent.Status)
	fmt.Println(customContent.Title)
	fmt.Println(customContent.ID)
}
```
{% endcode %}

## Get custom content by ID

`GET /wiki/api/v2/custom-content/{id}`

Returns a specific piece of custom content.

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

	customContent, response, err := instance.CustomContent.Get(context.Background(), 10001, "", 0)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Println(customContent.ID)
	fmt.Println(customContent.CustomContentID)
	fmt.Println(customContent.Status)
	fmt.Println(customContent.Title)
	fmt.Println(customContent.ID)
}
```
{% endcode %}

## Update custom content

`PUT /wiki/api/v2/custom-content/{id}`

Update a custom content by id.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.CustomContentPayloadScheme{
		ID:              "",
		Type:            "",
		Status:          "",
		SpaceID:         "",
		PageID:          "",
		BlogPostID:      "",
		CustomContentID: "",
		Title:           "",
		Body: &models.BodyTypeScheme{
			Representation: "",
			Value:          "",
		},
		Version: &models.CustomContentPayloadVersionScheme{
			Number:  0,
			Message: "",
		},
	}

	customContent, response, err := instance.CustomContent.Update(context.Background(), 10001, payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Println(customContent.ID)
	fmt.Println(customContent.CustomContentID)
	fmt.Println(customContent.Status)
	fmt.Println(customContent.Title)
	fmt.Println(customContent.ID)
}
```
{% endcode %}

## Delete custom content

`DELETE /wiki/api/v2/custom-content/{id}`

Delete a custom content by id.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	response, err := instance.CustomContent.Delete(context.Background(), 10001)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}
}
```
{% endcode %}

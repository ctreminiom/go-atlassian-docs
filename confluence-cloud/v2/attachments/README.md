---
cover: ../../../.gitbook/assets/growthgauntletcoverillo.jpg
coverY: 0
---

# 🧺 Attachments

## Get attachment by id

`GET /wiki/api/v2/attachments/{id}`

Get returns a specific attachment

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

	attachment, response, err := instance.Attachment.Get(context.Background(), "att219152391", 0, false)
	if err != nil {
		if response != nil {
			log.Println(response.Endpoint)
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Print(attachment)
}
```
{% endcode %}

## Get attachments by type

`GET /wiki/api/v2/{custom-content-labels-pages-blogposts}/{id}/attachments`

Gets returns the attachments of specific entity type.

* You can extract the attachments for blog-posts,custom-contents, labels and pages
* Depending on the entity type, the endpoint will change based on the entity type.
* Valid entityType values: <mark style="color:purple;">**blogposts**</mark>, <mark style="color:orange;">**custom-content**</mark>, <mark style="color:green;">**labels**</mark>, <mark style="color:red;">**pages**</mark>
* The number of results is limited by the limit parameter and additional results (if available) will be available through the next URL present in the Link response header.

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

	options := &models.AttachmentParamsScheme{
		Sort:         "",
		MediaType:    "",
		FileName:     "",
		SerializeIDs: false,
	}

	attachments, response, err := instance.Attachment.Gets(context.Background(), 65798145, "pages", options, "", 50)
	if err != nil {
		if response != nil {
			log.Println(response.Endpoint)
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	for _, attachment := range attachments.Results {
		fmt.Println(attachment.ID, attachment.Title)
	}
}
```
{% endcode %}

## Delete attachment

`DELETE /wiki/api/v2/attachments/{id}`

Delete an attachment by id.

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

	response, err := instance.Attachment.Delete(context.Background(), "att219152391")
	if err != nil {
		if response != nil {
			log.Println(response.Endpoint)
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}
}
```
{% endcode %}

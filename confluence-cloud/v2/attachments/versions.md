---
cover: ../../../.gitbook/assets/leadership-principles_1120x545@2x-1560x760.png
coverY: 0
---

# 💻 Versions

## Get attachment versions

`GET /wiki/api/v2/attachments/{id}/versions`

Returns the versions of specific attachment.

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

	versions, response, err := instance.Attachment.Version.Gets(context.Background(), "att225509380", "", "", 20)
	if err != nil {
		if response != nil {
			log.Println(response.Endpoint)
			log.Println(response.Code)
			log.Println(response.Bytes.String())
			log.Println(response.Header)
		}

		log.Fatal(err)
	}

	for _, version := range versions.Results {
		fmt.Println(version.AuthorID)
		fmt.Println(version.Number)
		fmt.Println(version.CreatedAt)
		fmt.Println(version.Number)
	}
}
```
{% endcode %}

## Get attachment version

`GET /wiki/api/v2/attachments/{attachment-id}/versions/{version-number}`

Retrieves version details for the specified attachment and version number.

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

	version, response, err := instance.Attachment.Version.Get(context.Background(), "att225509380", 1)
	if err != nil {
		if response != nil {
			log.Println(response.Endpoint)
			log.Println(response.Code)
			log.Println(response.Bytes.String())
			log.Println(response.Header)
		}

		log.Fatal(err)
	}

	fmt.Println(version.CreatedAt)
	fmt.Println(version.AuthorID)
	fmt.Println(version.MinorEdit)
	fmt.Println(version.NextVersion)
	fmt.Println(version.Collaborators)
	fmt.Println(version.ContentTypeModified)
}
```
{% endcode %}

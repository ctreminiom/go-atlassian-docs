# 🔃 Versions

## Get content versions

Returns the versions for a piece of content in descending order.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		ctx       = context.Background()
		contentID = "80412692"
		expand    = []string{"collaborators", "content"}
		start     = 0
		limit     = 50
	)

	versions, response, err := instance.Content.Version.Gets(ctx, contentID, expand, start, limit)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, version := range versions.Results {
		fmt.Println(version.Number, version.Content.Title, version.By.Email)
	}
}
```

## Restore content version

Restores a historical version to be the latest version. That is, a new version is created with the content of the historical version.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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
	contentID := "80412692"
	expand := []string{"collaborators", "content"}

	payload := &models.ContentRestorePayloadScheme{
		OperationKey: "restore",
		Params: &models.ContentRestoreParamsPayloadScheme{
			VersionNumber: 1,
			Message:       "message sample :)",
			RestoreTitle:  true,
		},
	}

	version, response, err := instance.Content.Version.Restore(ctx, contentID, payload, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(version.Content.Title)
	fmt.Println(version.Content.ID)
	fmt.Println(version.By.Email)
	fmt.Println(version.By.Type)
	fmt.Println(version.By.AccountID)
	fmt.Println(version.Collaborators.UserKeys)
}
```

## Get content version

Returns a version for a piece of content.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		ctx       = context.Background()
		contentID = "80412692"
		versionID = 1
		expand    = []string{"collaborators", "content"}
	)

	version, response, err := instance.Content.Version.Get(ctx, contentID, versionID, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(version.Content.Title)
	fmt.Println(version.Content.ID)
	fmt.Println(version.By.Email)
	fmt.Println(version.By.Type)
	fmt.Println(version.By.AccountID)
	fmt.Println(version.Collaborators.UserKeys)
}
```

## Delete content version

Delete a historical version. This does not delete the changes made to the content in that version, rather the changes for the deleted version are rolled up into the next version. Note, you cannot delete the current version.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		ctx           = context.Background()
		contentID     = "80412692"
		versionNumber = 2
	)

	response, err := instance.Content.Version.Delete(ctx, contentID, versionNumber)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}
}
```

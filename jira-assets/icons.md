---
cover: ../.gitbook/assets/conways-law_1120x545@2x-1560x760.jpg
coverY: 0
---

# 👾 Icons

## Get icon

`GET /jsm/assets/workspace/{workspaceId}/v1/icon/{id}`

The `Get` method loads a single asset icon by id.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/assets"
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

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId

	// Instance the Assets Cloud client
	asset, err := assets.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	asset.Auth.SetBasicAuth(mail, token)

	icon, response, err := asset.Icon.Get(context.Background(), workSpaceID, "1")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(icon.ID, icon.Name)
}
```
{% endcode %}

## Get global icons

`GET /jsm/assets/workspace/{workspaceId}/v1/icon/global`

The `Gets`method returns all global icons i.e. icons not associated with a particular object schema.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/assets"
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

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId

	// Instance the Assets Cloud client
	asset, err := assets.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	asset.Auth.SetBasicAuth(mail, token)

	icons, response, err := asset.Icon.Global(context.Background(), workSpaceID)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, icon := range icons {
		fmt.Println(icon.ID, icon.Name)
	}
}
```
{% endcode %}

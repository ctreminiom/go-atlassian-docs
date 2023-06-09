# 🔎 Aql

## Overview

Assets Query Language (AQL) is a language format used in Assets in Jira Service Management to create search queries for one or more objects. Using AQL, you can return any object or group of objects in Assets in a search, filter objects, modify objects, create custom fields, automation, and more.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>AQL Sample</p></figcaption></figure>

{% embed url="https://support.atlassian.com/jira-service-management-cloud/docs/use-assets-query-language-aql/" %}

### Filter objects

The _**Filter**_ method retrieves a list of objects based on an AQL. Note that the preferred endpoint is /aql

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
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

	payload := &models.AQLSearchParamsScheme{
		Query:          "objectType = Office AND Name LIKE SYD",
		ObjectSchemaID: "2",
		ObjectTypeID:   "2",
		ResultsPerPage: 50,
	}

	page, response, err := asset.AQL.Filter(context.Background(), workSpaceID, payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(page)
	for _, object := range page.ObjectEntries {
		fmt.Println(object)
	}
}
```

\

# 🦞 Object

## Overview

Creating a custom Assets objects field allows your team to access Assets objects directly from the issue view. This is a powerful feature of Assets in Jira Service Management that can help your agents get the context they need to resolve an issue or request quickly and effectively.

Adding an object (i.e. as a value) to the Assets objects custom field creates a link between an issue and an object. This then allows you to see all of the connected issues from the object view.

This is useful for incident management because you can use the graph to traverse through dependencies and understand where things have gone wrong.

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

It’s also useful for change management because it allows you to see the bigger picture and evaluate risk - easier to do when you can see what depends on the item you’re making changes to.

### Get object by ID

The Get method load one object

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/davecgh/go-spew/spew"
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

	object, response, err := asset.Object.Get(context.Background(), workSpaceID, "AS-3")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println("Object ID:", object.ID)
	fmt.Println("Object Workspace ID:", object.WorkspaceId)
	fmt.Println("Object Global ID:", object.GlobalId)
	fmt.Println("Object Label:", object.Label)
	fmt.Println("Object ObjectKey:", object.ObjectKey)
	fmt.Println("Object ObjectType ID:", object.ObjectType.Id)
	fmt.Println("Object ObjectType Name:", object.ObjectType.Name)

	for index, attribute := range object.Attributes {

		fmt.Println("--------------")
		fmt.Println(index, "Attribute Global ID:", attribute.GlobalId)
		fmt.Println(index, "Attribute Reference:", attribute.ID)
		fmt.Println(index, "Attribute Name:", attribute.ObjectTypeAttribute.Name)
		fmt.Println(index, "Attribute Type:", attribute.ObjectTypeAttribute.DefaultType.Name)
		fmt.Println(index, "Attribute Options:", attribute.ObjectTypeAttribute.Options)
		fmt.Println(index, "Attribute TypeAttributeId:", attribute.ObjectTypeAttributeId)

		fmt.Println("--------------")
	}

	log.Println("Endpoint:", response.Endpoint)

	spew.Dump(object)

}
```

### Update object by ID

The _**Update**_ method updates an asset object

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

	payload := &models.ObjectPayloadScheme{
		ObjectTypeID: "1",
		AvatarUUID:   "",
		HasAvatar:    false,
		Attributes: []*models.ObjectPayloadAttributeScheme{
			{
				ObjectTypeAttributeID: "16",
				ObjectAttributeValues: []*models.ObjectPayloadAttributeValueScheme{
					{
						Value: "A placeholder value",
					},
				},
			},
		},
	}

	object, response, err := asset.Object.Update(context.Background(), workSpaceID, "AS-2", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println(object.ID)
	fmt.Println(object.WorkspaceId)
	fmt.Println(object.GlobalId)
	fmt.Println(object.Label)
	fmt.Println(object.ObjectKey)
	fmt.Println(object.Avatar.ObjectId)
	fmt.Println(object.ObjectType.Name)
	fmt.Println(len(object.Attributes))
	fmt.Println("--------")
}
```

### Delete object by ID

The _**Delete**_ method deletes an asset object

```go
package main

import (
	"context"
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

	response, err = asset.Object.Delete(context.Background(), workSpaceID, "AS-2")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)

}
```

### Get object attributes

The _**Attributes**_ method returns the asset object attributes

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

	attributes, response, err := asset.Object.Attributes(context.Background(), workSpaceID, "AS-3")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for index, attribute := range attributes {

		fmt.Println("--------------")
		fmt.Println(index, "Attribute Global ID:", attribute.GlobalId)
		fmt.Println(index, "Attribute Reference:", attribute.ID)
		fmt.Println(index, "Attribute Name:", attribute.ObjectTypeAttribute.Name)
		fmt.Println(index, "Attribute Type:", attribute.ObjectTypeAttribute.DefaultType.Name)
		fmt.Println(index, "Attribute Options:", attribute.ObjectTypeAttribute.Options)
		fmt.Println(index, "Attribute TypeAttributeId:", attribute.ObjectTypeAttributeId)

		fmt.Println("--------------")
	}

	log.Println("Endpoint:", response.Endpoint)
}
```

### Get object changelogs

The _**History**_ method returns the asset object changelogs

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/davecgh/go-spew/spew"
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

	history, response, err := asset.Object.History(context.Background(), workSpaceID, "AS-3", true)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, record := range history {
		spew.Dump(record)
	}

	log.Println("Endpoint:", response.Endpoint)

}
```

### Get object references

The _**References**_ method returns the asset object references links

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/davecgh/go-spew/spew"
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

	references, response, err := asset.Object.References(context.Background(), workSpaceID, "AS-3")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, record := range references {
		spew.Dump(record)
	}

	log.Println("Endpoint:", response.Endpoint)
}
```

### Create object

The _**Create**_ method creates an asset object

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"github.com/davecgh/go-spew/spew"
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

	payload := &models.ObjectPayloadScheme{
		ObjectTypeID: "3",
		AvatarUUID:   "",
		HasAvatar:    false,
		Attributes: []*models.ObjectPayloadAttributeScheme{
			{
				ObjectTypeAttributeID: "13",
				ObjectAttributeValues: []*models.ObjectPayloadAttributeValueScheme{
					{
						Value: "SmartPhone Mocked",
					},
				},
			},
		},
	}

	object, response, err := asset.Object.Create(context.Background(), workSpaceID, payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
}
```

### Get object tickets

The _**Relation**_ method returns the relation between Jira issues and Assets objects

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

	tickets, response, err := asset.Object.Relation(context.Background(), workSpaceID, "SVC-1")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, ticket := range tickets.Tickets {
		fmt.Println(ticket.Id, ticket.Key)
	}

	log.Println("Endpoint:", response.Endpoint)
}
```

### Filter objects <a href="#filter-objects" id="filter-objects"></a>

The _**Filter**_ method returns Objects using an AQL query

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

	objects, response, err := asset.Object.Filter(context.Background(), workSpaceID, "Name LIKE Test", true, 0, 50)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}

		log.Fatal(err)
	}

	log.Println(objects.Total)
	log.Println(objects.IsLast)
	log.Println(objects.MaxResults)
	log.Println(objects.StartAt)
	log.Println(len(objects.Values))

	for _, object := range objects.Values {
		fmt.Println(object.ID)
	}

}
```

---
cover: ../../../.gitbook/assets/homegrown-innovation_1120x545@2x-1560x760.png
coverY: 0
---

# 🧰 Type

An object type represents a group of similar objects in an _object schema_. These objects share _attributes_, and other characteristics that make them useful to hold information about a certain type of asset or service.

Each object type can have a single parent object type and multiple child object types. You can configure inheritance so that child object types automatically receive the attributes of their parents and pass their own attributes to their children.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Similarly you can set some object types as abstract, meaning that they can’t contain any objects of their own but can pass their attributes to their children, who can themselves contain objects. Inheritance and abstract object types can be used to create object schemas that are both simple and powerful.

### Get object type

`GET /jsm/assets/workspace/{workspaceId}/v1/objecttype/{id}`

The _**Get**_ method finds an object type by id

{% code fullWidth="true" %}
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

	payload := &models.ObjectTypePayloadScheme{
		Name:               "Iphone 13",
		Description:        "description sample",
		IconId:             "143",
		ObjectSchemaId:     "2",
		ParentObjectTypeId: "1",
		Inherited:          false,
		AbstractObjectType: false,
	}

	object, response, err := asset.ObjectType.Update(context.Background(), workSpaceID, "2", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

### Update object type

`PUT /jsm/assets/workspace/{workspaceId}/v1/objecttype/{id}`

The _**Update**_ method updates an existing object type

{% code fullWidth="true" %}
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

	object, response, err := asset.ObjectType.Get(context.Background(), workSpaceID, "2")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

### Delete object type

`DELETE /jsm/assets/workspace/{workspaceId}/v1/objecttype/{id}`

The _**Delete**_ method deletes an object type

{% code fullWidth="true" %}
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

	object, response, err := asset.ObjectType.Delete(context.Background(), workSpaceID, "4")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

### Get object type attributes

`GET /jsm/assets/workspace/{workspaceId}/v1/objecttype/{id}/attributes`

The _**Attributes**_ method finds all attributes for this object type

{% code fullWidth="true" %}
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

	options := &models.ObjectTypeAttributesParamsScheme{
		OnlyValueEditable:       true,
		OrderByName:             false,
		Query:                   "",
		IncludeValuesExist:      false,
		ExcludeParentAttributes: false,
		IncludeChildren:         true,
		OrderByRequired:         false,
	}

	attributes, response, err := asset.ObjectType.Attributes(context.Background(), workSpaceID, "2", options)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, attribute := range attributes {

		spew.Dump(attribute)
	}

	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

### Update object type position

`POST /jsm/assets/workspace/{workspaceId}/v1/objecttype/{id}/position`

The _**Position**_ method changes position of this object type.

{% code fullWidth="true" %}
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

	payload := &models.ObjectTypePositionPayloadScheme{
		ToObjectTypeId: "2",
		Position:       0,
	}

	object, response, err := asset.ObjectType.Position(context.Background(), workSpaceID, "2", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

### Create object type

`POST /jsm/assets/workspace/{workspaceId}/v1/objecttype/create`

The _**Create**_ method creates a new object type

{% code fullWidth="true" %}
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

	payload := &models.ObjectTypePayloadScheme{
		Name:               "Office",
		Description:        "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin nec ex.",
		IconId:             "143",
		ObjectSchemaId:     "2",
		Inherited:          false,
		AbstractObjectType: false,
	}

	object, response, err := asset.ObjectType.Create(context.Background(), workSpaceID, payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(object)
	log.Println("Endpoint:", response.Endpoint)
}
```
{% endcode %}

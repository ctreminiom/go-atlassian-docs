---
cover: ../../.gitbook/assets/artboard-2@3x-1560x760.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 📠 Schema

## Get object schema list

`GET /jsm/assets/workspace/{workspaceId}/v1/objectschema/list`

The `List` method return all the object schemas available on Assets.

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

	schemas, response, err := asset.ObjectSchema.List(context.Background(), workSpaceID)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, schema := range schemas.Values {

		fmt.Println("--------")
		fmt.Println(schema.Name)
		fmt.Println(schema.GlobalId)
		fmt.Println(schema.Id)
		fmt.Println(schema.Name)
		fmt.Println(schema.ObjectSchemaKey)
		fmt.Println(schema.Status)
		fmt.Println(schema.Description)
		fmt.Println(schema.Created)
		fmt.Println(schema.Updated)
		fmt.Println(schema.ObjectCount)
		fmt.Println(schema.ObjectTypeCount)
		fmt.Println(schema.CanManage)
		fmt.Println("--------")
	}
}
```
{% endcode %}

## Create object schema

`POST /jsm/assets/workspace/{workspaceId}/v1/objectschema/create`

The `Create` method creates a new object schema

{% code fullWidth="true" %}
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

	payload := &models.ObjectSchemaPayloadScheme{
		Name:            "Computers",
		ObjectSchemaKey: "COMP",
		Description:     "The IT department schema",
	}

	schema, response, err := asset.ObjectSchema.Create(context.Background(), workSpaceID, payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println(schema.Name)
	fmt.Println(schema.GlobalId)
	fmt.Println(schema.Id)
	fmt.Println(schema.Name)
	fmt.Println(schema.ObjectSchemaKey)
	fmt.Println(schema.Status)
	fmt.Println(schema.Description)
	fmt.Println(schema.Created)
	fmt.Println(schema.Updated)
	fmt.Println(schema.ObjectCount)
	fmt.Println(schema.ObjectTypeCount)
	fmt.Println(schema.CanManage)
	fmt.Println("--------")
}
```
{% endcode %}

## Get object schema

`GET /jsm/assets/workspace/{workspaceId}/v1/objectschema/{id}`

The `Get` method returns an object schema by ID

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

	schema, response, err := asset.ObjectSchema.Get(context.Background(), workSpaceID, "3")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println(schema.Name)
	fmt.Println(schema.GlobalId)
	fmt.Println(schema.Id)
	fmt.Println(schema.Name)
	fmt.Println(schema.ObjectSchemaKey)
	fmt.Println(schema.Status)
	fmt.Println(schema.Description)
	fmt.Println(schema.Created)
	fmt.Println(schema.Updated)
	fmt.Println(schema.ObjectCount)
	fmt.Println(schema.ObjectTypeCount)
	fmt.Println(schema.CanManage)
	fmt.Println("--------")
}
```
{% endcode %}

## Update object schema

`PUT /jsm/assets/workspace/{workspaceId}/v1/objectschema/{id}`

The `Update` method updates an object schema

{% code fullWidth="true" %}
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

	payload := &models.ObjectSchemaPayloadScheme{
		Name:            "Computers - UPDATED",
		ObjectSchemaKey: "COMPDE",
		Description:     "The IT department schema - UPDATED",
	}

	schema, response, err := asset.ObjectSchema.Update(context.Background(), workSpaceID, "3", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println(schema.Name)
	fmt.Println(schema.GlobalId)
	fmt.Println(schema.Id)
	fmt.Println(schema.Name)
	fmt.Println(schema.ObjectSchemaKey)
	fmt.Println(schema.Status)
	fmt.Println(schema.Description)
	fmt.Println(schema.Created)
	fmt.Println(schema.Updated)
	fmt.Println(schema.ObjectCount)
	fmt.Println(schema.ObjectTypeCount)
	fmt.Println(schema.CanManage)
	fmt.Println("--------")
}
```
{% endcode %}

## Delete object schema

`DELETE /jsm/assets/workspace/{workspaceId}/v1/objectschema/{id}`

The `Delete` method deletes a schema

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

	schema, response, err := asset.ObjectSchema.Delete(context.Background(), workSpaceID, "3")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println("--------")
	fmt.Println(schema.Name)
	fmt.Println(schema.GlobalId)
	fmt.Println(schema.Id)
	fmt.Println(schema.Name)
	fmt.Println(schema.ObjectSchemaKey)
	fmt.Println(schema.Status)
	fmt.Println(schema.Description)
	fmt.Println(schema.Created)
	fmt.Println(schema.Updated)
	fmt.Println(schema.ObjectCount)
	fmt.Println(schema.ObjectTypeCount)
	fmt.Println(schema.CanManage)
	fmt.Println("--------")
}
```
{% endcode %}

## Get object schema attributes

`GET /jsm/assets/workspace/{workspaceId}/v1/objectschema/{id}/attributes`

The `Attributes` method finds all the object type attributes for this object schema

{% code fullWidth="true" %}
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

	options := &models.ObjectSchemaAttributesParamsScheme{
		OnlyValueEditable: true,
		Extended:          true,
		Query:             "",
	}

	attributes, response, err := asset.ObjectSchema.Attributes(context.Background(), workSpaceID, "1", options)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, attribute := range attributes {

		fmt.Println("-----")
		fmt.Println(attribute.Id)
		fmt.Println(attribute.GlobalId)
		fmt.Println(attribute.Name)
		fmt.Println(attribute.DefaultType)
		fmt.Println(attribute.Options)
		fmt.Println(attribute.Position)
		fmt.Println("-----")

	}
}	
```
{% endcode %}

## Get object schema types

`GET /jsm/assets/workspace/{workspaceId}/v1/objectschema/{id}/objecttypes`

The `ObjectTypes` method returns all the object types for this object schema

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

	types, response, err := asset.ObjectSchema.ObjectTypes(context.Background(), workSpaceID, "1", false)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	for _, objectType := range types {
		fmt.Println(objectType)
	}
}
```
{% endcode %}

# 🔩 Schemes

## Get all schemas

Get all SCIM features metadata. Filtering, pagination, and sorting are not supported.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	schemas, response, err := cloudAdmin.SCIM.Scheme.Gets(context.Background(), directoryID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(schemas)

}

```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMSchemasScheme struct {
	TotalResults int               `json:"totalResults,omitempty"`
	ItemsPerPage int               `json:"itemsPerPage,omitempty"`
	StartIndex   int               `json:"startIndex,omitempty"`
	Schemas      []string          `json:"schemas,omitempty"`
	Resources    []*ResourceScheme `json:"Resources,omitempty"`
}

type ResourceScheme struct {
	ID          string              `json:"id,omitempty"`
	Name        string              `json:"name,omitempty"`
	Description string              `json:"description,omitempty"`
	Attributes  []*AttributeScheme  `json:"attributes,omitempty"`
	Meta        *ResourceMetaScheme `json:"meta,omitempty"`
}

type ResourceMetaScheme struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
}

type AttributeScheme struct {
	Name          string                `json:"name,omitempty"`
	Type          string                `json:"type,omitempty"`
	MultiValued   bool                  `json:"multiValued,omitempty"`
	Description   string                `json:"description,omitempty"`
	Required      bool                  `json:"required,omitempty"`
	CaseExact     bool                  `json:"caseExact,omitempty"`
	Mutability    string                `json:"mutability,omitempty"`
	Returned      string                `json:"returned,omitempty"`
	Uniqueness    string                `json:"uniqueness,omitempty"`
	SubAttributes []*SubAttributeScheme `json:"subAttributes,omitempty"`
}

type SubAttributeScheme struct {
	Name        string `json:"name,omitempty"`
	Type        string `json:"type,omitempty"`
	MultiValued bool   `json:"multiValued,omitempty"`
	Description string `json:"description,omitempty"`
	Required    bool   `json:"required,omitempty"`
	CaseExact   bool   `json:"caseExact,omitempty"`
	Mutability  string `json:"mutability,omitempty"`
	Returned    string `json:"returned,omitempty"`
	Uniqueness  string `json:"uniqueness,omitempty"`
}
```

## Get user schemas

Get the user schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	schemas, response, err := cloudAdmin.SCIM.Scheme.User(context.Background(), directoryID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(schemas)
}

```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMSchemaScheme struct {
	ID          string              `json:"id,omitempty"`
	Name        string              `json:"name,omitempty"`
	Description string              `json:"description,omitempty"`
	Attributes  []*AttributeScheme  `json:"attributes,omitempty"`
	Meta        *ResourceMetaScheme `json:"meta,omitempty"`
}
```

## Get group schemas

Get the group schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	schemas, response, err := cloudAdmin.SCIM.Scheme.Group(context.Background(), directoryID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, attribute := range schemas.Attributes {
		log.Println("----------------------")
		log.Println("Type", attribute.Type)
		log.Println("Description", attribute.Description)
		log.Println("Name", attribute.Name)
		log.Println("Required", attribute.Required)
		log.Println("Returned", attribute.Returned)
		log.Println("Mutability", attribute.Mutability)
		log.Println("SubAttributes", len(attribute.SubAttributes))

		for _, subAttribute := range attribute.SubAttributes {
			log.Println("==============================")
			log.Println("==", subAttribute.Uniqueness)
			log.Println("==", subAttribute.Mutability)
			log.Println("==", subAttribute.Returned)
			log.Println("==", subAttribute.Required)
			log.Println("==", subAttribute.Name)
			log.Println("==", subAttribute.Description)
			log.Println("==============================")

		}

		log.Println("----------------------")

	}
}

```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type SCIMSchemaScheme struct {
	ID          string              `json:"id,omitempty"`
	Name        string              `json:"name,omitempty"`
	Description string              `json:"description,omitempty"`
	Attributes  []*AttributeScheme  `json:"attributes,omitempty"`
	Meta        *ResourceMetaScheme `json:"meta,omitempty"`
}
```

## Get user enterprise extension schemas

Get the user enterprise extension schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	enterpriseSchemas, response, err := cloudAdmin.SCIM.Scheme.Enterprise(context.Background(), directoryID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, schema := range enterpriseSchemas.Attributes {
		log.Println(schema)
	}

}
```

## Get feature metadata

Get metadata about the supported SCIM features. This is a service provider configuration endpoint providing supported SCIM features. Filtering, pagination, and sorting are not supported.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

	serviceProvider, response, err := cloudAdmin.SCIM.Scheme.Feature(context.Background(), directoryID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(serviceProvider)

}
```

---
cover: >-
  ../../.gitbook/assets/vanessa_lovegrove_cognitive_overload_1120x545@2x-1560x760.jpeg
coverY: 186
---

# 🔩 Schemas

## Get all schemas

`GET /scim/directory/{directoryId}/Schemas`

Get all SCIM features metadata. Filtering, pagination, and sorting are not supported.

{% code fullWidth="true" %}
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
{% endcode %}

## Get user schemas

`GET /scim/directory/{directoryId}/Schemas/urn:ietf:params:scim:schemas:core:2.0:User`

Get the user schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

{% code fullWidth="true" %}
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
{% endcode %}

## Get group schemas

`GET /scim/directory/{directoryId}/Schemas/urn:ietf:params:scim:schemas:core:2.0:Group`

Get the group schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

{% code fullWidth="true" %}
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
{% endcode %}

## Get user enterprise extension schemas

`GET /scim/directory/{directoryId}/Schemas/urn:ietf:params:scim:schemas:extension:enterprise:2.0:User`

Get the user enterprise extension schemas from the SCIM provider. Filtering, pagination and sorting are not supported.

{% code fullWidth="true" %}
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
{% endcode %}

## Get feature metadata

`GET /scim/directory/{directoryId}/ServiceProviderConfig`

Get metadata about the supported SCIM features. This is a service provider configuration endpoint providing supported SCIM features. Filtering, pagination, and sorting are not supported.

{% code fullWidth="true" %}
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
{% endcode %}

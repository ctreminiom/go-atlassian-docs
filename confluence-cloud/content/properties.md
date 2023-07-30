---
cover: ../../.gitbook/assets/emailqa_1120x545@2x-1560x760.jpg
coverY: 0
---

# 🎮 Properties

## Get content properties

`GET /wiki/rest/api/content/{id}/property`

Returns the properties for a piece of content. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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
      contentID  = "80412692"
      expand     = []string{"version"}
      startAt    = 0
      maxResults = 50
   )

   properties, response, err := instance.Content.Property.Gets(context.Background(), contentID, expand, startAt, maxResults)
   if err != nil {
      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   for _, property := range properties.Results {
      log.Println(property)
   }

}
```
{% endcode %}

## Create content property

`POST /wiki/rest/api/content/{id}/property`

Creates a property for an existing piece of content. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).&#x20;

* This is the same as [Create content property for key](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-properties/) except that the key is specified in the request body instead of as a path parameter.&#x20;
* Content properties can also be added when creating a new piece of content by including them in the `metadata.properties` of the request.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/http"
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

	var payload = &models.ContentPropertyPayloadScheme{
		Key:   "key",
		Value: "value",
	}

	property, response, err := instance.Content.Property.Create(context.Background(), "80412692", payload)
	if err != nil {
		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	fmt.Println(property)
}
```
{% endcode %}

## Get content property

`GET /wiki/rest/api/content/{id}/property/{key}`

Returns a content property for a piece of content. For more information, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "fmt"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   property, response, err := instance.Content.Property.Get(context.Background(), "80412692", "editor")
   if err != nil {
      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   fmt.Println(property)
}
```
{% endcode %}

## Delete content property

`DELETE /wiki/rest/api/content/{id}/property/{key}`

Deletes a content property. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   response, err := instance.Content.Property.Delete(context.Background(), "80412692", "key")
   if err != nil {
      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
}
```
{% endcode %}

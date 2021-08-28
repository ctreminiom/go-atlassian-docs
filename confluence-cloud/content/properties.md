# 🎮 Properties

## Get content properties

Returns the properties for a piece of content. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

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
            log.Println(response.API)
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

## Create content property

Creates a property for an existing piece of content. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/). This is the same as [Create content property for key](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-properties/) except that the key is specified in the request body instead of as a path parameter. Content properties can also be added when creating a new piece of content by including them in the `metadata.properties` of the request.

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

   var payload = &confluence.ContentPropertyPayloadScheme{
      Key:   "key",
      Value: "value",
   }

   property, response, err := instance.Content.Property.Create(context.Background(), "80412692", payload)
   if err != nil {
      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   fmt.Println(property)

}
```

## Get content property

Returns a content property for a piece of content. For more information, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

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
            log.Println(response.API)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   fmt.Println(property)
}
```

## Delete content property

Deletes a content property. For more information about content properties, see [Confluence entity properties](https://developer.atlassian.com/cloud/confluence/confluence-entity-properties/).

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
            log.Println(response.API)
         }
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
}
```


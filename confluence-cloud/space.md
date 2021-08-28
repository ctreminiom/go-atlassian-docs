# 💾 Space

## Get content for space

Returns all content in a space. The returned content is grouped by type \(pages then blogposts\), then ordered by content ID in ascending order.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   var (
      spaceKey = "DUMMY"
      depth = "all"
      expand = []string{"operations"}
      startAt = 0
      maxResults = 50
   )

   contents, response, err := instance.Space.Content(context.Background(), spaceKey, depth, expand, startAt, maxResults)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)


   log.Println(contents.Links)
   log.Println(contents.Page)
}
```

## Get content by type for space

Returns all content of a given type, in a space. The returned content is ordered by content ID in ascending order.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   var (
      spaceKey = "DUMMY"
      contentType = "page"
      depth = "all"
      expand = []string{"operations"}
      startAt = 0
      maxResults = 50
   )

   contents, response, err := instance.Space.ContentByType(context.Background(), spaceKey, contentType, depth, expand, startAt, maxResults)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)


   for _, content := range contents.Results {
      log.Println(content.ID, content.Title)
   }

}
```

## Create space

Creates a new space. Note, currently you cannot set space labels when creating a space.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   var payload = &confluence.CreateSpaceScheme{
      Key:              "DUM",
      Name:             "Dum Confluence Space",
      Description:      &confluence.CreateSpaceDescriptionScheme{
         Plain: &confluence.CreateSpaceDescriptionPlainScheme{
            Value:          "Confluence Space Description Sample",
            Representation: "plain",
         },
      },
      AnonymousAccess:  true,
      UnlicensedAccess: false,
   }

   space, response, err := instance.Space.Create(context.Background(), payload, false)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(space)
}
```

## Delete space

Deletes a space. Note, the space will be deleted in a long running task. Therefore, the space may not be deleted yet when this method has returned. Clients should poll the status link that is returned in the response until the task completes.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   task, response, err := instance.Space.Delete(context.Background(), "DS")
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   log.Println(task.ID)
   log.Println(task.Links.Status)
}
```

## Get space

Returns a space. This includes information like the name, description, and permissions, but not the content in the space.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   space, response, err := instance.Space.Get(context.Background(), "DUMMY", []string{"operations"})
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(space)
}
```

## Update space

Updates the name, description, or homepage of a space.

* For security reasons, permissions cannot be updated via the API and must be changed via the user interface instead.
* Currently, you cannot set space labels when updating a space.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
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

   var (
      spaceKey = "DUMMY"
      payload = &confluence.UpdateSpaceScheme{
         Name:        "DUMMY Space - Updated",
         Description: &confluence.CreateSpaceDescriptionScheme{
            Plain: &confluence.CreateSpaceDescriptionPlainScheme{
               Value:          "Dummy Space - Description - Updated",
               Representation: "plain",
            },
         },
         Homepage:    &confluence.UpdateSpaceHomepageScheme{ID: "65798145"},
      }
   )

   spaceUpdated, response, err := instance.Space.Update(context.Background(), spaceKey, payload)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.API)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(spaceUpdated)
}
```


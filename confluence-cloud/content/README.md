# 📃 Content

## Get content

Returns all content in a Confluence instance. By default, the following objects are expanded: `space`, `history`, `version`.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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
      contentID = "64290828"
      expand = []string{"any"}
      version = 1
   )

   content, response, err := instance.Content.Get(context.Background(), contentID, expand, version)
   if err != nil {

      if response != nil {
         log.Println(response.API)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:",    response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(content)
}
```

## Create content

Creates a new piece of content or publishes an existing draft. To publish a draft, add the `id` and `status` properties to the body of the request. Set the `id` to the ID of the draft and set the `status` to 'current'. When the request is sent, a new piece of content will be created and the metadata from the draft will be transferred into it. By default, the following objects are expanded: `space`, `history`, `version`.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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

   var payload = &confluence.ContentScheme{
      Type:  "page", // Valid values: page, blogpost, comment
      Title: "Confluence Page Title",
      Space: &confluence.SpaceScheme{Key: "DUMMY"},
      Body: &confluence.BodyScheme{
         Storage: &confluence.BodyNodeScheme{
            Value:          "<p>This is <br/> a new page</p>",
            Representation: "storage",
         },
      },
   }

   newConfluence, response, err := instance.Content.Create(context.Background(), payload)
   if err != nil {

      if response != nil {
         log.Fatal(response.API)
      }
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   log.Println("The new content has been created")
   log.Println(newConfluence.ID)
   log.Println(newConfluence.Links.Self)
   log.Println(newConfluence.Title)
   log.Println(newConfluence.Space.Name)
}
```

## Get content by ID

Returns a single piece of content, like a page or a blog post. By default, the following objects are expanded: `space`, `history`, `version`.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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
      contentID = "64290828"
      expand = []string{"any"}
      version = 1
   )

   content, response, err := instance.Content.Get(context.Background(), contentID, expand, version)
   if err != nil {

      if response != nil {
         log.Println(response.API)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:",    response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(content)
}
```

## Update content

Updates a piece of content. Use this method to update the title or body of a piece of content, change the status, change the parent page, and more.

{% hint style="warning" %}
**Note**, updating draft content is currently not supported.
{% endhint %}

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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

   var payload = &confluence.ContentScheme{
      Type:  "page", // Valid values: page, blogpost, comment
      Title: "Confluence Page Title - Updated",
      Body: &confluence.BodyScheme{
         Storage: &confluence.BodyNodeScheme{
            Value:          "<p>This is <br/> a new page - updated</p>",
            Representation: "storage",
         },
      },
      Version: &confluence.VersionScheme{Number: 2},
   }

   content, response, err := instance.Content.Update(context.Background(), "64290828", payload)
   if err != nil {

      if response != nil {
         log.Println(response.API)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:",    response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(content)
}
```

## Delete content

Moves a piece of content to the space's trash or purges it from the trash, depending on the content's type and status:

* If the content's type is `page` or `blogpost` and its status is `current`, it will be trashed.
* If the content's type is `page` or `blogpost` and its status is `trashed`, the content will be purged from the trash and deleted permanently. Note, you must also set the `status` query parameter to `trashed` in your request.
* If the content's type is `comment` or `attachment`, it will be deleted permanently without being trashed.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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

   var contentID = "74219528"

   response, err := instance.Content.Delete(context.Background(), contentID, "")
   if err != nil {

      if response != nil {
         log.Println(response.API)
         log.Println(response)
      }

      log.Fatal(err)
   }

   log.Println(response)

}
```

## Get content history

Returns the most recent update for a piece of content.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
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
      contentID = "76513281"
      expand = []string{"lastUpdated", "previousVersion"}
   )

   contentHistory, response, err := instance.Content.History(context.Background(), contentID, expand)
   if err != nil {

      if response != nil {
         log.Println(response.API)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(contentHistory)
}
```


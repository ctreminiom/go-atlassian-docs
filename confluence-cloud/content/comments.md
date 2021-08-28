# 📬 Comments

## Get content comments

Returns the comments on a piece of content.

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
      contentID = "76513281"
      expand = []string{"childTypes.all"}
      startAt = 0
      maxResults = 50
   )

   comments, response, err := instance.Content.Comment.Gets(context.Background(), contentID, expand,
      nil, startAt, maxResults)
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

   for _, comment := range comments.Results {
      log.Println(comment.Title, comment.ID, comment.Type)
   }

}
```


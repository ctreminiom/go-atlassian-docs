---
cover: >-
  ../../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
coverY: 0
---

# 📬 Comments

## Get content comments

`GET /wiki/rest/api/content/{id}/child/comment`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns the comments on a piece of content.

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
{% endcode %}

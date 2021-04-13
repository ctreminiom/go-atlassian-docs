# 📚 Knowledgebase

## Search articles

Returns articles that match the given query string across all service desks.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/jira"
   "log"
   "os"
)

func main() {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   atlassian, err := jira.New(nil, host)
   if err != nil {
      return
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   var (
      query = "login"
      start = 0
      limit = 50
   )

   articles, response, err := atlassian.ServiceManagement.Knowledgebase.Search(
      context.Background(),
      query,
      false,
      start, limit,
   )

   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, article := range articles.Values {
      log.Println(article)
   }

}
```

## Get articles

Returns articles that match the given query and belong to the knowledge base linked to the service desk.

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/jira"
   "log"
   "os"
)

func main() {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   atlassian, err := jira.New(nil, host)
   if err != nil {
      return
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   var (
      serviceDeskID = 1
      query         = "login"
      start         = 0
      limit         = 50
   )

   articles, response, err := atlassian.ServiceManagement.Knowledgebase.Gets(
      context.Background(),
      serviceDeskID,
      query,
      false,
      start, limit,
   )

   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, article := range articles.Values {
      log.Println(article)
   }

}
```

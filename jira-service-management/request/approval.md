---
description: >-
  Jira Service Management allows you to add an approval step to a status in a
  workflow, which allows you to specify if an approval is needed for issue types
  (and their associated request types)
---

# 🚫 Approval

## Get approvals

This method returns all approvals on a customer request.

```go
package main

import (
   "context"
   "encoding/json"
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

   var issueKey = "DESK-12"
   approvals, response, err := atlassian.ServiceManagement.Request.Approval.Gets(context.Background(), issueKey, 0, 50)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, customRequest := range approvals.Values {

      dataAsJson, err := json.MarshalIndent(customRequest, "", "\t")
      if err != nil {
         log.Fatal(err)
      }

      log.Println(string(dataAsJson))
   }

}
```

## Get approval by id

This method returns an approval. Use this method to determine the status of an approval and the list of approvers.

```go
package main

import (
   "context"
   "encoding/json"
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
      issueKey   = "DESK-12"
      approvalID = 2
   )

   approvalsMembers, response, err := atlassian.ServiceManagement.Request.Approval.Get(context.Background(), issueKey, approvalID)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   dataAsJson, err := json.MarshalIndent(approvalsMembers, "", "\t")
   if err != nil {
      log.Fatal(err)
   }

   log.Println(string(dataAsJson))

}
```

## Answer approval

This method enables a user to **Approve** or **Decline** an approval on a customer request. The approval is assumed to be owned by the user making the call.

```go
package main

import (
   "context"
   "encoding/json"
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
      issueKey   = "DESK-12"
      approvalID = 2
   )

   approvalsMembers, response, err := atlassian.ServiceManagement.Request.Approval.Answer(context.Background(), issueKey, approvalID, true)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   dataAsJson, err := json.MarshalIndent(approvalsMembers, "", "\t")
   if err != nil {
      log.Fatal(err)
   }

   log.Println(string(dataAsJson))

}
```


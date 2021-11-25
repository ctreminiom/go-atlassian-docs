# 🪜 Organization

## Get organizations

This method returns a list of organizations in the Jira Service Management instance. Use this method when you want to present a list of organizations or want to locate an organization by name.

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
      accountID = ""
      start     = 0
      limit     = 50
   )

   organizations, response, err := atlassian.ServiceManagement.Organization.Gets(context.Background(), accountID, start, limit)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, organization := range organizations.Values {
      log.Println(organization)
   }

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type OrganizationPageScheme struct {
   Size       int                   `json:"size"`
   Start      int                   `json:"start"`
   Limit      int                   `json:"limit"`
   IsLastPage bool                  `json:"isLastPage"`
   Values     []*OrganizationScheme `json:"values"`
   Expands    []string              `json:"_expands"`
   Links      struct {
      Self    string `json:"self"`
      Base    string `json:"base"`
      Context string `json:"context"`
      Next    string `json:"next"`
      Prev    string `json:"prev"`
   } `json:"_links"`
}

type OrganizationScheme struct {
   ID    string `json:"id"`
   Name  string `json:"name"`
   Links struct {
      Self string `json:"self"`
   } `json:"_links"`
}
```

## Create organization

This method creates an organization by passing the name of the organization.

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

   var organizationName = "Organization Name"

   newOrganization, response, err := atlassian.ServiceManagement.Organization.Create(context.Background(), organizationName)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
   log.Printf("The organization has been created: %v", newOrganization.ID)
}
```

## Get organization

```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/jira"
   "log"
   "os"
)

func main()  {

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

   organization, response, err := atlassian.ServiceManagement.Organization.Get(context.Background(), 2)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
   log.Println(organization.ID, organization.Name, organization.Links.Self)
}
```

## Get organizations

This method returns a list of all organizations associated with a service desk.

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
      accountID = ""
      projectID = 1
      start     = 0
      limit     = 50
   )

   organizations, response, err := atlassian.ServiceManagement.Organization.Project(
      context.Background(),
      accountID,
      projectID,
      start,
      limit,
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

   for _, organization := range organizations.Values {
      log.Printf(organization.ID)
   }

}
```

## Associate organization

This method adds an organization to a service desk. If the organization ID is already associated with the service desk, no change is made and the resource returns a 204 success code.

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
      projectID      = 1
      organizationID = 2
   )

   response, err := atlassian.ServiceManagement.Organization.Associate(context.Background(), projectID, organizationID)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Detach organization

This method removes an organization from a service desk. If the organization ID does not match an organization associated with the service desk, no change is made and the resource returns a 204 success code.

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
      projectID      = 1
      organizationID = 2
   )

   response, err := atlassian.ServiceManagement.Organization.Detach(context.Background(), projectID, organizationID)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Get users in organization

This method returns all the users associated with an organization. Use this method where you want to provide a list of users for an organization or determine if a user is associated with an organization.w

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
      organizationID = 2
      start          = 0
      limit          = 50
   )

   users, response, err := atlassian.ServiceManagement.Organization.Users(context.Background(), organizationID, start, limit)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, user := range users.Values {
      log.Printf(user.DisplayName, user.EmailAddress)
   }

}
```

## Add users to organization

This method adds users to an organization.

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
      accountIDs     = []string{"5b86be50b8e3cb5895860d6d"}
      organizationID = 2
   )

   response, err := atlassian.ServiceManagement.Organization.Add(context.Background(), organizationID, accountIDs)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Remove users from organization

This method removes users from an organization.

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
      accountIDs     = []string{"5b86be50b8e3cb5895860d6d"}
      organizationID = 2
   )

   response, err := atlassian.ServiceManagement.Organization.Remove(context.Background(), organizationID, accountIDs)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

}
```

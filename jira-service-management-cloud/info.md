# 🖥️ Info

## Get info

This method retrieves information about the Jira Service Management instance such as software version, builds, and related links.

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

   info, response, err := atlassian.ServiceManagement.Info.Get(context.Background())
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
   log.Println(info)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type InfoScheme struct {
   Version         string `json:"version"`
   PlatformVersion string `json:"platformVersion"`
   BuildDate       struct {
      Iso8601     string `json:"iso8601"`
      Jira        string `json:"jira"`
      Friendly    string `json:"friendly"`
      EpochMillis int64  `json:"epochMillis"`
   } `json:"buildDate"`
   BuildChangeSet   string `json:"buildChangeSet"`
   IsLicensedForUse bool   `json:"isLicensedForUse"`
   Links            struct {
      Self string `json:"self"`
   } `json:"_links"`
}
```


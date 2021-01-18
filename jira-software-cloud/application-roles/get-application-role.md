# Get Application Role

### Overview

This method returns an existing application role using the **key** as a parameter.

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
      log.Fatal(err)
   }

   atlassian.Auth.SetBasicAuth(mail, token)

   role, response, err := atlassian.Role.Get(context.Background(), "jira-software")
   if err != nil {
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)
   log.Println(role.Key)
}
```




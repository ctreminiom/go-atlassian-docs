# Get Application Roles

### Overview

This method returns all application roles created on your Jira Cloud instance. 

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

	myCloudInstance, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	myCloudInstance.Auth.SetBasicAuth(mail, token)

	roles, response, err := myCloudInstance.Role.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, role := range *roles {
	
		log.Println(role.Key)
		log.Println(role.Name)
		log.Println(role.Groups)
		
		log.Println(role.DefaultGroups)
		log.Println(role.SelectedByDefault)
		log.Println(role.Defined)
		
		log.Println(role.NumberOfSeats)
		log.Println(role.RemainingSeats)
		log.Println(role.UserCount)
		
		log.Println(role.UserCountDescription)
		log.Println(role.HasUnlimitedSeats)
		log.Println(role.Platform)
		
	}
}
```


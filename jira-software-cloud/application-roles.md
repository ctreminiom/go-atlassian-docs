---
description: >-
  This resource represents application roles. Use it to get details of an
  application role or all application roles.
---

# 🌐 Application Roles

## Application Roles

This method returns all application roles stored on your Jira Cloud instance:

### Example

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	applicationRoles, response, err := jiraCloud.Role.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, applicationRole := range *applicationRoles {
		log.Printf("Application Role Name: %v", applicationRole.Name)
		log.Printf("Application Role Key: %v", applicationRole.Key)
		log.Printf("Application Role User Count: %v", applicationRole.UserCount)
	}

}

```

## Application Role

This method returns an existing application role using the **key** as a parameter:

### Example

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	applicationRole, response, err := jiraCloud.Role.Get(context.Background(), "jira-software")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Application Role Name: %v", applicationRole.Name)
	log.Printf("Application Role Key: %v", applicationRole.Key)
	log.Printf("Application Role User Count: %v", applicationRole.UserCount)

	return

}


```


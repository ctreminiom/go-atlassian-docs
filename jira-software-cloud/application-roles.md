---
description: >-
  This resource represents application roles. Use it to get details of an
  application role or all application roles.
---

# 🔬 Application Roles

## Application Roles

This method returns all application roles stored on your Jira Cloud instance, the method returns the following information:

| variable | description |
| :--- | :--- |
| roles | A slice of the **ApplicationRoleScheme** struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

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

## Application Role

This method returns an existing application role using the **key** as a parameter, the method returns the following information:

| variable | description |
| :--- | :--- |
| role | An **ApplicationRoleScheme** struct with the application role information parsed |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

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

## **ApplicationRoleScheme** 

```go
type ApplicationRoleScheme struct {
	Key                  string   `json:"key,omitempty"`
	Groups               []string `json:"groups,omitempty"`
	Name                 string   `json:"name,omitempty"`
	DefaultGroups        []string `json:"defaultGroups,omitempty"`
	SelectedByDefault    bool     `json:"selectedByDefault,omitempty"`
	Defined              bool     `json:"defined,omitempty"`
	NumberOfSeats        int      `json:"numberOfSeats,omitempty"`
	RemainingSeats       int      `json:"remainingSeats,omitempty"`
	UserCount            int      `json:"userCount,omitempty"`
	UserCountDescription string   `json:"userCountDescription,omitempty"`
	HasUnlimitedSeats    bool     `json:"hasUnlimitedSeats,omitempty"`
	Platform             bool     `json:"platform,omitempty"`
}
```


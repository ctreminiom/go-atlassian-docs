# 🧱 Application Roles

This resource represents application roles. Use it to get details of an application role or all application roles.

![This is the Application Roles with the groups associated ](<../.gitbook/assets/image (3).png>)

### Get all application roles

&#x20;Returns all application roles. In Jira, application roles are managed using the [**Application access configuration**](https://confluence.atlassian.com/x/3YxjL) page.

* 🔒 **Permissions required**:  Administer Jira [global permission](https://confluence.atlassian.com/x/x4dKLg)

{% embed url="https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-application-roles/#api-rest-api-2-applicationrole-get" %}
Atlassian Official Documentation
{% endembed %}

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
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Application Role Name: %v", applicationRole.Name)
	log.Printf("Application Role Key: %v", applicationRole.Key)
	log.Printf("Application Role User Count: %v", applicationRole.UserCount)

	return

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

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

### Get application role

This method returns an existing application role using the **key **as a parameter

* 🔒 **Permissions required**:  Administer Jira [global permission](https://confluence.atlassian.com/x/x4dKLg)

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
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, applicationRole := range applicationRoles {
		log.Printf("Application Role Name: %v", applicationRole.Name)
		log.Printf("Application Role Key: %v", applicationRole.Key)
		log.Printf("Application Role User Count: %v", applicationRole.UserCount)
	}

}
```

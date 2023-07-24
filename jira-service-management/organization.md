---
cover: >-
  ../.gitbook/assets/the-keystroke-that-changed-how-i-worked-forever-compressed-1560x760.gif
coverY: 0
---

# 🛂 Organization

## Get organizations

`GET /rest/servicedeskapi/organization`

This method returns a list of organizations in the Jira Service Management instance. Use this method when you want to present a list of organizations or want to locate an organization by name.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
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

	organizations, response, err := atlassian.Organization.Gets(context.Background(), accountID, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, organization := range organizations.Values {
		log.Println(organization)
	}
}
```
{% endcode %}

## Create organization

`POST /rest/servicedeskapi/organization`

This method creates an organization by passing the name of the organization.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var organizationName = "Organization Name"

	newOrganization, response, err := atlassian.Organization.Create(context.Background(), organizationName)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The organization has been created: %v", newOrganization.ID)
}
```
{% endcode %}

## Get organization

`GET /rest/servicedeskapi/organization/{organizationId}`

Get returns details of an organization. Use this method to get organization details whenever your application component is passed an organization ID but needs to display other organization details.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	organization, response, err := atlassian.Organization.Get(context.Background(), 2)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(organization.ID, organization.Name, organization.Links.Self)
}
```
{% endcode %}

## Delete Organization

`DELETE /rest/servicedeskapi/organization/{organizationId}`

This method deletes an organization. Note that the organization is deleted regardless of other associations it may have. For example, associations with service desks.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	response, err := atlassian.Organization.Delete(context.Background(), 2)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}
}
```
{% endcode %}

## Get organizations

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/organization`

This method returns a list of all organizations associated with a service desk.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
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

	organizations, response, err := atlassian.Organization.Project(
		context.Background(),
		accountID,
		projectID,
		start,
		limit,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, organization := range organizations.Values {
		log.Printf(organization.ID)
	}

}
```
{% endcode %}

## Associate organization

`POST /rest/servicedeskapi/servicedesk/{serviceDeskId}/organization`

This method adds an organization to a service desk. If the organization ID is already associated with the service desk, no change is made and the resource returns a 204 success code.

{% code fullWidth="true" %}
```go
package main
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)
func main() {
	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)
	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}
	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")
	var (
		projectID      = 1
		organizationID = 2
	)
	response, err := atlassian.Organization.Associate(context.Background(), projectID, organizationID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Detach organization

`DELETE /rest/servicedeskapi/servicedesk/{serviceDeskId}/organization`

This method removes an organization from a service desk. If the organization ID does not match an organization associated with the service desk, no change is made and the resource returns a 204 success code.

{% code fullWidth="true" %}
```go
package main
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)
func main() {
	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)
	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}
	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")
	var (
		projectID      = 1
		organizationID = 2
	)
	response, err := atlassian.Organization.Detach(context.Background(), projectID, organizationID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get Project Organizations

## Get users in organization

`GET /rest/servicedeskapi/organization/{organizationId}/user`

This method returns all the users associated with an organization. Use this method where you want to provide a list of users for an organization or determine if a user is associated with an organization.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
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

	users, response, err := atlassian.Organization.Users(context.Background(), organizationID, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, user := range users.Values {
		log.Printf(user.DisplayName, user.EmailAddress)
	}
}
```
{% endcode %}

## Add users to organization

`POST /rest/servicedeskapi/organization/{organizationId}/user`

This method adds users to an organization.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		accountIDs     = []string{"5b86be50b8e3cb5895860d6d"}
		organizationID = 2
	)

	response, err := atlassian.Organization.Add(context.Background(), organizationID, accountIDs)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
</code></pre>

## Remove users from organization

`DELETE /rest/servicedeskapi/organization/{organizationId}/user`

This method removes users from an organization.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		accountIDs     = []string{"5b86be50b8e3cb5895860d6d"}
		organizationID = 2
	)

	response, err := atlassian.Organization.Remove(context.Background(), organizationID, accountIDs)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

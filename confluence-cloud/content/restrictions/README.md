# 🔞 Restrictions

## Get restrictions

Returns the restrictions on a piece of content.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"net/http"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	restrictions, response, err := instance.Content.Restriction.Gets(context.Background(), "80412692", []string{"restrictions.user", "restrictions.group"}, 0, 50)
	if err != nil {
		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, restriction := range restrictions.Results {

		if restriction.Restrictions.Group != nil {
			for _, group := range restriction.Restrictions.Group.Results {
				fmt.Println(restriction.Operation, group.Name)
			}
		}

		if restriction.Restrictions.User != nil {
			for _, user := range restriction.Restrictions.User.Results {
				fmt.Println(restriction.Operation, user.Email, user.AccountID, user.AccountType)
			}
		}
	}

}
```

## Add Restrictions

Adds restrictions to a piece of content. Note, this does not change any existing restrictions on the content.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload :=  &models.ContentRestrictionUpdatePayloadScheme{
		Results: []*models.ContentRestrictionUpdateScheme{
			{
				Operation: "update",
				Restrictions: &models.ContentRestrictionRestrictionUpdateScheme{
					Group: []*models.SpaceGroupScheme{
						{
							Type: "group",
							Name: "jira-users-22",
						},
					},
					User: []*models.ContentUserScheme{
						{
							AccountID:      "5e5f6acefc1fca0af44135f8",
						},
					},
				},
			},
		},
	}

	contentID := "80412692"
	expand := []string{"restrictions.user", "restrictions.group", "content"}
	ctx := context.Background()

	restrictions, response, err := instance.Content.Restriction.Add(ctx, contentID, payload, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Code)
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, restriction := range restrictions.Results {

		if restriction.Restrictions.Group != nil {
			for _, group := range restriction.Restrictions.Group.Results {
				fmt.Println(restriction.Operation, group.Name)
			}
		}

		if restriction.Restrictions.User != nil {
			for _, user := range restriction.Restrictions.User.Results {
				fmt.Println(restriction.Operation, user.Email, user.AccountID, user.AccountType)
			}
		}
	}


}

```

## Update Restrictions

Updates restrictions for a piece of content. This removes the existing restrictions and replaces them with the restrictions in the request.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload :=  &models.ContentRestrictionUpdatePayloadScheme{
		Results: []*models.ContentRestrictionUpdateScheme{
			{
				Operation: "update",
				Restrictions: &models.ContentRestrictionRestrictionUpdateScheme{
					Group: []*models.SpaceGroupScheme{
						{
							Type: "group",
							Name: "jira-users-22",
						},
					},
				},
			},
		},
	}

	contentID := "80412692"
	expand := []string{"restrictions.user", "restrictions.group", "content"}
	ctx := context.Background()

	restrictions, response, err := instance.Content.Restriction.Update(ctx, contentID, payload, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Code)
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, restriction := range restrictions.Results {

		if restriction.Restrictions.Group != nil {
			for _, group := range restriction.Restrictions.Group.Results {
				fmt.Println(restriction.Operation, group.Name)
			}
		}

		if restriction.Restrictions.User != nil {
			for _, user := range restriction.Restrictions.User.Results {
				fmt.Println(restriction.Operation, user.Email, user.AccountID, user.AccountType)
			}
		}
	}

}
```

## Delete Restrictions

Removes all restrictions (read and update) on a piece of content.

```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	contentID := "80412692"
	expand := []string{"restrictions.user", "restrictions.group", "content"}
	ctx := context.Background()

	restrictions, response, err := instance.Content.Restriction.Delete(ctx, contentID, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Code)
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, restriction := range restrictions.Results {

		if restriction.Restrictions.Group != nil {
			for _, group := range restriction.Restrictions.Group.Results {
				fmt.Println(restriction.Operation, group.Name)
			}
		}

		if restriction.Restrictions.User != nil {
			for _, user := range restriction.Restrictions.User.Results {
				fmt.Println(restriction.Operation, user.Email, user.AccountID, user.AccountType)
			}
		}
	}

}
```

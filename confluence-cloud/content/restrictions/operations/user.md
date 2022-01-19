# 👤 User

## Get content restriction status for user

Returns whether the specified content restriction applies to a user.&#x20;

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-user-get" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
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
	ctx := context.Background()
	operationKey := "update"
	accountID := "5b86be50b8e3cb5895860d6d"

	response, err := instance.Content.Restriction.Operation.User.Get(ctx, contentID, operationKey, accountID)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}
```

## Add user to content restriction

Adds a user to a content restriction. That is, grant read or update permission to the user for a piece of content.

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-user-put" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
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
	ctx := context.Background()
	operationKey := "update"
	accountID := "5e5f6a63157ed50cd2b9eaca"

	response, err := instance.Content.Restriction.Operation.User.Add(ctx, contentID, operationKey, accountID)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}

```

## Remove user from content restriction

Removes a group from a content restriction. That is, remove read or update permission for the group for a piece of content.

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-user-delete" %}
Official Documentation
{% endembed %}

```go
package main

import (
	"context"
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
	ctx := context.Background()
	operationKey := "update"
	accountID := "5e5f6a63157ed50cd2b9eaca"

	response, err := instance.Content.Restriction.Operation.User.Remove(ctx, contentID, operationKey, accountID)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}

```

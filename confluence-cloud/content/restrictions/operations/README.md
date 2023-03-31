# 🎑 Operations

## Get Restrictions by Operation

Returns restrictions on a piece of content by operation. This method is similar to [Get restrictions](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions/) except that the operations are properties of the return object, rather than items in a results array.

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

	restrictions, response, err := instance.Content.Restriction.Operation.Gets(ctx, contentID, expand)
	if err != nil {
		if response != nil {
			log.Println(response.Code)
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(restrictions)
	
}
```

## Get Restrictions for Operation

Returns the restictions on a piece of content for a given operation (read or update).

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
	operationKey := "read"

	restrictions, response, err := instance.Content.Restriction.Operation.Get(ctx, contentID, operationKey, expand, 0, 50)
	if err != nil {
		if response != nil {
			log.Println(response.Code)
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(restrictions)
}
```

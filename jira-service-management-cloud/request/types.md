# 💾 Types

## Get all request types

This method returns all customer request types used in the Jira Service Management instance, optionally filtered by a query string.

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

   var (
      query        = ""
      start, limit = 0, 50
   )

   requestTypes, response, err := atlassian.ServiceManagement.RequestType.Search(context.Background(), query, start, limit)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
         log.Println("HTTP Endpoint Used", response.Endpoint)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, requestType := range requestTypes.Values {
      log.Println(requestType.Name, requestType.Name)
   }

}
```

## Get request types

This method returns all customer request types from a service desk. There are two parameters for filtering the returned list:

* `groupId` which filters the results to items in the customer request type group.
* `searchQuery` which is matched against request types' `name` or `description`. For example, the strings "Install", "Inst", "Equi", or "Equipment" will match a request type with the _name_ "Equipment Installation Request".

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

	var (
		ctx                        = context.Background()
		serviceDeskID, groupID int = 1, 0
		start, limit           int = 0, 50
	)

	requestTypes, response, err := atlassian.ServiceManagement.RequestType.Gets(ctx, serviceDeskID, groupID, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, requestType := range requestTypes.Values {
		log.Println(requestType.Name, requestType.Name)
	}

}

```

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type ProjectRequestTypePageScheme struct {
   Expands    []string `json:"_expands"`
   Size       int      `json:"size"`
   Start      int      `json:"start"`
   Limit      int      `json:"limit"`
   IsLastPage bool     `json:"isLastPage"`
   Links      struct {
      Base    string `json:"base"`
      Context string `json:"context"`
      Next    string `json:"next"`
      Prev    string `json:"prev"`
   } `json:"_links"`
   Values []*RequestTypeScheme `json:"values"`
}
```

## Create request type

This method enables a customer request type to be added to a service desk based on an issue type. Note that not all customer request type fields can be specified in the request and these fields are given the following default values:

* Request type icon is given the headset icon.
* Request type groups is left empty, which means this customer request type will not be visible on the [customer portal](https://confluence.atlassian.com/servicedeskcloud/configuring-the-customer-portal-732528918.html)

.

Request type status mapping is left empty, so the request type has no custom status mapping but inherits the status map from the issue type upon which it is based.

Request type field mapping is set to show the required fields as specified by the issue type used to create the customer request type.

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

	var (
		ctx           = context.Background()
		serviceDeskID = 1
		issueTypeID   = "10001"
		name          = "Request Type Sample Name"
		description   = "Request Type Sample Description"
		helpText      = "Request Type Sample HelpText"
	)

	newRequestType, response, err := atlassian.ServiceManagement.RequestType.Create(ctx,
		serviceDeskID,
		issueTypeID,
		name,
		description,
		helpText,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("newRequestType", newRequestType.ID, newRequestType.Name)
}

```

## Get request type by id

This method returns a customer request type from a service desk.

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

	var (
		serviceDeskID = 1
		requestTypeID = 9
	)

	requestType, response, err := atlassian.ServiceManagement.RequestType.Get(context.Background(), serviceDeskID, requestTypeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("newRequestType", requestType.ID, requestType.Name)

}

```

## Delete request type

This method deletes a customer request type from a service desk, and removes it from all customer requests.

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

	var (
		serviceDeskID = 1
		requestTypeID = 9
	)

	response, err := atlassian.ServiceManagement.RequestType.Delete(context.Background(), serviceDeskID, requestTypeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Get request type fields

This method returns the fields for a service desk's customer request type.

Also, the following information about the user's permissions for the request type is returned:

* `canRaiseOnBehalfOf` returns `true` if the user has permission to raise customer requests on behalf of other customers. Otherwise, returns `false`.
* `canAddRequestParticipants` returns `true` if the user can add customer request participants. Otherwise, returns `false`.

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

	var (
		serviceDeskID = 1
		requestTypeID = 9
	)

	fields, response, err := atlassian.ServiceManagement.RequestType.Fields(context.Background(), serviceDeskID, requestTypeID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range fields.RequestTypeFields {
		log.Println(field.Name, field.Description, field.FieldID)
	}
}

```


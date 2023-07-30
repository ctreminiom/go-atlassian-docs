---
cover: >-
  ../../../../.gitbook/assets/vanessa_lovegrove_cognitive_overload_1120x545@2x-1560x760.jpeg
coverY: 0
---

# 🫂 Group

## Get content restriction status for group

`GET /wiki/rest/api/content/{id}/restriction/byOperation/{operation}/group/{group}`

Returns whether the specified content restriction applies to a group.&#x20;

* This endpoint combines the get content restriction status by group name and group id.
* If you provide the group id (UUID), the method will create the endpoint with the verb /**byGroupId**/{groupNameOrID}
* If you provide the group name, the endpoint uses the verb **/group/**{groupNameOrID}.

```go
var endpoint strings.Builder
endpoint.WriteString(fmt.Sprintf("wiki/rest/api/content/%v/restriction/byOperation/%v/", contentID, operationKey))

// check if the group id is an uuid type
// if so, it's the group id
groupID, err := uuid.Parse(groupNameOrID)

if err == nil {
	endpoint.WriteString(fmt.Sprintf("byGroupId/%v", groupID.String()))
} else {
	endpoint.WriteString(fmt.Sprintf("group/%v", groupNameOrID))
}
```

{% code fullWidth="true" %}
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
	groupNameOrID := "confluence-users"

	response, err := instance.Content.Restriction.Operation.Group.Get(ctx, contentID, operationKey, groupNameOrID)
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
{% endcode %}

## Add group to content restriction

`PUT /wiki/rest/api/content/{id}/restriction/byOperation/{operation}/group/{grou[}`

Adds a group to a content restriction. That is, grant read or update permission to the group for a piece of content.

{% code fullWidth="true" %}
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
	operationKey := "read"
	groupNameOrID := "jira-users-22"

	response, err := instance.Content.Restriction.Operation.Group.Add(ctx, contentID, operationKey, groupNameOrID)
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
{% endcode %}

## Remove group from content restriction

`DELETE /wiki/rest/api/content/{id}/restriction/byOperation/{operationKey}/group/{groupName}`

Removes a group from a content restriction. That is, remove read or update permission for the group for a piece of content.

{% code fullWidth="true" %}
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
	operationKey := "read"
	groupNameOrID := "jira-users-22"

	response, err := instance.Content.Restriction.Operation.Group.Remove(ctx, contentID, operationKey, groupNameOrID)
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
{% endcode %}

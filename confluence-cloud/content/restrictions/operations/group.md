# 🫂 Group

## Get content restriction status for group

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

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-group-groupname-get" %}
Official Documentation / Group Name
{% endembed %}

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-bygroupid-groupid-get" %}
Official Documentation / Group ID
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

## Add group to content restriction

Adds a group to a content restriction. That is, grant read or update permission to the group for a piece of content.

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-group-groupname-put" %}
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
e
```

## Remove group from content restriction

Removes a group from a content restriction. That is, remove read or update permission for the group for a piece of content.

{% embed url="https://developer.atlassian.com/cloud/confluence/rest/api-group-content-restrictions#api-wiki-rest-api-content-id-restriction-byoperation-operationkey-group-groupname-delete" %}
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

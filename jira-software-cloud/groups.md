---
description: >-
  This resource represents groups of users. Use it to get, create, find, and
  delete groups as well as add and remove users from groups.
---

# 👫 Groups

{% page-ref page="groups.md" %}

{% tabs %}
{% tab title="Create group" %}
Creates a group.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-post).
{% endhint %}

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

	group, response, err := atlassian.Group.Create(context.Background(), "jira-users")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Group created", group.Name)
}

```
{% endtab %}

{% tab title="Remove group" %}
Deletes a group.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-delete).
{% endhint %}

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

	response, err := atlassian.Group.Delete(context.Background(), groupName)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endtab %}

{% tab title="Bulk get groups" %}
Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of groups. 

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-bulk-get).
{% endhint %}

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

	options := jira.GroupBulkOptionsScheme{
		GroupIDs:   nil,
		GroupNames: nil,
	}

	groups, response, err := atlassian.Group.Bulk(context.Background(), &options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(groups.IsLast)

	for index, group := range groups.Values {
		groupsNames = append(groupsNames, group.Name)
		log.Printf("#%v, Group: %v", index, group.Name)
	}
}

```
{% endtab %}

{% tab title="Get users from group" %}
 Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all users in a group.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-member-get).
{% endhint %}

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

	members, response, err := atlassian.Group.Members(context.Background(), groupName, false, 0, 100)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(members.IsLast)

	for index, member := range members.Values {
		log.Printf("#%v - Group %v - Member Mail %v - Member AccountID %v", index, groupName, member.EmailAddress, member.AccountID)
	}
}

```
{% endtab %}

{% tab title="Add user to group" %}
Adds a user to a group.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-user-post).
{% endhint %}

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

	_, response, err := atlassian.Group.Add(context.Background(), "groupName", "accountID")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endtab %}

{% tab title="Remove user from group" %}
Removes a user from a group.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-groups/#api-rest-api-3-group-user-delete).
{% endhint %}

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

	response, err := atlassian.Group.Remove(context.Background(), "groupName", "accountID")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endtab %}
{% endtabs %}


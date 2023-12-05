---
cover: ../.gitbook/assets/2240x1090-1-1560x760.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 👫 Groups

In Jira, a group is a collection of users who have similar roles, responsibilities, or permissions. Groups can be used to simplify user management by allowing you to grant permissions and roles to entire groups of users rather than individual users.

Here are some ways you can use groups in Jira:

1. **Assigning permissions:** You can assign permissions to a group of users instead of individual users. For example, you might create a group called "Developers" and grant them permissions to create and modify issues.
2. **Sharing filters and dashboards:** You can share filters and dashboards with groups of users. For example, you might create a filter that shows all issues assigned to the "Developers" group and share it with all members of that group.
3. **Managing notifications:** You can use groups to manage notifications in Jira. For example, you might create a group called "Product Owners" and add all product owners to that group. You can then set up notifications so that all members of the "Product Owners" group are notified when certain events occur in Jira.
4. **Restricting access:** You can use groups to restrict access to certain parts of Jira. For example, you might create a group called "Administrators" and grant them access to sensitive parts of Jira such as user management and system settings.

## Create Group

`POST /rest/api/{2-3}/group`

Creates a group

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	group, response, err := atlassian.Group.Create(context.Background(), "jira-users-22")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Group created", group.Name)
}
```
{% endcode %}

## Remove group

`DELETE /rest/api/{2-3}/group`

Deletes a group

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Group.Delete(context.Background(), "jira-users-2")
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endcode %}

## Bulk Groups

`GET /rest/api/{2-3}/group/bulk`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of groups, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := models.GroupBulkOptionsScheme{
		GroupIDs:   nil,
		GroupNames: nil,
	}

	groups, response, err := atlassian.Group.Bulk(context.Background(), &options, 0, 50)
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(groups.IsLast)

	for index, group := range groups.Values {
		log.Printf("#%v, Group: %v", index, group.Name)
	}
}
```
{% endcode %}

## Get users from groups

`GET /rest/api/{2-3}/group/member`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all users in a group

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	members, response, err := atlassian.Group.Members(context.Background(), "jira-users", false, 0, 100)
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(members.IsLast)

	for index, member := range members.Values {
		log.Printf("#%v - Group %v - Member Mail %v - Member AccountID %v", index, "jira-users", member.EmailAddress, member.AccountID)
	}
}
```
{% endcode %}

## Add user to group

`POST /rest/api/{2-3}/group/user`

Adds a user to a group

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	_, response, err := atlassian.Group.Add(context.Background(), "groupName", "accountID")
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Remove user from group

`DELETE /rest/api/{2-3}/group/user`

Removes a user from a group

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Group.Remove(context.Background(), "groupName", "accountID")
	if err != nil {
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

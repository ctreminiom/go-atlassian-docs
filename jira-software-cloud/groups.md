# 👫 Groups

A Jira group is a convenient way to manage a collection of users. You can use groups throughout Jira to:

* Allow application access.
* Grant global permissions or project-specific access.
* Receive email notifications.
* Access issue filters and dashboards.
* Reference workflow conditions.
* Integrate with project roles.

![](<../.gitbook/assets/image (6).png>)

This resource represents groups of users. Use it to get, create, find, and delete groups as well as add and remove users from groups.

## Create Group

Creates a group

* 🔒 **Permissions required**:  Site administration

```go
package main

import (
	"context"
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

## Remove group

Deletes a group

* 🔒 **Permissions required**:  Site administration

```go
package main

import (
	"context"
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

## Bulk Groups

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of groups, the method returns the following information:

```go
package main

import (
	"context"
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

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type BulkGroupScheme struct {
	MaxResults int  `json:"maxResults,omitempty"`
	StartAt    int  `json:"startAt,omitempty"`
	Total      int  `json:"total,omitempty"`
	IsLast     bool `json:"isLast,omitempty"`
	Values     []struct {
		Name    string `json:"name,omitempty"`
		GroupID string `json:"groupId,omitempty"`
	} `json:"values,omitempty"`
}
```

## Get users from groups

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all users in a group, the method returns the following information:

```go
package main

import (
	"context"
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

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type GroupMemberPageScheme struct {
	Self       string                   `json:"self,omitempty"`
	NextPage   string                   `json:"nextPage,omitempty"`
	MaxResults int                      `json:"maxResults,omitempty"`
	StartAt    int                      `json:"startAt,omitempty"`
	Total      int                      `json:"total,omitempty"`
	IsLast     bool                     `json:"isLast,omitempty"`
	Values     []*GroupUserDetailScheme `json:"values,omitempty"`
}

type GroupUserDetailScheme struct {
	Self         string `json:"self"`
	Name         string `json:"name"`
	Key          string `json:"key"`
	AccountID    string `json:"accountId"`
	EmailAddress string `json:"emailAddress"`
	DisplayName  string `json:"displayName"`
	Active       bool   `json:"active"`
	TimeZone     string `json:"timeZone"`
	AccountType  string `json:"accountType"`
}
```

## Add user to group

Adds a user to a group

```go
package main

import (
	"context"
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

## Remove user from group

Removes a user from a group

```go
package main

import (
	"context"
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

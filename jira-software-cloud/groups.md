---
description: >-
  This resource represents groups of users. Use it to get, create, find, and
  delete groups as well as add and remove users from groups.
---

# 👫 Groups

## Create Group

Creates a group, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `GroupScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| name | the group name to create |

### Example

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

### GroupScheme

```go
type GroupScheme struct {
   Name  string `json:"name,omitempty"`
   Self  string `json:"self,omitempty"`
   Users struct {
      Size  int `json:"size,omitempty"`
      Items []struct {
         Self        string `json:"self,omitempty"`
         AccountID   string `json:"accountId,omitempty"`
         DisplayName string `json:"displayName,omitempty"`
         Active      bool   `json:"active,omitempty"`
      } `json:"items,omitempty"`
      MaxResults int `json:"max-results,omitempty"`
      StartIndex int `json:"start-index,omitempty"`
      EndIndex   int `json:"end-index,omitempty"`
   } `json:"users,omitempty"`
   Expand string `json:"expand,omitempty"`
}
```

## Remove group

Deletes a group,  the method returns the following information:

| variable | description |
| :--- | :--- |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| name | the group name to remove |

### Example

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

	response, err := atlassian.Group.Delete(context.Background(), "groupName")
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

## Bulk Groups

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of groups, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `BulkGroupScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| options | A `GroupBulkOptionsScheme` struct |
| startAt | the pagination start index |
| maxResults | the maximum values returned per pagination |

### Example

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

### GroupBulkOptionsScheme

```go
type GroupBulkOptionsScheme struct {
   GroupIDs   []string
   GroupNames []string
}
```

## Get users from groups

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all users in a group, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `GroupUsersScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name  | description |
| :--- | :--- |
| ctx | a context.Context instance |
| group | the group name to extract the members |
| inactive | select if you want to return the **inactive** users |
| startAt | the pagination start index |
| maxResults | the maximum values returned per pagination |

### Example

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

	members, response, err := atlassian.Group.Members(context.Background(), "groupName", false, 0, 100)
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

### GroupUsersScheme

```go
type GroupUsersScheme struct {
   Self       string `json:"self,omitempty"`
   NextPage   string `json:"nextPage,omitempty"`
   MaxResults int    `json:"maxResults,omitempty"`
   StartAt    int    `json:"startAt,omitempty"`
   Total      int    `json:"total,omitempty"`
   IsLast     bool   `json:"isLast,omitempty"`
   Values     []struct {
      Self         string `json:"self,omitempty"`
      Name         string `json:"name,omitempty"`
      Key          string `json:"key,omitempty"`
      AccountID    string `json:"accountId,omitempty"`
      EmailAddress string `json:"emailAddress,omitempty"`
      AvatarUrls   struct {
      } `json:"avatarUrls,omitempty"`
      DisplayName string `json:"displayName,omitempty"`
      Active      bool   `json:"active,omitempty"`
      TimeZone    string `json:"timeZone,omitempty"`
      AccountType string `json:"accountType,omitempty"`
   } `json:"values,omitempty"`
}
```

## Add user to group

Adds a user to a group, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `GroupScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| group | the Jira Cloud group |
| accountID | the Atlassian account ID |

### Example

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

## Remove user from group

Removes a user from a group, the method returns the following information:

| variable | description |
| :--- | :--- |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| group | the Jira Cloud group |
| accountID | the Atlassian account ID |

### Example

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


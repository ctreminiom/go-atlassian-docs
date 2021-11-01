# 📈 Dashboards

This resource represents dashboards. Use it to obtain the details of dashboards as well as add and remove item properties from dashboards.

![.gif created using the video https://www.youtube.com/watch?v=VswPTqLQzqA](../.gitbook/assets/atlassian-api-dashboard.gif)



### Get all dashboards

Returns a list of dashboards owned by or shared with the user. The list may be filtered to include only favorite or owned dashboards.

* 🔒 **Permissions required**:  Anonymously

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	dashboards, response, err := jiraCloud.Dashboard.Gets(context.Background(), 0, 50, "")

	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, dashboard := range dashboards.Dashboards {
		log.Println(dashboard.ID, dashboard.Name)
	}

	return

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type DashboardPageScheme struct {
	StartAt    int               `json:"startAt,omitempty"`
	MaxResults int               `json:"maxResults,omitempty"`
	Total      int               `json:"total,omitempty"`
	Dashboards []*DashboardScheme `json:"dashboards,omitempty"`
}

type DashboardScheme struct {
	ID               string                   `json:"id,omitempty"`
	IsFavourite      bool                     `json:"isFavourite,omitempty"`
	Name             string                   `json:"name,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []*SharePermissionScheme `json:"sharePermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}

type SharePermissionScheme struct {
	Type    string             `json:"type"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}
```

### Create dashboard

This method creates a dashboard on Jira Cloud.

{% hint style="warning" %}
This method is **Experimental**
{% endhint %}

* 🔒 **Permissions required**:  Access to the application

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	var payload = &jira.DashboardPayloadScheme{
		Name:        "Team Tracking 3",
		Description: "description sample",
		SharePermissions: []*jira.SharePermissionScheme{
			{
				Type: "project",
				Project: &jira.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &jira.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	dashboard, response, err := jiraCloud.Dashboard.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type DashboardScheme struct {
	ID               string                   `json:"id,omitempty"`
	IsFavourite      bool                     `json:"isFavourite,omitempty"`
	Name             string                   `json:"name,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []*SharePermissionScheme `json:"sharePermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}


type SharePermissionScheme struct {
	Type    string             `json:"type"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}
```

### Search for dashboards

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of dashboards. This operation is similar to [Get dashboards](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-dashboards/#api-rest-api-3-dashboard-get) except that the results can be refined to include dashboards that have specific attributes.&#x20;

{% hint style="info" %}
For example, dashboards with a particular name. When multiple attributes are specified only filters matching all attributes are returned.
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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	searchOptions := jira.DashboardSearchOptionsScheme{
		DashboardName:       "Bug",
		GroupPermissionName: "administrators",
		OrderBy:             "description",
		Expand:              []string{"description", "favourite", "sharePermissions"},
	}

	dashboards, response, err := jiraCloud.Dashboard.Search(context.Background(), &searchOptions, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, dashboard := range dashboards.Values {
		log.Printf("Dashboard Name: %v", dashboard.Name)
		log.Printf("Dashboard ID: %v", dashboard.ID)
		log.Printf("Dashboard View: %v", dashboard.View)

		if dashboard.SharePermissions != nil {
			for _, permission := range dashboard.SharePermissions {
				log.Println(permission)
			}
		}
	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type DashboardSearchScheme struct {
	Self       string `json:"self"`
	MaxResults int    `json:"maxResults"`
	StartAt    int    `json:"startAt"`
	Total      int    `json:"total"`
	IsLast     bool   `json:"isLast"`
	Values     []struct {
		Description      string                   `json:"description,omitempty"`
		ID               string                   `json:"id"`
		IsFavourite      bool                     `json:"isFavourite"`
		Name             string                   `json:"name"`
		Owner            *UserScheme              `json:"owner,omitempty"`
		Popularity       int                      `json:"popularity"`
		Self             string                   `json:"self"`
		SharePermissions []*SharePermissionScheme `json:"sharePermissions"`
		View             string                   `json:"view"`
		Rank             int    					`json:"rank"`
	} `json:"values"`
}

type UserScheme struct {
	Self             string                      `json:"self,omitempty"`
	Key              string                      `json:"key,omitempty"`
	AccountID        string                      `json:"accountId,omitempty"`
	AccountType      string                      `json:"accountType,omitempty"`
	Name             string                      `json:"name,omitempty"`
	EmailAddress     string                      `json:"emailAddress,omitempty"`
	AvatarUrls       *AvatarURLScheme            `json:"avatarUrls,omitempty"`
	DisplayName      string                      `json:"displayName,omitempty"`
	Active           bool                        `json:"active,omitempty"`
	TimeZone         string                      `json:"timeZone,omitempty"`
	Locale           string                      `json:"locale,omitempty"`
	Groups           *UserGroupsScheme           `json:"groups,omitempty"`
	ApplicationRoles *UserApplicationRolesScheme `json:"applicationRoles,omitempty"`
	Expand           string                      `json:"expand,omitempty"`
}


type SharePermissionScheme struct {
	Type    string             `json:"type"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}
```

### Get dashboard

Returns a dashboard using the _dashboard-id_

* 🔒 **Permissions required**:  Access to the application

{% hint style="warning" %}
&#x20;However, to get a dashboard, the dashboard must be shared with the user or the user must own it. Note, users with _**Administer Jira**_ [global permission](https://confluence.atlassian.com/x/x4dKLg) are considered owners of the System dashboard. The System dashboard is considered to be shared with all other users.
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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	dashboard, response, err := jiraCloud.Dashboard.Get(context.Background(), "10001")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type DashboardScheme struct {
	ID               string                   `json:"id,omitempty"`
	IsFavourite      bool                     `json:"isFavourite,omitempty"`
	Name             string                   `json:"name,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []*SharePermissionScheme `json:"sharePermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}

type SharePermissionScheme struct {
	Type    string             `json:"type"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}

```

### Update dashboard

Updates a dashboard, replacing all the dashboard details with those provided. **The dashboard to be updated must be owned by the user.**

* 🔒 **Permissions required**:  Access to the application

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	var payload = &jira.DashboardPayloadScheme{
		Name:             "Team Tracking #1111",
		Description:      "",
		SharePermissions: []*jira.SharePermissionScheme{
			{
				Type: "project",
				Project: &jira.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &jira.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	dashboard, response, err := jiraCloud.Dashboard.Update(context.Background(), "10001", payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}
```

### Delete dashboard

Deletes a dashboard**, The dashboard to be deleted must be owned by the user.**

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	response, err := jiraCloud.Dashboard.Delete(context.Background(), "10003")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

### Copy dashboard

Copies a dashboard. Any values provided in the `dashboard` parameter replace those in the copied dashboard.

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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	var payload = &jira.DashboardPayloadScheme{
		Name:             "Team Tracking #2 copy",
		Description:      "Description sample",
		SharePermissions: []*jira.SharePermissionScheme{
			{
				Type: "project",
				Project: &jira.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &jira.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	dashboard, response, err := jiraCloud.Dashboard.Copy(context.Background(), "10001", payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}
```




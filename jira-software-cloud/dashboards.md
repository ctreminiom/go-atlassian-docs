# 📈 Dashboards

This resource represents dashboards. Use it to obtain the details of dashboards as well as add and remove item properties from dashboards.

![.gif created using the video https://www.youtube.com/watch?v=VswPTqLQzqA](../.gitbook/assets/atlassian-api-dashboard.gif)



## Get all dashboards

Returns a list of dashboards owned by or shared with the user. The list may be filtered to include only favorite or owned dashboards.

* 🔒 **Permissions required**:  Anonymously

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

	jiraCloud, err := v2.New(nil, host)
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
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type DashboardPageScheme struct {
	StartAt    int                `json:"startAt,omitempty"`
	MaxResults int                `json:"maxResults,omitempty"`
	Total      int                `json:"total,omitempty"`
	Dashboards []*DashboardScheme `json:"dashboards,omitempty"`
}

type DashboardScheme struct {
	ID               string                   `json:"id,omitempty"`
	IsFavourite      bool                     `json:"isFavourite,omitempty"`
	Name             string                   `json:"name,omitempty"`
	Owner            *UserScheme              `json:"owner,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Rank             int                      `json:"rank,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []*SharePermissionScheme `json:"sharePermissions,omitempty"`
	EditPermission   []*SharePermissionScheme `json:"editPermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}

type SharePermissionScheme struct {
	ID      int                `json:"id,omitempty"`
	Type    string             `json:"type,omitempty"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}

type DashboardSearchPageScheme struct {
	Self       string             `json:"self,omitempty"`
	MaxResults int                `json:"maxResults,omitempty"`
	StartAt    int                `json:"startAt,omitempty"`
	Total      int                `json:"total,omitempty"`
	IsLast     bool               `json:"isLast,omitempty"`
	Values     []*DashboardScheme `json:"values,omitempty"`
}
```

## Create dashboard

This method creates a dashboard on Jira Cloud.

## Search for dashboards



## Get dashboard

Returns a dashboard using the _dashboard-id_

* 🔒 **Permissions required**:  Access to the application

{% hint style="warning" %}
&#x20;However, to get a dashboard, the dashboard must be shared with the user or the user must own it. Note, users with _**Administer Jira**_ [global permission](https://confluence.atlassian.com/x/x4dKLg) are considered owners of the System dashboard. The System dashboard is considered to be shared with all other users.
{% endhint %}

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

	jiraCloud, err := v2.New(nil, host)
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
	Owner            *UserScheme              `json:"owner,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Rank             int                      `json:"rank,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []*SharePermissionScheme `json:"sharePermissions,omitempty"`
	EditPermission   []*SharePermissionScheme `json:"editPermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}

type SharePermissionScheme struct {
	ID      int                `json:"id,omitempty"`
	Type    string             `json:"type,omitempty"`
	Project *ProjectScheme     `json:"project,omitempty"`
	Role    *ProjectRoleScheme `json:"role,omitempty"`
	Group   *GroupScheme       `json:"group,omitempty"`
}
```

## Update dashboard

Updates a dashboard, replacing all the dashboard details with those provided. **The dashboard to be updated must be owned by the user.**

## Delete dashboard

Deletes a dashboard**, The dashboard to be deleted must be owned by the user.**

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

	jiraCloud, err := v2.New(nil, host)
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

## Copy dashboard

Copies a dashboard. Any values provided in the `dashboard` parameter replace those in the copied dashboard.

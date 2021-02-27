---
description: >-
  This resource represents dashboards. Use it to obtain the details of
  dashboards as well as add and remove item properties from dashboards.
---

# 📈 Dashboards

## Share Permissions

Details of share permissions used on the Jira Cloud Dashboards.

### type 

The type of share permission:

* `group` Shared with a group. If set in a request, then specify `sharePermission.group` as well.
* `project` Shared with a project. If set in a request, then specify `sharePermission.project` as well.
* `projectRole` Share with a project role in a project. This value is not returned in responses. It is used in requests, where it needs to be specified with `projectId` and `projectRoleId`.
* `global` Shared globally. If set in a request, no other `sharePermission` properties need to be specified.
* `loggedin` Shared with all logged-in users. Note: This value is set in a request by specifying `authenticated` as the `type`.
* `project-unknown` Shared with a project that the user does not have access to. Cannot be set in a request.

Valid values: `group`, `project`, `projectRole`, `global`, `loggedin`, `authenticated`, `project-unknown`

### project

 The project that the filter is shared with. This is similar to the project object returned by [Get project](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-projects/#api-rest-api-3-project-projectidorkey-get) but it contains a subset of the properties, which are: `self`, `id`, `key`, `assigneeType`, `name`, `roles`, `avatarUrls`, `projectType`, `simplified`.  
For a request, specify the `id` for the project.

### role

 The project role that the filter is shared with.  
For a request, specify the `id` for the role. You must also specify the `project` object and `id` for the project that the role is in.

### group

The group that the filter is shared with. For a request, specify the `name` property for the group.

## Get dashboards

This method returns a list of dashboards owned by or shared with the user. The list may be filtered to include only favorite or owned dashboards, the method returns the following information:

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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}

		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(len(dashboards.Dashboards))

	return

}
```

## Create dashboard

Creates a dashboard.

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

	var sharePermissions []jira.SharePermissionScheme

	projectPermission := &jira.SharePermissionScheme{
		Type: "project",
		Project: &jira.SharePermissionProjectScheme{
			ID: "10000",
		},
	}

	groupPermission := &jira.SharePermissionScheme{
		Type:  "group",
		Group: &jira.SharePermissionGroupScheme{Name: "jira-administrators"},
	}

	sharePermissions = append(sharePermissions, *projectPermission, *groupPermission)

	dashboard, response, err := jiraCloud.Dashboard.Create(context.Background(), "Team Tracking", "", &sharePermissions)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}

```

## Search for dashboards

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of dashboards. This operation is similar to [Get dashboards](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-dashboards/#api-rest-api-3-dashboard-get) except that the results can be refined to include dashboards that have specific attributes. 

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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, dashboard := range dashboards.Values {
		log.Printf("Dashboard Name: %v", dashboard.Name)
		log.Printf("Dashboard ID: %v", dashboard.ID)
		log.Printf("Dashboard View: %v", dashboard.View)
	}

}

```

## Get dashboard

Returns a dashboard.

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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)

}

```

## Update dashboard

Updates a dashboard, replacing all the dashboard details with those provided.

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

	var sharePermissions []jira.SharePermissionScheme

	projectPermission := &jira.SharePermissionScheme{
		Type: "project",
		Project: &jira.SharePermissionProjectScheme{
			ID: "10000",
		},
	}

	groupPermission := &jira.SharePermissionScheme{
		Type:  "group",
		Group: &jira.SharePermissionGroupScheme{Name: "jira-administrators"},
	}

	sharePermissions = append(sharePermissions, *projectPermission, *groupPermission)

	dashboard, response, err := jiraCloud.Dashboard.Update(context.Background(), "10001", "Team Tracking #1", "", &sharePermissions)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}

```

## Delete dashboard

Deletes a dashboard.

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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Copy dashboard

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

	var sharePermissions []jira.SharePermissionScheme

	projectPermission := &jira.SharePermissionScheme{
		Type: "project",
		Project: &jira.SharePermissionProjectScheme{
			ID: "10000",
		},
	}

	groupPermission := &jira.SharePermissionScheme{
		Type:  "group",
		Group: &jira.SharePermissionGroupScheme{Name: "jira-administrators"},
	}

	sharePermissions = append(sharePermissions, *projectPermission, *groupPermission)

	dashboard, response, err := jiraCloud.Dashboard.Copy(context.Background(), "10001", "Team Tracking #2 copy", "", &sharePermissions)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}

```






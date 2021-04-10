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
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}

		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, dashboard := range dashboards.Dashboards {
		log.Println(dashboard.ID, dashboard.Name)
	}

	return

}

```

### Create dashboard

This method creates a dashboard on Jira Cloud.

{% hint style="warning" %}
This method is **Experimental**
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

### Search for dashboards

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

### Get dashboard

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

### Update dashboard

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

### Delete dashboard

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






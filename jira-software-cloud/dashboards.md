---
cover: ../.gitbook/assets/screenshot-2023-06-01-at-1.59.32-pm-1-1560x760.png
coverY: 30
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

# 📈 Dashboards

Jira dashboards are customizable, visual displays that provide an overview of project status and performance metrics in real-time. Dashboards are used to track progress and identify trends, enabling teams to make informed decisions and prioritize tasks.

![.gif created using the video https://www.youtube.com/watch?v=VswPTqLQzqA](../.gitbook/assets/atlassian-api-dashboard.gif)

Here are some key elements of Jira dashboards:

1. **Gadgets**: Gadgets are the building blocks of Jira dashboards. They are small, customizable widgets that display information in various formats, such as charts, lists, or calendars. Examples of gadgets include the Agile Sprint Burndown gadget, which shows the remaining work in a sprint, or the Pie Chart gadget, which displays the distribution of issues across a project.
2. **Filters**: Filters are used to specify which data should be displayed in a gadget. For example, you can create a filter that displays all open bugs assigned to a specific developer. Filters can be saved and reused across multiple gadgets.
3. **Layout**: Dashboards can be customized with different layouts to display multiple gadgets on a single page. Users can create multiple dashboards for different purposes, such as a team dashboard or a project-specific dashboard.
4. **Permissions**: Jira dashboards can be shared with team members or made public, depending on the level of access required. Permissions can be set at the individual gadget level or for the entire dashboard.

## Get all dashboards

`GET /rest/api/{2-3}/dashboard`

Returns a list of dashboards owned by or shared with the user. The list may be filtered to include only favorite or owned dashboards.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jiraCloud, err := v3.New(nil, host)
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
{% endcode %}

## Create dashboard

`POST /rest/api/{2-3}/dashboard`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

This method allows you to create a new dashboard in your Jira instance. The request body should contain a JSON object with the following properties:

* **name**: The name of the dashboard.
* **sharePermissions**: An array of objects representing the users and groups who have permission to view the dashboard.
* **gadget**: An array of objects representing the gadgets that should be displayed on the dashboard.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	jiraCloud, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	var payload = &models.DashboardPayloadScheme{
		Name:        "Team Tracking 4",
		Description: "description sample",
		SharePermissions: []*models.SharePermissionScheme{
			{
				Type: "project",
				Project: &models.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &models.GroupScheme{Name: "jira-administrators"},
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
{% endcode %}

## Search for dashboards

`GET /rest/api/{2-3}/dashboard/search`

Returns a _paginated_ list of dashboards. This operation is similar to **Get dashboards** except that the results can be refined to include dashboards that have specific attributes.&#x20;

For example, dashboards with a particular name. When multiple attributes are specified only filters matching all attributes are returned.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	searchOptions := models.DashboardSearchOptionsScheme{
		DashboardName:       "Bug",
		GroupPermissionName: "administrators",
		OrderBy:             "description",
		Expand:              []string{"description", "favourite", "sharePermissions"},
	}

	dashboards, response, err := jira.Dashboard.Search(context.Background(), &searchOptions, 0, 50)
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
{% endcode %}

## Get dashboard

`GET /rest/api/{2-3}/dashboard/{id}`

Returns a dashboard using the _dashboard-id_

{% hint style="warning" %}
&#x20;However, to get a dashboard, the dashboard must be shared with the user or the user must own it. Note, users with _**Administer Jira**_ [global permission](https://confluence.atlassian.com/x/x4dKLg) are considered owners of the System dashboard. The System dashboard is considered to be shared with all other users.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	dashboard, response, err := jira.Dashboard.Get(context.Background(), "10001")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}
```
{% endcode %}

## Update dashboard

`PUT /rest/api/{2-3}/dashboard/{id}`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Updates a dashboard, replacing all the dashboard details with those provided. **The dashboard to be updated must be owned by the user.**

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	var payload = &models.DashboardPayloadScheme{
		Name:        "Team Tracking #1111",
		Description: "",
		SharePermissions: []*models.SharePermissionScheme{
			{
				Type: "project",
				Project: &models.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &models.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	dashboard, response, err := jira.Dashboard.Update(context.Background(), "10001", payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Printf("Dashboard Name: %v", dashboard.Name)
	log.Printf("Dashboard ID: %v", dashboard.ID)
	log.Printf("Dashboard View: %v", dashboard.View)
}
```
{% endcode %}

## Delete dashboard

`DELETE /rest/api/{2-3}/dashboard/{id}`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Deletes a dashboard, t**he dashboard to be deleted must be owned by the user.**

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jiraCloud, err := v3.New(nil, host)
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
{% endcode %}

## Copy dashboard

`POST /rest/api/{2-3}/dashboard/{id}/copy`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Copies a dashboard. Any values provided in the `dashboard` parameter replace those in the copied dashboard.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	var payload = &models.DashboardPayloadScheme{
		Name:        "Team Tracking #2 copy",
		Description: "Description sample",
		SharePermissions: []*models.SharePermissionScheme{
			{
				Type: "project",
				Project: &models.ProjectScheme{
					ID: "10000",
				},
				Role:  nil,
				Group: nil,
			},
			{
				Type:  "group",
				Group: &models.GroupScheme{Name: "jira-administrators"},
			},
		},
	}

	dashboard, response, err := jira.Dashboard.Copy(context.Background(), "10001", payload)
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
{% endcode %}

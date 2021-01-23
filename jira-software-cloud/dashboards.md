---
description: >-
  This resource represents dashboards. Use it to obtain the details of
  dashboards as well as add and remove item properties from dashboards.
---

# 📈 Dashboards

## Get dashboards

This method returns a list of dashboards owned by or shared with the user. The list may be filtered to include only favorite or owned dashboards, the method returns the following information:

| variable | description |
| :--- | :--- |
| dashboards | A **DashboardsSchemeResult** struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

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
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	dashboards, response, err := atlassian.Dashboard.Gets(context.Background(), 0, 50, "my")

	if err != nil {

		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}

		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Total Dashboards", dashboards.Total)

	for _, dashboard := range dashboards.Dashboards {

		log.Println(dashboard.Name)
		log.Println(dashboard.ID)
		log.Println(dashboard.IsFavourite)
		log.Println(dashboard.Popularity)
		log.Println(dashboard.Self)
		log.Println(dashboard.View)

		for _, permission := range dashboard.SharePermissions {
			log.Println(permission.ID)
			log.Println(permission.Type)
		}

	}

}

```

## **DashboardsSchemeResult**

```go
type DashboardsSchemeResult struct {
	StartAt    int               `json:"startAt,omitempty"`
	MaxResults int               `json:"maxResults,omitempty"`
	Total      int               `json:"total,omitempty"`
	Dashboards []DashboardScheme `json:"dashboards,omitempty"`
}

type SharePermissionsScheme struct {
	ID   int    `json:"id,omitempty"`
	Type string `json:"type,omitempty"`
}

type DashboardScheme struct {
	ID               string                   `json:"id,omitempty"`
	IsFavourite      bool                     `json:"isFavourite,omitempty"`
	Name             string                   `json:"name,omitempty"`
	Popularity       int                      `json:"popularity,omitempty"`
	Self             string                   `json:"self,omitempty"`
	SharePermissions []SharePermissionsScheme `json:"sharePermissions,omitempty"`
	View             string                   `json:"view,omitempty"`
}
```








---
description: >-
  This resource represents dashboards. Use it to obtain the details of
  dashboards as well as add and remove item properties from dashboards.
---

# 📈 Dashboards

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

## Create dashboard

## Search for dashboards



## Get dashboard



## Update dashboard



## Delete dashboard



## Copy dashboard






---
description: >-
  This resource represents application roles. Use it to get details of an
  application role or all application roles.
---

# 🔬 Application Roles

{% page-ref page="application-roles.md" %}

{% tabs %}
{% tab title="Get Aplication Roles" %}
This method returns all application roles created on your Jira Cloud instance. 

{% hint style="info" %}
Jira Cloud API endpoint documentation [here.](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-application-roles/#api-rest-api-3-applicationrole-get)
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

	myCloudInstance, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	myCloudInstance.Auth.SetBasicAuth(mail, token)

	roles, response, err := myCloudInstance.Role.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, role := range *roles {

		log.Println(role.Key)
		log.Println(role.Name)
		log.Println(role.Groups)

		log.Println(role.DefaultGroups)
		log.Println(role.SelectedByDefault)
		log.Println(role.Defined)

		log.Println(role.NumberOfSeats)
		log.Println(role.RemainingSeats)
		log.Println(role.UserCount)

		log.Println(role.UserCountDescription)
		log.Println(role.HasUnlimitedSeats)
		log.Println(role.Platform)

	}
}

```
{% endtab %}

{% tab title="Get Application Role" %}
This method returns an existing application role using the **key** as a parameter.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here.](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-application-roles/#api-rest-api-3-applicationrole-key-get)
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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	role, response, err := atlassian.Role.Get(context.Background(), "jira-software")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(role.Key)
}

```
{% endtab %}
{% endtabs %}




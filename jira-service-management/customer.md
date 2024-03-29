---
cover: ../.gitbook/assets/leadership-principles_1120x545@2x-1560x760.png
coverY: 0
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

# 👨⚖ Customer

{% embed url="https://github.com/ctreminiom/go-atlassian/blob/main/pkg/infra/models/sm_customer.go" %}
SM Customer Models
{% endembed %}

## Create customer

`POST /rest/servicedeskapi/customer`

This method adds a customer to the Jira Service Management instance by passing a JSON file including an email address and display name. The display name does not need to be unique. The record's identifiers, `name` and `key`, are automatically generated from the request details.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		email       = "example12@gmail.com"
		displayName = "Example Customer 1"
	)

	newCustomer, response, err := atlassian.Customer.Create(context.Background(), email, displayName)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println("The new customer has been created!!")
	log.Println("-------------------------")
	log.Println(newCustomer.Name)
	log.Println(newCustomer.DisplayName)
	log.Println(newCustomer.AccountID)
	log.Println(newCustomer.EmailAddress)
	log.Println(newCustomer.Links)
	log.Println(newCustomer)
	log.Println("-------------------------")

}
```
{% endcode %}

## Add customers

`POST /rest/servicedeskapi/servicedesk/{serviceDeskId}/customer`

Adds one or more customers to a service desk. If any of the passed customers are associated with the service desk, no changes will be made for those customers and the resource returns a 204 success code.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		accountIDs = []string{"qm:7ee1b8dc-1ce3-467b-94cd-9bb2dcf083e2:3f06c44b-36e8-4394-9ff3-d679f854477c"}
	)

	response, err := atlassian.Customer.Add(context.Background(), 1, accountIDs)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.Bytes.String()))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get customers

`GET /rest/servicedeskapi/servicedesk/{serviceDeskId}/customer`

This method returns a list of the customers on a service desk.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")
	atlassian.Auth.SetExperimentalFlag()

	var (
		serviceDeskID = 1
		query         = ""
		start         = 0
		limit         = 50
	)

	customers, response, err := atlassian.Customer.Gets(context.Background(), serviceDeskID, query, start, limit)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, customer := range customers.Values {
		log.Println(customer)
	}

}
```
{% endcode %}

## Remove customers

`DELETE /rest/servicedeskapi/servicedesk/{serviceDeskId}/customer`

This method removes one or more customers from a service desk. The service desk must have closed access. If any of the passed customers are not associated with the service desk, no changes will be made for those customers and the resource returns a 204 success code.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")
	atlassian.Auth.SetExperimentalFlag()

	var (
		accountIDs = []string{"qm:7ee1b8dc-1ce3-467b-94cd-9bb2dcf083e2:3f06c44b-36e8-4394-9ff3-d679f854477c"}
	)

	response, err := atlassian.Customer.Remove(context.Background(), 1, accountIDs)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

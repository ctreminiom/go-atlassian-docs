---
cover: ../.gitbook/assets/growthgauntletcoverillo.jpg
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

# ℹ Info

{% embed url="https://github.com/ctreminiom/go-atlassian/blob/main/pkg/infra/models/sm_info.go" %}
SM Info Models
{% endembed %}

## Get info

`GET /rest/servicedeskapi/info`

This method retrieves information about the Jira Service Management instance such as software version, builds, and related links.

{% code fullWidth="true" %}
```go
ppackage main

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

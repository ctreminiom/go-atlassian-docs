# ℹ Info

{% embed url="https://github.com/ctreminiom/go-atlassian/blob/main/pkg/infra/models/sm_info.go" %}
SM Info Models
{% endembed %}

## Get info

This method retrieves information about the Jira Service Management instance such as software version, builds, and related links.

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

{% hint style="info" %}
🧚‍♀️ **Tips:** You can extract the following struct tags
{% endhint %}

```go
type InfoScheme struct {
	Version          string               `json:"version,omitempty"`
	PlatformVersion  string               `json:"platformVersion,omitempty"`
	BuildDate        *InfoBuildDataScheme `json:"buildDate,omitempty"`
	BuildChangeSet   string               `json:"buildChangeSet,omitempty"`
	IsLicensedForUse bool                 `json:"isLicensedForUse,omitempty"`
	Links            *InfoLinkScheme      `json:"_links,omitempty"`
}

type InfoBuildDataScheme struct {
	Iso8601     string `json:"iso8601,omitempty"`
	Jira        string `json:"jira,omitempty"`
	Friendly    string `json:"friendly,omitempty"`
	EpochMillis int64  `json:"epochMillis,omitempty"`
}
```

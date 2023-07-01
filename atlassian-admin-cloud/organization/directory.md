---
cover: >-
  ../../.gitbook/assets/csd-222-t1illustrationrefresh-5-signs-of-a-toxic-work-culture-v4a-1560x760.png
coverY: 157
---

# 👨👩👧👦 Directory

A user directory is a place where you store information about users and groups. User information includes the person's full name, username, email address and other personal information.&#x20;

Group information includes the name of the group, the users that belong to the group, and possibly groups that belong to other groups. More specifically with Atlassian Cloud Admin, each organization contains two types of directories:

* a local directory
* an identity provider directory

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

A local directory contains users you’re not managing in your identity provider. You invite these users or they sign up themselves. An identity provider directory contains users you sync or authenticate through your identity provider. You can add and move users between authentication policies in different directories.

### User’s last active dates

`GET /admin/v1/orgs/{orgId}/directory/users/{accountId}/last-active-dates`

Activity returns a user’s last active date for each product listed in Atlassian Administration.t

* Last activity data can be delayed by up to 4 hours
* Active is defined as viewing a product's page for a minimum of 2 seconds.&#x20;
* If the user has not accessed a product, the product\_access response field will be empty
* The added\_to\_org date field is available only to customers using the new user management experience.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	organizationID := "876468db-c88c-16jj-6a64-790d74914d9a"

	userActivity, response, err := cloudAdmin.Organization.Directory.Activity(context.Background(), organizationID, "5b86be50b8e3cb5895860d6d")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, product := range userActivity.Data.ProductAccess {
		fmt.Println(product.Name, product.LastActive)
	}

	fmt.Println("AddedToOrg", userActivity.Data.AddedToOrg)

}
```
{% endcode %}

### Remove user access

`DELETE /admin/v1/orgs/{orgId}/directory/users/{accountId}`

Remove removes user access to products listed in Atlassian Administration.&#x20;

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	organizationID := "876468db-c88c-16jj-6a64-790d74914d9a"

	response, err := cloudAdmin.Organization.Directory.Remove(context.Background(), organizationID, "712020:37ca4f5c-7f2e-4c5c-b62c-61562d9f62ab")
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

### Suspend user access

`POST /admin/v1/orgs/{orgId}/directory/users/{accountId}/suspend-access`

Suspend suspends user access to products listed in Atlassian Administration.

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	organizationID := "876468db-c88c-16jj-6a64-790d74914d9a"

	message, response, err := cloudAdmin.Organization.Directory.Suspend(context.Background(), organizationID, "712020:84a760fe-5cc9-418b-b139-614a9a891634")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	fmt.Println(message.Message)
}
```
{% endcode %}

### Restore user access

`POST /admin/v1/orgs/{orgId}/directory/users/{accountId}/restore-access`

{% hint style="warning" %}
This is an experimental endpoint.
{% endhint %}

Restore restores user access to products listed in Atlassian Administration.

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	organizationID := "876468db-c88c-16jj-6a64-790d74914d9a"

	message, response, err := cloudAdmin.Organization.Directory.Restore(context.Background(), organizationID, "712020:84a760fe-5cc9-418b-b139-614a9a891634")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	fmt.Println(message.Message)
}
```
{% endcode %}

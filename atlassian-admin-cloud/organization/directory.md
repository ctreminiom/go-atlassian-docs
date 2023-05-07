# 📖 Directory

## User’s last active dates

Activity returns a user’s last active date for each product listed in Atlassian Administration.t

* Last activity data can be delayed by up to 4 hours
* Active is defined as viewing a product's page for a minimum of 2 seconds.&#x20;
* If the user has not accessed a product, the product\_access response field will be empty
* The added\_to\_org date field is available only to customers using the new user management experience.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Status</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>Your organization is identified by a Unique ID. You get your organization ID and Organization API key simultaneously.</td><td></td><td></td></tr><tr><td><strong>accountId</strong></td><td>Unique ID of the user's account</td><td></td><td></td></tr></tbody></table>

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

## Remove user access

Remove removes user access to products listed in Atlassian Administration.&#x20;

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>Your organization is identified by a Unique ID. You get your organization ID and Organization API key simultaneously.</td><td></td></tr><tr><td><strong>accountId</strong> </td><td>Unique ID of the user's account that you are deleting</td><td></td></tr></tbody></table>

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

## Suspend user access

Suspend suspends user access to products listed in Atlassian Administration.

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>Your organization is identified by a Unique ID. You get your organization ID and Organization API key simultaneously.</td><td></td></tr><tr><td><strong>accountId</strong> </td><td>Unique ID of the user's account that you are deleting</td><td></td></tr></tbody></table>

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

## Restore user access

Restore restores user access to products listed in Atlassian Administration.

* The API is available for customers using the new user management experience only.
* Users with emails whose domain is claimed can still be found in Managed accounts in Directory.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>Your organization is identified by a Unique ID. You get your organization ID and Organization API key simultaneously.</td><td></td></tr><tr><td><strong>accountId</strong> </td><td>Unique ID of the user's account that you are deleting</td><td></td></tr></tbody></table>

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

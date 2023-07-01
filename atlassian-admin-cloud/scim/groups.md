---
cover: ../../.gitbook/assets/artboard-2@3x-1560x760.png
coverY: 0
---

# ⛹♂ Groups

## Get a group by ID

`GET /scim/directory/{directoryId}/Groups/{id}`

Get a group from a directory by group ID.

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

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		groupID     = "e18da5e4-ba2e-4039-9046-30000af6c0b7"
	)

	group, response, err := cloudAdmin.SCIM.Group.Get(context.Background(), directoryID, groupID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(group)

	fmt.Println(string(response.BodyAsBytes))
}
```
{% endcode %}

## Update a group by ID

`PUT /scim/directory/{directoryId}/Groups/{id}`

Update a group in a directory by group ID.

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

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID  = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		groupID      = "e18da5e4-ba2e-4039-9046-30000af6c0b7"
		newGroupName = "scim-jira-users-updated"
	)

	group, response, err := cloudAdmin.SCIM.Group.Update(context.Background(), directoryID, groupID, newGroupName)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(group)
}
```
{% endcode %}

## Delete a group by ID

`DELETE /scim/directory/{directoryId}/Groups/{id}`

Delete a group from a directory. An attempt to delete a non-existent group fails with a 404 (Resource Not found) error.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main()  {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		groupID     = "e18da5e4-ba2e-4039-9046-30000af6c0b7"
	)


	response, err := cloudAdmin.SCIM.Group.Delete(context.Background(), directoryID, groupID)
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
{% endcode %}

## Update a group by ID (PATCH)

`PATCH /scim/directory/{directoryId}/Groups/{id}`

Update a group's information in a directory by `groupId` via `PATCH`. You can use this API to manage group membership.

{% hint style="warning" %}
**Note:** Renaming groups after they've synced to your Atlassian organization isn't supported in this release of the User Provisioning API. To rename a group, create a new group with the desired name, update membership, and then delete the old group.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		groupID     = "e18da5e4-ba2e-4039-9046-30000af6c0b7"
		accountID   = "635cdb2f-e72c-4122-bfd3-3aa6c7f02f96"
	)

	payload := &models.SCIMGroupPathScheme{
		Schemas: []string{"urn:ietf:params:scim:api:messages:2.0:PatchOp"},
		Operations: []*models.SCIMGroupOperationScheme{
			{
				Op:   "add",
				Path: "members",
				Value: []*models.SCIMGroupOperationValueScheme{
					{
						Value:   accountID,
						Display: "Example Display Name",
					},
				},
			},
		},
	}

	group, response, err := cloudAdmin.SCIM.Group.Path(context.Background(), directoryID, groupID, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(group.ID, group.DisplayName)

	for _, member := range group.Members {
		log.Println(member)
	}
}
```
{% endcode %}

## Get groups

`GET /scim/directory/{directoryId}/Groups`

Get groups from a directory. Filtering is supported with a single exact match (`eq`) against the `displayName` attribute. Pagination is supported. Sorting is not supported.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/admin"
   "log"
   "os"
)

func main()  {

   //ATLASSIAN_ADMIN_TOKEN
   var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

   cloudAdmin, err := admin.New(nil)
   if err != nil {
      log.Fatal(err)
   }

   cloudAdmin.Auth.SetBearerToken(scimApiKey)
   cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

   var directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"

   groups, response, err := cloudAdmin.SCIM.Group.Gets(context.Background(), directoryID, "", 0, 50)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, group := range groups.Resources {
      log.Println(group)
   }

}
```
{% endcode %}

## Create a group

`POST /scim/directory/{directoryId}/Groups`

Create a group in a directory. An attempt to create a group with an existing name fails with a 409 (Conflict) error.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var scimApiKey = os.Getenv("ATLASSIAN_SCIM_API_KEY")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(scimApiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		directoryID  = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		newGroupName = "scim-jira-users"
	)

	group, response, err := cloudAdmin.SCIM.Group.Create(context.Background(), directoryID, newGroupName)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The group %v has been created and it contains the ID %v", group.DisplayName, group.ID)
}
</code></pre>

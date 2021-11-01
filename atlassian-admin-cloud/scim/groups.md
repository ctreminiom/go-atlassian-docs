# 🧩 Groups

## Get a group by ID

Get a group from a directory by group ID.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ScimGroupScheme struct {
	Schemas     []string                 `json:"schemas,omitempty"`
	ID          string                   `json:"id,omitempty"`
	ExternalID  string                   `json:"externalId,omitempty"`
	DisplayName string                   `json:"displayName,omitempty"`
	Members     []*ScimGroupMemberScheme `json:"members,omitempty"`
	Meta        *ScimMetadata            `json:"meta,omitempty"`
}

type ScimGroupMemberScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type ScimMetadata struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}
```

## Update a group by ID

Update a group in a directory by group ID.

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

## Delete a group by ID

Delete a group from a directory. An attempt to delete a non-existent group fails with a 404 (Resource Not found) error.

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

## Update a group by ID (PATCH)

Update a group's information in a directory by `groupId` via `PATCH`. You can use this API to manage group membership.

{% hint style="warning" %}
**Note:** Renaming groups after they've synced to your Atlassian organization isn't supported in this release of the User Provisioning API. To rename a group, create a new group with the desired name, update membership, and then delete the old group.
{% endhint %}

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
		directoryID = "bcdde508-ee40-4df2-89cc-d3f6292c5971"
		groupID     = "e18da5e4-ba2e-4039-9046-30000af6c0b7"
		accountID   = "635cdb2f-e72c-4122-bfd3-3aa6c7f02f96"
	)

	payload := &admin.SCIMGroupPathScheme{
		Schemas: []string{"urn:ietf:params:scim:api:messages:2.0:PatchOp"},
		Operations: []*admin.SCIMGroupOperationScheme{
			{
				Op:   "add",
				Path: "members",
				Value: []*admin.SCIMGroupOperationValueScheme{
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
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(group.ID, group.DisplayName)

	for _, member := range group.Members {
		log.Println(member)
	}

}

```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ScimGroupScheme struct {
	Schemas     []string                 `json:"schemas,omitempty"`
	ID          string                   `json:"id,omitempty"`
	ExternalID  string                   `json:"externalId,omitempty"`
	DisplayName string                   `json:"displayName,omitempty"`
	Members     []*ScimGroupMemberScheme `json:"members,omitempty"`
	Meta        *ScimMetadata            `json:"meta,omitempty"`
}

type ScimGroupMemberScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type ScimMetadata struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}
```

## Get groups

Get groups from a directory. Filtering is supported with a single exact match (`eq`) against the `displayName` attribute. Pagination is supported. Sorting is not supported.

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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ScimGroupPageScheme struct {
   Schemas      []string           `json:"schemas,omitempty"`
   TotalResults int                `json:"totalResults,omitempty"`
   StartIndex   int                `json:"startIndex,omitempty"`
   ItemsPerPage int                `json:"itemsPerPage,omitempty"`
   Resources    []*ScimGroupScheme `json:"Resources,omitempty"`
}
```

## Create a group

Create a group in a directory. An attempt to create a group with an existing name fails with a 409 (Conflict) error.

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
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ScimGroupScheme struct {
	Schemas     []string                 `json:"schemas,omitempty"`
	ID          string                   `json:"id,omitempty"`
	ExternalID  string                   `json:"externalId,omitempty"`
	DisplayName string                   `json:"displayName,omitempty"`
	Members     []*ScimGroupMemberScheme `json:"members,omitempty"`
	Meta        *ScimMetadata            `json:"meta,omitempty"`
}

type ScimGroupMemberScheme struct {
	Type    string `json:"type,omitempty"`
	Value   string `json:"value,omitempty"`
	Display string `json:"display,omitempty"`
	Ref     string `json:"$ref,omitempty"`
}

type ScimMetadata struct {
	ResourceType string `json:"resourceType,omitempty"`
	Location     string `json:"location,omitempty"`
	LastModified string `json:"lastModified,omitempty"`
	Created      string `json:"created,omitempty"`
}
```

# 👔 Policy

## Get list of policies

Returns information about org policies

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		policyType     = ""
		policyChunks   []*models.OrganizationPolicyPageScheme
		cursor         string
	)

	for {

		policies, response, err := cloudAdmin.Organization.Policy.Gets(context.Background(), organizationID, policyType, cursor)
		if err != nil {
			if response != nil {
				log.Println("Response HTTP Response", response.Bytes.String())
			}
			log.Fatal(err)
		}

		log.Println("Response HTTP Code", response.Code)
		log.Println("HTTP Endpoint Used", response.Endpoint)
		policyChunks = append(policyChunks, policies)

		if len(policies.Links.Next) == 0 {
			break
		}

		//extract the next cursor pagination
		nextAsURL, err := url.Parse(policies.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		cursor = nextAsURL.Query().Get("cursor")
	}

	for _, chunk := range policyChunks {

		for _, policy := range chunk.Data {

			log.Println(policy.ID, policy.Type, policy.Attributes.Status)
		}

	}

}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type OrganizationPolicyPageScheme struct {
   Data []*OrganizationPolicyData `json:"data"`
   Links *LinkPageModelScheme `json:"links"`
   Meta struct {
      Next     string `json:"next"`
      PageSize int    `json:"page_size"`
   } `json:"meta"`
}

type OrganizationPolicyData struct {
	ID         string                        `json:"id,omitempty"`
	Type       string                        `json:"type,omitempty"`
	Attributes *OrganizationPolicyAttributes `json:"attributes,omitempty"`
}

type OrganizationPolicyAttributes struct {
	Type      string                        `json:"type,omitempty"`
	Name      string                        `json:"name,omitempty"`
	Status    string                        `json:"status,omitempty"`
	Resources []*OrganizationPolicyResource `json:"resources,omitempty"`
	CreatedAt time.Time                     `json:"createdAt,omitempty"`
	UpdatedAt time.Time                     `json:"updatedAt,omitempty"`
}

type OrganizationPolicyResource struct {
	ID                string `json:"id,omitempty"`
	ApplicationStatus string `json:"applicationStatus,omitempty"`
}
```

## Create a policy

Create a policy for an org

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.OrganizationPolicyData{
		Type: "policy",
		Attributes: &models.OrganizationPolicyAttributes{
			Type:   "data-residency", //ip-allowlist
			Name:   "SCIMUserNameScheme of this Policy",
			Status: "enabled", //disabled
		},
	}

	var organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"

	newPolicy, response, err := cloudAdmin.Organization.Policy.Create(context.Background(), organizationID, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	policyAsJSONKeys, err := json.MarshalIndent(newPolicy, "", "  ")
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("MarshalIndent Struct keys output\n %s\n", string(policyAsJSONKeys))
	fmt.Println(payload)
}

```

## Get a policy by ID

Returns information about a single policy by ID

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
   var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

   cloudAdmin, err := admin.New(nil)
   if err != nil {
      log.Fatal(err)
   }

   cloudAdmin.Auth.SetBearerToken(apiKey)
   cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

   var (
      organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
      policyID       = "60f0f660-be3e-4d70-bd34-9c2858ec040f"
   )

   policy, response, err := cloudAdmin.Organization.Policy.Get(context.Background(), organizationID, policyID)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   log.Println(policy.Data.Type, policy.Data.ID, policy.Data.Attributes)

}
```

## Update a policy

Update a policy for an org

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	"github.com/ctreminiom/go-atlassian/admin"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	//ATLASSIAN_ADMIN_TOKEN
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.OrganizationPolicyData{
		Type: "policy",
		Attributes: &models.OrganizationPolicyAttributes{
			Status: "disabled", //disabled
		},
	}

	var (
		organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
		policyID       = "eaffa6f0-eb42-4b09-b2fb-0c7932187783"
	)

	policy, response, err := cloudAdmin.Organization.Policy.Update(context.Background(), organizationID, policyID, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	policyAsJSONKeys, err := json.MarshalIndent(policy, "", "  ")
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("MarshalIndent Struct keys output\n %s\n", string(policyAsJSONKeys))
}

```

## Delete a policy

Delete a policy for an org

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
   var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

   cloudAdmin, err := admin.New(nil)
   if err != nil {
      log.Fatal(err)
   }

   cloudAdmin.Auth.SetBearerToken(apiKey)
   cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

   var (
      organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
      policyID       = "eaffa6f0-eb42-4b09-b2fb-0c7932187783"
   )

   response, err := cloudAdmin.Organization.Policy.Delete(context.Background(), organizationID, policyID)
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

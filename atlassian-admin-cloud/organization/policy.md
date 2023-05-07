# 👔 Policy

## Overview

An authentication policy allows you to specify authentication settings for different sets of users and configurations in your organization. It verifies that users who access your Atlassian organization are who they claim to be.

<figure><img src="https://images.ctfassets.net/zsv3d0ugroxu/4J2YuoHUe8n4FF9OsG1Cvi/fef56ff544f4825cea71c466848df277/screenshot_AuthenticationPolicyCard" alt=""><figcaption></figcaption></figure>

1. **Default policy** – We automatically add new members to a default policy in your local or identity provider directory.
2. **Non-billable** - Create a non-billable policy when you don’t want to pay for certain users. You can only set a non-billable policy as the default policy in the local directory.
3. **Local directory** - Contains members you’re not managing in your identity provider. You invite them or they sign up themselves.
4. **Two-step verification** – Require a second step when logging in or make it optional for members.
5. **Third-party login** – Allow or block logins from third-party accounts.
6. **Password requirements** – Track minimum password strength and expiration.
7. **Idle session duration** – Track how long members can be inactive before logging them out.
8. **Members** – Shows the number of members in a policy. Add or move members from one policy to another policy.
9. **Single sign-on (SSO)** – Track when you enforce login to Atlassian through SAML or Google Workspace SSO. You can only enforce SSO in an identity provider directory.
10. **Identity provider directory** - Contains members you sync or authenticate through your identity provider. You can add and move members between authentication policies.

### Get list of policies

Returns information about org policies.

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select" data-multiple>Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>ID of the organization to query</td><td></td></tr><tr><td><strong>cursor</strong></td><td>Sets the starting point for the page of results to return.</td><td></td></tr><tr><td><strong>type</strong></td><td>Sets the type for the page of policies to return.</td><td></td></tr></tbody></table>

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

### Create a policy

Create a policy for an org

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select">Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>ID of the organization to query</td><td></td></tr><tr><td><strong>payload</strong></td><td>The new policy information</td><td></td></tr></tbody></table>

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

### Get a policy by ID

Returns information about a single policy by ID

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select" data-multiple>Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>ID of the organization to return</td><td></td></tr><tr><td><strong>policyId</strong> </td><td>ID of the policy to query</td><td></td></tr></tbody></table>

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

### Update a policy

Update a policy for an org

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select" data-multiple>Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>ID of the organization to update policy for</td><td></td></tr><tr><td><strong>policyId</strong> </td><td>ID of the policy to update</td><td></td></tr><tr><td><strong>payload</strong></td><td>A struct pointer with the policy attributes to edit.</td><td></td></tr></tbody></table>

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

### Delete a policy

Delete a policy for an org

<table><thead><tr><th>Param</th><th>Description</th><th data-type="select" data-multiple>Type</th></tr></thead><tbody><tr><td><strong>orgId</strong> </td><td>ID of the organization to update policy for</td><td></td></tr><tr><td><strong>policyId</strong> </td><td>ID of the policy to update</td><td></td></tr></tbody></table>

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

---
cover: ../../.gitbook/assets/image-20201119-174701-1560x760.png
coverY: -101
---

# 🔓 Token

API tokens allow a user to authenticate with cloud apps and bypass two-step verification and SSO, and retrieve data from the instance through REST APIs. Token controls allow admins to view and revoke the use of API tokens by their managed accounts.

## Get API tokens

`GET /users/{account_id}/manage/api-tokens`

Gets the API tokens owned by the specified user.

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
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var accountID = "606a7ff396e8d60068a5c652"

	tokens, response, err := cloudAdmin.User.Token.Gets(context.Background(), accountID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, token := range *tokens {
		log.Println(token.ID, token.Label, token.CreatedAt, token.LastAccess)
	}
}
```
{% endcode %}

## Delete API token

`DELETE /users/{account_id}/manage/api-tokens/{tokenId}`

Deletes a specifid API token by ID.

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
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")

	var (
		accountID = "5e5f6a63157ed50cd2b9eaca"
		tokenID   = "asp_A0T8NbvnwgyVkw3K"
	)

	response, err := cloudAdmin.User.Token.Delete(context.Background(), accountID, tokenID)
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

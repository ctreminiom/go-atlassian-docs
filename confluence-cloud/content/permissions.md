# 🛡 Permissions

## Check content permissions

Check if a user or a group can perform an operation to the specified content. The `operation` to check must be provided. The user’s account ID or the ID of the group can be provided in the `subject` to check permissions against a specified user or group. The following permission checks are done to make sure that the user or group has the proper access:

* site permissions
* space permissions
* content restrictions

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/http"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var (
		contentID = "76513281"
		payload = &models.CheckPermissionScheme{
			Subject:   &models.PermissionSubjectScheme{
				Type:       "user",
				Identifier: "5b86be50b8e3cb5895860d6d",
			},
			Operation: "read",
		}
	)

	check, response, err := instance.Content.Permission.Check(context.Background(), contentID, payload)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.API)
			}
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	log.Println(check.HasPermission)
}

```

---
cover: ../.gitbook/assets/strategic-planning-vs-cognitive-biases-1560x760.jpg
coverY: 0
---

# 📙 Workspaces

### Get workspaces

`GET /rest/servicedeskapi/assets/workspace`

The **Gets** method returns a list of Assets workspace IDs

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/assets"
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

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId
}

```
{% endcode %}

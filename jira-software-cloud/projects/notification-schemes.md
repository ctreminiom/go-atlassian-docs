# 📬 Notification Schemes

## Get Notification schemes

Search returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v2/intro/#pagination) list of [notification schemes](https://confluence.atlassian.com/x/8YdKLg) ordered by the display name.

<table><thead><tr><th>Param</th><th>Description</th><th>Type</th><th data-hidden>Reference</th></tr></thead><tbody><tr><td><strong>startAt</strong></td><td>The index of the first item to return in a page of results (page offset).</td><td><code>int</code></td><td></td></tr><tr><td><strong>maxResults</strong></td><td>The maximum number of items to return per page.</td><td><code>int</code></td><td></td></tr><tr><td><strong>id</strong></td><td>The list of notification schemes IDs to be filtered by.</td><td><code>[ ]string</code></td><td><code>*models.GetSpacesOptionSchemeV2.IDs</code></td></tr><tr><td><strong>projectId</strong></td><td>The list of projects IDs to be filtered by.</td><td><code>[ ]string</code></td><td></td></tr><tr><td><strong>onlyDefault</strong></td><td>When set to true, returns only the default notification scheme. If you provide project IDs not associated with the default, returns an empty page. The default value is false.</td><td><code>bool</code></td><td></td></tr><tr><td><strong>expand</strong></td><td><p></p><p>Use expand to include additional information in the response. This parameter accepts a comma-separated list. Expand options include:</p><ul><li><code>all</code> </li><li><code>field</code> </li><li><code>group</code> </li><li><code>notificationSchemeEvents</code> </li><li><code>projectRole</code> </li><li><code>user</code> </li></ul></td><td><code>[ ]string</code></td><td></td></tr></tbody></table>

```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	options := &models.NotificationSchemeSearchOptions{
		NotificationSchemeIDs: []string{"10000"},
		ProjectIDs:            nil,
		OnlyDefault:           false,
		Expand:                nil,
	}

	notificationSchemes, response, err := jira.NotificationScheme.Search(context.Background(), options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	for _, notificationScheme := range notificationSchemes.Values {
		fmt.Println(notificationScheme)
	}
}
```

## Create Notification scheme

Create creates a notification scheme with notifications. You can create up to 1000 notifications per request.

```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.NotificationSchemePayloadScheme{
		Description: "Notification Scheme #2",
		Name:        "Notification Scheme Description sample",
		Events: []*models.NotificationSchemePayloadEventScheme{
			{
				Event: &models.NotificationSchemeEventTypeScheme{
					ID: "1",
				},
				Notifications: []*models.NotificationSchemeEventNotificationScheme{
					{
						NotificationType: "Group",
						Parameter:        "jira-administrators",
					},
				},
			},
		},
	}

	notificationScheme, response, err := jira.NotificationScheme.Create(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	fmt.Println(notificationScheme.Id)
}
```

## Get Project notification schemes

Projects returns a paginated mapping of project that have notification scheme assigned. You can provide either one or multiple notification scheme IDs or project IDs to filter by.&#x20;

If you don't provide any, this will return a list of all mappings. Note that only company-managed (classic) projects are supported. This is because `team-managed` projects don't have a concept of a default notification scheme. The mappings are ordered by projectId.

| Param                    | Description                                                               | Type        |
| ------------------------ | ------------------------------------------------------------------------- | ----------- |
| **startAt**              | The index of the first item to return in a page of results (page offset). | `int`       |
| **maxResults**           | The maximum number of items to return per page.                           | `int`       |
| **notificationSchemeId** | The list of notifications scheme IDs to be filtered out                   | `[ ]string` |
| **projectId**            | The list of project IDs to be filtered out                                | `[ ]string` |

```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	mapping, response, err := jira.NotificationScheme.Projects(context.Background(), nil, nil, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	for _, element := range mapping.Values {
		fmt.Println(element.NotificationSchemeId, element.ProjectId)
	}
}
```

## Get Notification scheme

Get returns a notification scheme, including the list of events and the recipients who will receive notifications for those events.

| Param      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                          | Type        |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| **id**     | The ID of the notification scheme. Use [Get notification schemes paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-issue-notification-schemes/#api-rest-api-2-notificationscheme-get) to get a list of notification scheme IDs.                                                                                                                                                                                       | `int`       |
| **expand** | <p></p><p>Use <a href="https://developer.atlassian.com/cloud/jira/platform/rest/v2/intro/#expansion">expand</a> to include additional information in the response. This parameter accepts a comma-separated list. Expand options include:</p><ul><li><code>all</code> </li><li><code>field</code> </li><li><code>group</code> </li><li><code>notificationSchemeEvents</code> </li><li><code>projectRole</code> </li><li><code>user</code> </li></ul> | `[ ]string` |

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.NotificationSchemePayloadScheme{
		Description: "description update sample",
		Name:        "new notification scheme name",
	}

	response, err := jira.NotificationScheme.Update(context.Background(), "10002", payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

}
```

## Update Notification scheme

Update updates a notification scheme.

```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	notificationScheme, response, err := jira.NotificationScheme.Get(context.Background(), "10000", []string{"all"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	fmt.Println(notificationScheme.Self)
	fmt.Println(notificationScheme.Name)
	fmt.Println(notificationScheme.ID)
	fmt.Println(notificationScheme.Description)
	fmt.Println(notificationScheme.Scope)
	fmt.Println(notificationScheme.NotificationSchemeEvents)
}
```

## Delete Notification scheme

Delete deletes a notification scheme.

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	response, err := jira.NotificationScheme.Delete(context.Background(), "10002")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

}
```

## Append Notifications to scheme

Append adds notifications to a notification scheme. You can add up to 1000 notifications per request.

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.NotificationSchemeEventsPayloadScheme{
		NotificationSchemeEvents: []*models.NotificationSchemePayloadEventScheme{
			{
				Event: &models.NotificationSchemeEventTypeScheme{
					ID: "1",
				},
				Notifications: []*models.NotificationSchemeEventNotificationScheme{
					{
						NotificationType: "Group",
						Parameter:        "jira-administrators",
					},
				},
			},
		},
	}

	response, err := jira.NotificationScheme.Append(context.Background(), "10002", payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}
}
```

## Remove Notifications to scheme

Remove removes a notification from a notification scheme.

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	response, err := jira.NotificationScheme.Remove(context.Background(), "10002", "10000")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

}
```

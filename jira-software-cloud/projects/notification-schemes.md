---
cover: ../../.gitbook/assets/bb7f24ad-a935-415f-a11f-c1b2ffec7cb8-1560x760.jpeg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 📬 Notification Schemes

## Get Notification schemes

`GET /rest/api/{2-3}/notificationscheme`

Search returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v2/intro/#pagination) list of [notification schemes](https://confluence.atlassian.com/x/8YdKLg) ordered by the display name.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
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

	options := &#x26;models.NotificationSchemeSearchOptions{
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
</code></pre>

## Create Notification scheme

`POST /rest/api/{2-3}/notificationscheme`

Create creates a notification scheme with notifications. You can create up to 1000 notifications per request.

{% code fullWidth="true" %}
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
{% endcode %}

## Get Project notification schemes

`GET /rest/api/{2-3}/notificationscheme/project`

Projects returns a paginated mapping of project that have notification scheme assigned. You can provide either one or multiple notification scheme IDs or project IDs to filter by.&#x20;

If you don't provide any, this will return a list of all mappings. Note that only company-managed (classic) projects are supported. This is because `team-managed` projects don't have a concept of a default notification scheme. The mappings are ordered by projectId.

{% code fullWidth="true" %}
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
{% endcode %}

## Get Notification scheme

`GET /rest/api/{2-3}/notificationscheme/{id}`

Get returns a notification scheme, including the list of events and the recipients who will receive notifications for those events.

{% code fullWidth="true" %}
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
{% endcode %}

## Update Notification scheme

`PUT /rest/api/{2-3}/notificationscheme/{id}`

Update updates a notification scheme.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
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
</code></pre>

## Delete Notification scheme

`DELETE /rest/api/{2-3}/notificationscheme/{notificationSchemeId}`

Delete deletes a notification scheme.

{% code fullWidth="true" %}
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
{% endcode %}

## Append Notifications to scheme

`PUT /rest/api/{2-3}/notificationscheme/{id}/notification`

Append adds notifications to a notification scheme. You can add up to 1000 notifications per request.

{% code fullWidth="true" %}
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
{% endcode %}

## Remove Notifications to scheme

`DELETE /rest/api/{2-3}/notificationscheme/{notificationSchemeId}/notification/{notificationId}`

Remove removes a notification from a notification scheme.

{% code fullWidth="true" %}
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
{% endcode %}

---
cover: ../../.gitbook/assets/smt-3204-integrated-cicd-_blog_1120x545@2x-1560x760.png
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

# 💾 Webhooks

## List webhooks for a workspace

`GET /2.0/workspaces/{workspace}/hooks`

Returns a paginated list of webhooks installed on this workspace.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/bitbucket"
	"log"
	"os"
)

func main() {

	token := os.Getenv("BITBUCKET_TOKEN")

	instance, err := bitbucket.New(nil, "")
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBearerToken(token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	hooks, response, err := instance.Workspace.Hook.Gets(context.TODO(), "ctreminiom")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, hook := range hooks.Values {
		fmt.Println(hook.UUID, hook.Description, hook.Events, hook.URL)
	}
}
```
{% endcode %}

## Create webhook for a workspace

`POST /2.0/workspaces/{workspace}/hooks`

Creates a new webhook on the specified workspace. Workspace webhooks are fired for events from all repositories contained by that workspace.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/bitbucket"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	token := os.Getenv("BITBUCKET_TOKEN")

	instance, err := bitbucket.New(nil, "")
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBearerToken(token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.WebhookSubscriptionPayloadScheme{
		Description: "Webhook Description",
		Url:         "https://example.com/",
		Active:      true,
		Events: []string{
			"repo:push",
		},
	}

	hook, response, err := instance.Workspace.Hook.Create(context.TODO(), "ctreminiom", payload)
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(hook.UUID)
	fmt.Println(hook.Description)
	fmt.Println(hook.Events)
	fmt.Println(hook.URL)
}
```
{% endcode %}

## Update webhook for a workspace

`PUT /2.0/workspaces/{workspace}/hooks/{uid}`

Updates the specified webhook subscription.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/bitbucket"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	token := os.Getenv("BITBUCKET_TOKEN")

	instance, err := bitbucket.New(nil, "")
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBearerToken(token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.WebhookSubscriptionPayloadScheme{
		Description: "Webhook Description - UPDATED",
		Url:         "https://example1.com/",
		Active:      false,
		Events:      []string{"repo:push"},
	}

	hook, response, err := instance.Workspace.Hook.Update(context.TODO(), "ctreminiom", "{71cbd4cd-44fe-492d-88ac-049b8625cfae}", payload)
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(hook.UUID)
	fmt.Println(hook.Description)
	fmt.Println(hook.Events)
	fmt.Println(hook.URL)
}
```
{% endcode %}

## Get webhook for a workspace

`GET /2.0/workspaces/{workspace}/hooks/{uid}`

Returns the webhook with the specified id installed on the given workspace.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/bitbucket"
	"log"
	"os"
)

func main() {

	token := os.Getenv("BITBUCKET_TOKEN")

	instance, err := bitbucket.New(nil, "")
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBearerToken(token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	hook, response, err := instance.Workspace.Hook.Get(context.TODO(), "ctreminiom", "{71cbd4cd-44fe-492d-88ac-049b8625cfae}")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(hook.UUID)
	fmt.Println(hook.Description)
	fmt.Println(hook.Events)
	fmt.Println(hook.URL)
}
```
{% endcode %}

## Delete webhook for a workspace

`DELETE /2.0/workspaces/{workspace}/hooks/{uid}`

Deletes the specified webhook subscription from the given workspace.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/bitbucket"
	"log"
	"os"
)

func main() {

	token := os.Getenv("BITBUCKET_TOKEN")

	instance, err := bitbucket.New(nil, "")
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBearerToken(token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	response, err := instance.Workspace.Webhook.Delete(context.TODO(), "ctreminiom", "{71cbd4cd-44fe-492d-88ac-049b8625cfae}")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}
}
```
{% endcode %}

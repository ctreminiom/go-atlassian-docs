---
cover: >-
  ../../../.gitbook/assets/csd-222-t1illustrationrefresh-5-signs-of-a-toxic-work-culture-v4a-1560x760.png
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

# 🔃 Remote

This resource represents remote issue links, a way of linking Jira to information in other systems. Use it to get, create, update, and delete remote issue links either by ID or global ID. The global ID provides a way of accessing remote issue links using information about the item's remote system host and remote system identifier.

## Get remote issue links <a href="#get-remote-issue-links" id="get-remote-issue-links"></a>

`GET /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink`

Gets returns the remote issue links for an issue. When a remote issue link global ID is provided the record with that global ID is returned, otherwise all remote issue links are returned.

{% hint style="info" %}
Where a global ID includes reserved URL characters these must be escaped in the request.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	remoteLinks, response, err := instance.Issue.Link.Remote.Gets(context.Background(), "KP-23", "")
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	for _, remoteLink := range remoteLinks {
		fmt.Println(remoteLink.ID)
	}
}
```
{% endcode %}

## Create remote issue link <a href="#create-remote-issue-link" id="create-remote-issue-link"></a>

`POST /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink`

Create creates or updates a remote issue link for an issue.

If a `globalId` is provided and a remote issue link with that global ID is found it is updated. Any fields without values in the request are set to null. Otherwise, the remote issue link is created.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.RemoteLinkScheme{
		Application: &models.RemoteLinkApplicationScheme{
			Name: "My Acme Tracker",
			Type: "com.acme.tracker",
		},
		GlobalID: "system=http://www.mycompany.com/support&id=1",
		ID:       0,
		Object: &models.RemoteLinkObjectScheme{
			Icon: &models.RemoteLinkObjectLinkScheme{
				Title:    "Support Ticket",
				URL16X16: "http://www.mycompany.com/support/ticket.png",
			},
			Status: &models.RemoteLinkObjectStatusScheme{
				Icon: &models.RemoteLinkObjectLinkScheme{
					Link:     "http://www.mycompany.com/support?id=1&details=closed",
					Title:    "Case Closed",
					URL16X16: "http://www.mycompany.com/support/resolved.png",
				},
				Resolved: true,
			},
			Summary: "Customer support issue",
			Title:   "TSTSUP-111",
			URL:     "http://www.mycompany.com/support?id=1",
		},
		Relationship: "causes",
	}

	remoteLink, response, err := instance.Issue.Link.Remote.Create(context.Background(), "KP-23", payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Print(remoteLink.Self, remoteLink.ID)
}
```
{% endcode %}

## Delete remote issue link by ID <a href="#delete-remote-issue-link-by-id" id="delete-remote-issue-link-by-id"></a>

`DELETE /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink/{linkId}`

Delete deletes a remote issue link from an issue.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	response, err := instance.Issue.Link.Remote.DeleteById(context.Background(), "KP-23", "10001")
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}
}
```
{% endcode %}

## Get remote issue link <a href="#get-remote-issue-link" id="get-remote-issue-link"></a>

`GET /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink/{linkId}`

Get returns a remote issue link for an issue.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	remoteLink, response, err := instance.Issue.Link.Remote.Get(context.Background(), "KP-23", "10002")
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Println(remoteLink.ID, remoteLink.GlobalID)
}
```
{% endcode %}

## Update remote issue link

`PUT /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink/{linkId}`

Update updates a remote issue link for an issue.

{% hint style="info" %}
Fields without values in the request are set to null.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.RemoteLinkScheme{
		Application: &models.RemoteLinkApplicationScheme{
			Name: "My Acme Tracker",
			Type: "com.acme.tracker",
		},
		GlobalID: "system=http://www.mycompany.com/support&id=1",
		Object: &models.RemoteLinkObjectScheme{
			Icon: &models.RemoteLinkObjectLinkScheme{
				Title:    "Support Ticket",
				URL16X16: "http://www.mycompany.com/support/ticket.png",
			},
			Status: &models.RemoteLinkObjectStatusScheme{
				Icon: &models.RemoteLinkObjectLinkScheme{
					Link:     "http://www.mycompany.com/support?id=1&details=closed",
					Title:    "Case Closed",
					URL16X16: "http://www.mycompany.com/support/resolved.png",
				},
				Resolved: true,
			},
			Summary: "Customer support issue",
			Title:   "TSTSUP-111",
			URL:     "http://www.mycompany.com/support?id=1",
		},
		Relationship: "causes",
	}

	response, err := instance.Issue.Link.Remote.Update(context.Background(), "KP-23", "10001", payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}
}
```
{% endcode %}

## Delete remote issue link by Global ID

`DELETE /rest/api/{2-3}/issue/{issueIdOrKey}/remotelink`

DeleteByGlobalId deletes the remote issue link from the issue using the link's global ID where the global ID includes reserved URL characters these must be escaped in the request.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	response, err := instance.Issue.Link.Remote.DeleteByGlobalId(context.Background(), "KP-23", "system=http://www.mycompany.com/support&id=1")
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}
		
		log.Fatal(err)
	}
}
```
{% endcode %}

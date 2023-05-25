# 🔃 Remote

This resource represents remote issue links, a way of linking Jira to information in other systems. Use it to get, create, update, and delete remote issue links either by ID or global ID. The global ID provides a way of accessing remote issue links using information about the item's remote system host and remote system identifier.

## Get remote issue links <a href="#get-remote-issue-links" id="get-remote-issue-links"></a>

Gets returns the remote issue links for an issue. When a remote issue link global ID is provided the record with that global ID is returned, otherwise all remote issue links are returned.

{% hint style="info" %}
Where a global ID includes reserved URL characters these must be escaped in the request.
{% endhint %}

<table><thead><tr><th>Param</th><th width="252">Description</th><th width="180">Type</th><th data-type="select">Required</th></tr></thead><tbody><tr><td><strong>issueIdOrKey</strong></td><td>The ID or key of the issue.</td><td>String</td><td></td></tr><tr><td><strong>globalId</strong></td><td>The global ID of the remote issue link.</td><td>String</td><td></td></tr></tbody></table>

```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

## Create remote issue link <a href="#create-remote-issue-link" id="create-remote-issue-link"></a>

Create creates or updates a remote issue link for an issue.

If a `globalId` is provided and a remote issue link with that global ID is found it is updated. Any fields without values in the request are set to null. Otherwise, the remote issue link is created.

<table><thead><tr><th>Param</th><th width="292.3333333333333">Type</th><th data-type="select">Required</th></tr></thead><tbody><tr><td><strong>issueIdOrKey</strong> </td><td>string</td><td></td></tr><tr><td><strong>payload</strong></td><td>*models.RemoteLinkScheme</td><td></td></tr></tbody></table>

```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

## Delete remote issue link by ID <a href="#delete-remote-issue-link-by-id" id="delete-remote-issue-link-by-id"></a>

Delete deletes a remote issue link from an issue.

<table><thead><tr><th width="166">Param</th><th width="241">Description</th><th width="92">Type</th><th data-type="select">Required</th></tr></thead><tbody><tr><td><strong>issueIdOrKey</strong> </td><td>The ID or key of the issue.</td><td>string</td><td></td></tr><tr><td><strong>linkId</strong> </td><td>The ID of a remote issue link.</td><td>string</td><td></td></tr></tbody></table>

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

## Get remote issue link <a href="#get-remote-issue-link" id="get-remote-issue-link"></a>

Get returns a remote issue link for an issue.

<table><thead><tr><th width="156.33333333333331">Param</th><th width="270">Description</th><th width="85">Type</th><th data-type="select">Required</th></tr></thead><tbody><tr><td><strong>issueIdOrKey</strong> </td><td>The ID or key of the issue.</td><td>string</td><td></td></tr><tr><td><strong>linkId</strong> </td><td>The ID of the remote issue link.</td><td>string</td><td></td></tr></tbody></table>

```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

## Update remote issue link

Update updates a remote issue link for an issue.

{% hint style="info" %}
Fields without values in the request are set to null.
{% endhint %}

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

## Delete remote issue link by Global ID

DeleteByGlobalId deletes the remote issue link from the issue using the link's global ID where the global ID includes reserved URL characters these must be escaped in the request.

```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

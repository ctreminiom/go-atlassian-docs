---
cover: >-
  ../../.gitbook/assets/smt-2370_crowd3.5_licensing_bloghero_1120x545@2x@2x-1560x760.png
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

# 📰 Workspace

A workspace is where you will create repositories, collaborate on your code, and organize different streams of work in your Bitbucket Cloud account.

You can change your workspace ID (aka workspace slug) in Bitbucket Cloud; however, this will change the URL for **all** the repositories, snippets, and static websites for that workspace.

Below is how the URLs will be formatted for any repositories you create in your workspace:

* www.bitbucket.org/<mark style="color:blue;">**\<workspace ID>**</mark>/<mark style="color:purple;">**\<repo name>**</mark>

{% hint style="info" %}
Workspaces replace the use of teams and users in API calls.
{% endhint %}

## Get a workspace

`GET /2.0/workspaces/{workspace}`

Returns the requested workspace.

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

	workspace, response, err := instance.Workspace.Get(context.Background(), "ctreminiom")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(workspace.Type)
	fmt.Println(workspace.Links)
	fmt.Println(workspace.Uuid)
	fmt.Println(workspace.Name)
	fmt.Println(workspace.Name)
	fmt.Println(workspace.Slug)
	fmt.Println(workspace.IsPrivate)
	fmt.Println(workspace.CreatedOn)
	fmt.Println(workspace.UpdatedOn)
}
```
{% endcode %}

## Get members in a workspace

`GET /2.0/workspaces/{workspace}/members`

Returns all members of the requested workspace.

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

	members, response, err := instance.Workspace.Members(context.Background(), "ctreminiom")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, member := range members.Values {

		fmt.Println(member.User.Uuid)
		fmt.Println(member.Workspace.Name)
		fmt.Println(member.Links.Self)
	}
}
```
{% endcode %}

## Get member in a workspace

`GET /2.0/workspaces/{workspace}/members/{member}`

Returns the workspace membership, which includes a `User` object for the member and a `Workspace` object for the requested workspace.

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

	member, response, err := instance.Workspace.Membership(context.Background(), "ctreminiom", "{55adaf95-93a2-49b6-950a-b4cb33c4ac1f}")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(member.User.DisplayName)
	fmt.Println(member.Workspace.Name)
	fmt.Println(member.Links.Self)
}
```
{% endcode %}

## Get projects in a workspace

`GET /2.0/workspaces/{workspace}/projects`

Returns the list of projects in this workspace.

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

	projects, response, err := instance.Workspace.Projects(context.Background(), "ctreminiom")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, project := range projects.Values {
		fmt.Println(project.Name, project.Uuid, project.IsPrivate, project.Key)
	}
}

```
{% endcode %}

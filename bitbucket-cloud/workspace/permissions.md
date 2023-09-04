---
cover: ../../.gitbook/assets/csd-6414_blog-hero-1160x652@2x-1-1560x760.png
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

# 🛡 Permissions

## Get user permissions in a workspace

`GET /2.0/workspaces/{workspace}/permissions`

Returns the list of members in a workspace and their permission levels. Permission can be:

* `owner`
* `collaborator`
* `member`

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

	users, response, err := instance.Workspace.Permission.Members(context.TODO(), "ctreminiom", "permission=\"owner\"")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, user := range users.Values {
		fmt.Println(user)
	}
}
```
{% endcode %}

## Gets all repository permissions in a workspace

`GET /2.0/workspaces/{workspace}/permissions/repositories`

Returns an object for each repository permission for all of a workspace's repositories.

Permissions returned are effective permissions: the highest level of permission the user has. This does not distinguish between direct and indirect (group) privileges.

Only users with admin permission for the team may access this resource. The permissions can be:

* `admin`
* `write`
* `read`

Results may be further [filtered or sorted](https://developer.atlassian.com/cloud/bitbucket/rest/intro/#filtering) by repository, user, or permission by adding the following query string parameters:

* `q=repository.name="geordi"` or `q=permission>"read"`
* `sort=user.display_name`

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

	repositories, response, err := instance.Workspace.Permission.Repositories(context.TODO(), "ctreminiom", "permission=\"admin\"", "")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, repo := range repositories.Values {
		fmt.Println(repo.Type, repo.Repository.Name, repo.Repository.Uuid, repo.Permission)
	}
}

```
{% endcode %}

## Get repository permission in a workspace

`GET /2.0/workspaces/{workspace}/permissions/repositories/{repo_slug}`

Returns an object for the repository permission of each user in the requested repository.

Permissions returned are effective permissions: the highest level of permission the user has. This does not distinguish between direct and indirect (group) privileges.

Only users with admin permission for the repository may access this resource, the permissions can be:

* `admin`
* `write`
* `read`

Results may be further [filtered or sorted](https://developer.atlassian.com/cloud/bitbucket/rest/intro/#filtering) by user, or permission by adding the following query string parameters:

* `q=permission>"read"`
* `sort=user.display_name`

Note that the query parameter values need to be URL escaped so that `=` would become `%3D`.

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

	repositories, response, err := instance.Workspace.Permission.Repository(context.TODO(), "ctreminiom", "microservice-a", "permission=\"admin\"", "")
	if err != nil {

		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, repo := range repositories.Values {
		fmt.Println(repo.Type, repo.Repository.Name, repo.Repository.Uuid, repo.Permission)
	}
}

```
{% endcode %}

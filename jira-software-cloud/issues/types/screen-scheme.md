---
description: This resource represents issue type screen schemes.
cover: ../../../.gitbook/assets/2240x1090-1-1560x760.jpg
coverY: 0
---

# 🛅 Screen Scheme

## Get issue type screen schemes

`GET /rest/api/{2-3}/issuetypescreenscheme`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type screen schemes. Only issue type screen schemes used in classic projects are returned.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	screenSchemes, response, err := atlassian.Issue.Type.ScreenScheme.Gets(context.Background(), []int{10001, 10002}, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(screenSchemes)

	for _, screenScheme := range screenSchemes.Values {
		log.Println(screenScheme)
	}
}
```
{% endcode %}

## Create issue type screen scheme

`POST /rest/api/{2-3}/issuetypescreenscheme`

Creates an issue-type screen scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := models.IssueTypeScreenSchemePayloadScheme{
		Name: "FX 2 Issue Type Screen Scheme",
		IssueTypeMappings: []*models.IssueTypeScreenSchemeMappingPayloadScheme{
			{
				IssueTypeID:    "default",
				ScreenSchemeID: "10000",
			},
			{
				IssueTypeID:    "10004", // Bug
				ScreenSchemeID: "10002",
			},
		},
	}

	issueTypeScreenSchemeID, response, err := atlassian.Issue.Type.ScreenScheme.Create(context.Background(), &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(issueTypeScreenSchemeID)
}
```
{% endcode %}

## Get issue type screen scheme items

`GET /rest/api/{2-3}/issuetypescreenscheme/mapping`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue-type screen scheme items. Only issue type screen schemes used in classic projects are returned.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	mapping, response, err := atlassian.Issue.Type.ScreenScheme.Mapping(context.Background(), nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range mapping.Values {
		log.Println(item)
	}

}
```
{% endcode %}

## Assign issue type screen scheme to project

`PUT /rest/api/{2-3}/issuetypescreenscheme/project`

Assigns an issue-type screen scheme to a project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10002"
		projectID               = "10002"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Assign(context.Background(), issueTypeScreenSchemeID, projectID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get issue type screen schemes for projects

`GET /rest/api/{2-3}/issuetypescreenscheme/project`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of issue type screen schemes and, for each issue type screen scheme, a list of the projects that use it. Only issue type screen schemes used in classic projects are returned.

{% hint style="warning" %}
CREATE THE CODE SAMPLE
{% endhint %}

## Update issue type screen scheme

`PUT /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSchemeId}`

Updates an issue type screen scheme.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10002"
		name                    = "FX Issue Type Screen Scheme - UPDATED"
		description             = ""
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Update(context.Background(), issueTypeScreenSchemeID, name, description)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
</code></pre>

## Delete issue type screen scheme

`DELETE /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSchemeId}`

Deletes an issue type screen scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10004"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Delete(context.Background(), issueTypeScreenSchemeID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Append mappings to issue type screen scheme

`PUT /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSchemeId}/mapping`

Appends issue type to screen scheme mappings to an issue type screen scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var payload = &models.IssueTypeScreenSchemePayloadScheme{
		IssueTypeMappings: []*models.IssueTypeScreenSchemeMappingPayloadScheme{
			{
				IssueTypeID:    "10000", // Epic Issue Type
				ScreenSchemeID: "10002",
			},
			{
				IssueTypeID:    "10002", // Task Issue Type
				ScreenSchemeID: "10002",
			},
		},
	}

	var issueTypeScreenSchemeID = "10005"

	response, err := atlassian.Issue.Type.ScreenScheme.Append(context.Background(), issueTypeScreenSchemeID, payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Update issue type screen scheme default screen scheme

`PUT /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSc}/mapping/default`

Updates the default screen scheme of an issue-type screen scheme. The default screen scheme is used for all unmapped issue types.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10005"
		screenSchemeID          = "10002"
	)

	response, err := atlassian.Issue.Type.ScreenScheme.UpdateDefault(context.Background(), issueTypeScreenSchemeID, screenSchemeID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Remove mappings from issue type screen scheme

`POST /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSc}/mapping/remove`

Removes issue type to screen scheme mappings from an issue type screen scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		issueTypeScreenSchemeID = "10005"
		issueTypesIDs           = []string{"10002", "10000"}
	)

	response, err := atlassian.Issue.Type.ScreenScheme.Remove(context.Background(), issueTypeScreenSchemeID, issueTypesIDs)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get issue type screen scheme projects

`GET /rest/api/{2-3}/issuetypescreenscheme/{issueTypeScreenSchemeId}/project`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of projects associated with an issue-type screen scheme. Only company-managed projects associated with an issue-type screen scheme are returned.

{% hint style="warning" %}
CREATE THE CODE SAMPLE
{% endhint %}

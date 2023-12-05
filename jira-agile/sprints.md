---
cover: ../.gitbook/assets/2240x1090-1-1560x760.jpg
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

# 🗓 Sprints

## Create sprint <a href="#create-print" id="create-print"></a>

`POST /rest/agile/1.0/sprint`

Creates a future sprint. Sprint name and origin board id are required. Start date, end date, and goal are optional.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
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

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.SprintPayloadScheme{
		Name:          "Sprint XX",
		StartDate:     "2015-04-11T15:22:00.000+10:00",
		EndDate:       "2015-04-20T01:22:00.000+10:00",
		OriginBoardID: 4,
		Goal:          "Sprint XX goal",
	}

	sprint, response, err := atlassian.Sprint.Create(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Bytes.String())
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(sprint)
}
```
{% endcode %}

## Get sprint

`GET /rest/agile/1.0/sprint/{sprintId}`

Returns the sprint for a given sprint ID. The sprint will only be returned if the user can view the board that the sprint was created on, or view at least one of the issues in the sprint.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	sprint, response, err := atlassian.Sprint.Get(context.Background(), 3)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(sprint)
}
```
{% endcode %}

## Update sprint

`PUT /rest/agile/1.0/sprint/{sprintId}`

Performs a full update of a sprint. A full update means that the result will be exactly the same as the request body. Any fields not present in the request JSON will be set to null.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
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

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.SprintPayloadScheme{
		Name:      "Sprint XX-Updated",
		Goal:      "Sprint XX goal-Updated",
		State:     "Active",
		StartDate: "2020-04-11T15:22:00.000+10:00",
		EndDate:   "2021-04-20T01:22:00.000+10:00",
	}

	sprint, response, err := atlassian.Sprint.Update(context.Background(), 2, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(sprint.Name, sprint.Goal)
}
```
{% endcode %}

## Partially update sprint

`POST /rest/agile/1.0/sprint/{sprintId}`

Performs a partial update of a sprint. A partial update means that fields not present in the request JSON will not be updated.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
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

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	payload := &#x26;models.SprintPayloadScheme{
		Name: "Sprint XX-Patched",
	}

	sprint, response, err := atlassian.Sprint.Path(context.Background(), 2, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(sprint.Name, sprint.Goal)
}
</code></pre>

## Get issues for sprint <a href="#get-issues-for-sprint" id="get-issues-for-sprint"></a>

`GET /rest/agile/1.0/sprint/{sprintId}/issue`

Returns all issues in a sprint, for a given sprint ID. This only includes issues that the user has permission to view. By default, the returned issues are ordered by rank.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
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

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	options := &models.IssueOptionScheme{
		JQL:           "",
		Fields:        nil,
		Expand:        nil,
		ValidateQuery: false,
	}

	issues, response, err := atlassian.Sprint.Issues(context.Background(), 2, options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issues.Total)
	log.Println(issues.Expand)
	log.Println(issues.MaxResults)
	log.Println(issues.StartAt)

	for _, issue := range issues.Issues {
		log.Println(issue.Key, issue.ID)
	}
}
```
{% endcode %}

## Start sprint

`GET /rest/agile/1.0/sprint/{sprintId}/issue`

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	response, err := atlassian.Sprint.Start(context.Background(), 3)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

}
</code></pre>

## Close Sprint

`GET /rest/agile/1.0/sprint/{sprintId}/issue`

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	response, err := atlassian.Sprint.Start(context.Background(), 3)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

}
```
{% endcode %}

## Delete Sprint

`DELETE /rest/agile/1.0/sprint/{sprintId}`

Delete deletes a sprint. Once a sprint is deleted, all open issues in the sprint will be moved to the backlog.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	response, err := atlassian.Sprint.Delete(context.Background(), 3)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

}
```
{% endcode %}

## Move Issues To Sprint

`POST /rest/agile/1.0/sprint/{sprintId}/issue`

Move moves issues to a sprint, for a given sprint ID. Issues can only be moved to open or active sprints. The maximum number of issues that can be moved in one operation is 50.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/agile"
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

	atlassian, err := agile.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	options := &models.SprintMovePayloadScheme{
		Issues:            nil,
		RankBeforeIssue:   "",
		RankAfterIssue:    "",
		RankCustomFieldId: 0,
	}

	response, err := atlassian.Sprint.Move(context.Background(), 3, options)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

}
```
{% endcode %}

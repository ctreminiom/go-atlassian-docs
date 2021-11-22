---
description: >-
  A sprint is a short, time-boxed period when a scrum team works to complete a
  set amount of work. Sprints are at the very heart of scrum and agile
  methodologies, and getting sprints right will help you
---

# 🗓 Sprints

### Create sprint <a href="create-print" id="create-print"></a>

Creates a future sprint. Sprint name and origin board id are required. Start date, end date, and goal are optional.

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

### Get sprint

Returns the sprint for a given sprint ID. The sprint will only be returned if the user can view the board that the sprint was created on, or view at least one of the issues in the sprint.

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

### Update sprint

Performs a full update of a sprint. A full update means that the result will be exactly the same as the request body. Any fields not present in the request JSON will be set to null.

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

### Partially update sprint

Performs a partial update of a sprint. A partial update means that fields not present in the request JSON will not be updated.

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
		Name: "Sprint XX-Patched",
	}

	sprint, response, err := atlassian.Sprint.Path(context.Background(), 2, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response",response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(sprint.Name, sprint.Goal)
}
```

### Get issues for sprint <a href="api-agile-1-0-sprint-sprintid-issue-get" id="api-agile-1-0-sprint-sprintid-issue-get"></a>

Returns all issues in a sprint, for a given sprint ID. This only includes issues that the user has permission to view. By default, the returned issues are ordered by rank.

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

### Start sprint

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

### Close Sprint

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

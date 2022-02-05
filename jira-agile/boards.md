---
description: >-
  A board displays issues from one or more projects, giving you a flexible way
  of viewing, managing, and reporting on work in progress.
---

# 📉 Boards

### Get issues for backlog

Returns all issues from the board's backlog, for the given board ID.&#x20;

* This only includes issues that the user has permission to view.&#x20;
* The backlog contains incomplete issues that are not assigned to any future or active sprint.&#x20;

{% hint style="info" %}
Note, if the user does not have permission to view the board, no issues will be returned at all.&#x20;
{% endhint %}

Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% embed url="https://developer.atlassian.com/cloud/jira/software/rest/api-group-board#api-agile-1-0-board-boardid-backlog-get" %}
Official Documentation
{% endembed %}

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

	var (
		options = &models.IssueOptionScheme{
			JQL:           "project = KP",
			ValidateQuery: true,
			Fields:        []string{"status", "issuetype", "summary"},
			Expand:        []string{"changelog"},
		}

		boardID   = 4
		startAt   = 0
		maxResult = 50
	)

	issues, response, err := atlassian.Board.Backlog(context.Background(), boardID, startAt, maxResult, options)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.Bytes.Bytes()))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issues.Issues {
		log.Println(issue.Key)
	}
}
```

### Get configuration

Get the board configuration.&#x20;

{% embed url="https://developer.atlassian.com/cloud/jira/software/rest/api-group-board#api-agile-1-0-board-boardid-configuration-get" %}
Official Documentation
{% endembed %}

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

	boardConfig, response, err := atlassian.Board.Configuration(context.Background(), 4)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(boardConfig)
}
```

### Create board

Creates a new board. Board name, type ,and filter ID is required

{% embed url="https://developer.atlassian.com/cloud/jira/software/rest/api-group-board#api-agile-1-0-board-post" %}
Official Documentation
{% endembed %}

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

	newBoard := &models.BoardPayloadScheme{
		Name:     "DUMMY Board Name",
		Type:     "scrum", //scrum or kanban
		FilterID: 10016,

		// Omit the Location if you want to the board to yourself (location)
		Location: &models.BoardPayloadLocationScheme{
			ProjectKeyOrID: "KP",
			Type:           "project",
		},
	}

	board, response, err := atlassian.Board.Create(context.Background(), newBoard)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(board)
}

```

### Get epics

Returns all epics from the board, for the given board ID. This only includes epics that the user has permission to view. Note, if the user does not have permission to view the board, no epics will be returned at all.

{% embed url="https://gist.github.com/ctreminiom/65d8a79aa693002bf50ae0aa75034e02" %}

### Get board by filter id

Returns any boards which use the provided filter id. This method can be executed by users without a valid software license in order to find which boards are using a particular filter.

{% embed url="https://gist.github.com/ctreminiom/e13dc5201c713f4f415a0619dc9b05be" %}

### Get board

Returns the board for the given board ID. This board will only be returned if the user has permission to view it. Admins without the view permission will see the board as a private one, so will see only a subset of the board's data (board location for instance).

{% embed url="https://gist.github.com/ctreminiom/fcca71eace448e6415d55a012adeafc3" %}

### Get issues for board

Returns all issues from a board, for a given board ID. This only includes issues that the user has permission to view. An issue belongs to the board if its status is mapped to the board's column. Epic issues do not belongs to the scrum boards. Note, if the user does not have permission to view the board, no issues will be returned at all. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.\


{% embed url="https://gist.github.com/ctreminiom/c703131c9ef0c0524db183fe268e97ae" %}

### Get board issues for epic

Returns all issues that belong to an epic on the board, for the given epic ID and the board ID. This only includes issues that the user has permission to view. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% embed url="https://gist.github.com/ctreminiom/7ee4c06dee1d532fd64900ad8089af38" %}

### Get board issues for sprint

Get all issues you have access to that belong to the sprint from the board. Issue returned from this resource contains additional fields like: sprint, closedSprints, flagged and epic. Issues are returned ordered by rank. JQL order has higher priority than default rank.

{% embed url="https://gist.github.com/ctreminiom/5676d132f2b8c4eda85159585fb656ad" %}

### Get issues without epic for board

Returns all issues that do not belong to any epic on a board, for a given board ID. This only includes issues that the user has permission to view. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% embed url="https://gist.github.com/ctreminiom/f1ba96d7eda69d5f1ab17ee229002c35" %}

### Move issues to backlog for board

Move issues to the backlog of a particular board (if they are already on that board). This operation is equivalent to remove future and active sprints from a given set of issues if the board has sprints If the board does not have sprints this will put the issues back into the backlog from the board. At most 50 issues may be moved at once.

{% embed url="https://gist.github.com/ctreminiom/46f31c0b73e45d662808cadc45560ac8" %}

### Get projects

Returns all projects that are associated with the board, for the given board ID. If the user does not have permission to view the board, no projects will be returned at all. Returned projects are ordered by the name.

{% embed url="https://gist.github.com/ctreminiom/0a9dab82f395e0b40e807fa1d3b11c52" %}

### Get all sprints

Returns all sprints from a board, for a given board ID. This only includes sprints that the user has permission to view.

{% embed url="https://gist.github.com/ctreminiom/366682bb009e8c0f8119a50cef6eb831" %}

### Get all versions

Returns all versions from a board, for a given board ID. This only includes versions that the user has permission to view. Note, if the user does not have permission to view the board, no versions will be returned at all. Returned versions are ordered by the name of the project from which they belong and then by sequence defined by user.

{% embed url="https://gist.github.com/ctreminiom/ed2e5a1aed651991c3ba36895433d814" %}

---
description: >-
  A board displays issues from one or more projects, giving you a flexible way
  of viewing, managing, and reporting on work in progress.
cover: ../../.gitbook/assets/team_personality_tests_1120x545@2x-1560x760.jpeg
coverY: 0
---

# 📉 Boards

## Get issues for backlog

`GET /rest/agile/1.0/board/{boardId}/backlog`

Returns all issues from the board's backlog, for the given board ID.&#x20;

* This only includes issues that the user has permission to view.&#x20;
* The backlog contains incomplete issues that are not assigned to any future or active sprint.&#x20;

{% hint style="info" %}
Note, if the user does not have permission to view the board, no issues will be returned at all.&#x20;
{% endhint %}

Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

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
{% endcode %}

## Get configuration

`GET /rest/agile/1.0/board/{boardId}/configuration`

Get the board configuration.&#x20;

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
{% endcode %}

## Create board

`POST /rest/agile/1.0/board`

Creates a new board. Board name, type ,and filter ID is required

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
{% endcode %}

## Get epics

`GET /rest/agile/1.0/board/{boardId}/epic`

Returns all epics from the board, for the given board ID. This only includes epics that the user has permission to view. Note, if the user does not have permission to view the board, no epics will be returned at all.

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

	epicsPage, response, err := atlassian.Board.Epics(context.Background(), 4, 0, 50, false)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, epic := range epicsPage.Values {
		log.Println(epic)
	}
}

```
{% endcode %}

## Get board by filter id

`GET /rest/agile/1.0/board/filter/{filterId}`

Returns any boards which use the provided filter id. This method can be executed by users without a valid software license in order to find which boards are using a particular filter.

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

	var (
		filterID  = 10016
		startAt   = 0
		maxResult = 50
	)

	boards, response, err := atlassian.Board.Filter(context.Background(), filterID, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, board := range boards.Values {
		log.Println(board.Name, board.ID, board.Type)
	}
}

```
{% endcode %}

## Get board

`GET /rest/agile/1.0/board/{boardId}`

Returns the board for the given board ID.&#x20;

* This board will only be returned if the user has permission to view it.&#x20;
* Admins without the view permission will see the board as a private one, so will see only a subset of the board's data (board location for instance).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	board, response, err := atlassian.Board.Get(context.Background(), 4)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(board)
	log.Println(board.Location)

	fmt.Println(response.Bytes.String())

}

```
{% endcode %}

## Get issues for board

`GET /rest/agile/1.0/board/{boardId}/issue`

Returns all issues from a board, for a given board ID.&#x20;

* This only includes issues that the user has permission to view.&#x20;
* An issue belongs to the board if its status is mapped to the board's column.&#x20;
* Epic issues do not belongs to the scrum boards.&#x20;
* Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

{% hint style="info" %}
**Note**, if the user does not have permission to view the board, no issues will be returned at all.&#x20;
{% endhint %}

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

	var (
		options = &models.IssueOptionScheme{
			//JQL:           "project = KP",
			//ValidateQuery: true,
			Fields: []string{"status", "issuetype", "summary"},
			Expand: []string{"changelog"},
		}

		boardID   = 4
		startAt   = 0
		maxResult = 50
	)

	issuesPage, response, err := atlassian.Board.Issues(context.Background(), boardID, options, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issuesPage.Issues {
		log.Println(issue.Key)
	}
}
```
{% endcode %}

## Get board issues for epic

`GET /rest/agile/1.0/board/{boardId}/epic/{epicId}/issue`

Returns all issues that belong to an epic on the board, for the given epic ID and the board ID.&#x20;

* This only includes issues that the user has permission to view.&#x20;
* Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

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

	var (
		options = &models.IssueOptionScheme{
			//JQL:           "project = KP",
			//ValidateQuery: true,
			Fields: []string{"status", "issuetype", "summary"},
			Expand: []string{"changelog"},
		}

		boardID   = 4
		epicID    = 10029
		startAt   = 0
		maxResult = 50
	)

	issuesPage, response, err := atlassian.Board.IssuesByEpic(context.Background(), boardID, epicID, options, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issuesPage.Issues {
		log.Println(issue.Key)
	}
}
```
{% endcode %}

## Get board issues for sprint

`GET /rest/agile/1.0/board/{boardId}/sprint/{sprintId}/issue`

Get all issues you have access to that belong to the sprint from the board.&#x20;

* Issue returned from this resource contains additional fields like: sprint, closedSprints, flagged and epic. Issues are returned ordered by rank. JQL order has higher priority than default rank.

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

	var (
		options = &models.IssueOptionScheme{
			//JQL:           "project = KP",
			//ValidateQuery: true,
			Fields: []string{"status", "issuetype", "summary"},
			Expand: []string{"changelog"},
		}

		boardID   = 4
		sprintID  = 4
		startAt   = 0
		maxResult = 50
	)

	issuesPage, response, err := atlassian.Board.IssuesBySprint(context.Background(), boardID, sprintID, options, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issuesPage.Issues {
		log.Println(issue.Key)
	}
}
```
{% endcode %}

## Get issues without epic for board

`GET /rest/agile/1.0/board/{boardId}/epic/none/issue`

Returns all issues that do not belong to any epic on a board, for a given board ID. This only includes issues that the user has permission to view. Issues returned from this resource include Agile fields, like sprint, closedSprints, flagged, and epic. By default, the returned issues are ordered by rank.

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

	issuesPage, response, err := atlassian.Board.IssuesWithoutEpic(context.Background(), boardID, options, startAt, maxResult)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, issue := range issuesPage.Issues {
		log.Println(issue.Key)
	}
}
```
{% endcode %}

## Move issues to backlog for board

`POST /rest/agile/1.0/board/{boardId}/issue`

Move issues to the backlog of a particular board (if they are already on that board).&#x20;

This operation is equivalent to remove future and active sprints from a given set of issues if the board has sprints If the board does not have sprints this will put the issues back into the backlog from the board. At most 50 issues may be moved at once.

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

	var (
		boardID = 7
		payload = &models.BoardMovementPayloadScheme{
			Issues:          []string{"KP-3"},
			RankBeforeIssue: "",
			RankAfterIssue:  "",
		}
	)

	response, err := atlassian.Board.Move(context.Background(), boardID, payload)
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

## Get projects

`GET /rest/agile/1.0/board/{boardId}/project`

Returns all projects that are associated with the board, for the given board ID. If the user does not have permission to view the board, no projects will be returned at all. Returned projects are ordered by the name.

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

	projects, response, err := atlassian.Board.Projects(context.Background(), 4, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, project := range projects.Values {
		log.Println(project)
	}
}
```
{% endcode %}

## Get all sprints

`GET /rest/agile/1.0/board/{boardId}/sprint`

Returns all sprints from a board, for a given board ID. This only includes sprints that the user has permission to view.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	var (
		boardID   = 4
		states    = []string{"future", "active"} // valid values: future, active, closed
		startAt   = 0
		maxResult = 50
	)

	sprints, response, err := atlassian.Board.Sprints(context.Background(), boardID, startAt, maxResult, states)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, sprint := range sprints.Values {
		log.Println(sprint)
	}

	fmt.Println(response.Bytes.String())
}
```
{% endcode %}

## Get all versions

`GET /rest/agile/1.0/board/{boardId}/version`

Returns all versions from a board, for a given board ID. This only includes versions that the user has permission to view. Note, if the user does not have permission to view the board, no versions will be returned at all. Returned versions are ordered by the name of the project from which they belong and then by sequence defined by user.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	var (
		boardID    = 4
		startAt    = 0
		maxResults = 50
		released   = false
	)

	versionsPage, response, err := atlassian.Board.Versions(context.Background(), boardID, startAt, maxResults, released)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, version := range versionsPage.Values {
		log.Println(version)
	}

	fmt.Println(response.Bytes.String())
}
```
{% endcode %}

## Delete Board

`DELETE /rest/agile/1.0/board/{boardId}`

Delete deletes the board. Admin without the view permission can still remove the board.

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

	response, err := atlassian.Board.Delete(context.Background(), 4)
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

## Get Boards

`GET /rest/agile/1.0/board`

Gets returns all boards. This only includes boards that the user has permission to view.

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

	options := &models.GetBoardsOptions{
		BoardType:               "",
		BoardName:               "",
		ProjectKeyOrID:          "",
		AccountIDLocation:       "",
		ProjectIDLocation:       "",
		IncludePrivate:          true,
		NegateLocationFiltering: false,
		OrderBy:                 "",
		Expand:                  "",
		FilterID:                0,
	}

	boards, response, err := atlassian.Board.Gets(context.Background(), options, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, board := range boards.Values {
		log.Println(board)
	}
}
```
{% endcode %}

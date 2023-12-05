---
cover: ../../.gitbook/assets/bb7f24ad-a935-415f-a11f-c1b2ffec7cb8-1560x760.jpeg
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

# 📃 Backlog

In <mark style="color:blue;">Jira</mark>, a backlog is a prioritized list of work items that need to be completed. It serves as a repository for all the tasks, user stories, bugs, and other work items that are yet to be worked on or are in the planning stage. Backlogs are typically associated with agile project management methodologies such as Scrum and Kanban.

<figure><img src="https://wac-cdn.atlassian.com/dam/jcr:6e0122d0-c7fe-4b32-bcfb-20b480780e51/AgileBacklogManyEpics.svg?cdnVersion=1004" alt="" width="563"><figcaption></figcaption></figure>

Backlogs are visualized as lists of items that can be sorted, filtered, and prioritized. Teams can easily move items between different backlogs, update their status, estimate effort, and collaborate on the work items. Backlogs provide a centralized view of all the work in progress and assist in effective planning, tracking, and delivery of projects.

### Move issues to backlog

`POST /rest/agile/1.0/backlog/issue`

Move moves issues to the backlog.

* This operation is equivalent to remove future and active sprints from a given set of issues.
* At most 50 issues may be moved at once.

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

   agile, err := agile.New(nil, host)
   if err != nil {
      return
   }

   agile.Auth.SetBasicAuth(mail, token)
   agile.Auth.SetUserAgent("curl/7.54.0")

   response, err := agile.Backlog.Move(context.Background(), []string{"KP-23"})
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", response.Bytes.String())
         log.Println("Status HTTP Response", response.Status)
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.Code)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   return
}
</code></pre>

### Move issues to a board backlog

`POST /rest/agile/1.0/backlog/{boardId}/issue`

* This operation is equivalent to remove future and active sprints from a given set of issues if the board has sprints.
* If the board does not have sprints this will put the issues back into the backlog from the board.
* At most 50 issues may be moved at once.

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

	agile, err := agile.New(nil, host)
	if err != nil {
		return
	}

	agile.Auth.SetBasicAuth(mail, token)
	agile.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.BoardBacklogPayloadScheme{
		Issues:            []string{"KP-23"},
		RankBeforeIssue:   "",
		RankAfterIssue:    "",
		RankCustomFieldId: 0,
	}

	response, err := agile.Backlog.MoveTo(context.Background(), 5, payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("Status HTTP Response", response.Status)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	return
}
```
{% endcode %}

# 🏣 Priorities

An issue's priority defines its importance in relation to other issues, so it helps your users determine which issues should be tackled first. Jira comes with a set of default priorities, which you can modify or add to. You can also choose different priorities for your projects.

![](../../.gitbook/assets/image%20%2812%29.png)

This resource represents issue priorities. Use it to obtain a list of issue priorities and details for individual issue priorities.

## Get priorities

Returns the list of all issue priorities, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	priorities, response, err := atlassian.Issue.Priority.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for pos, priority := range priorities {
		log.Println(pos, priority)
	}
}
```

## Get priority

Returns an issue priority, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var priorityID = "1"

	priority, response, err := atlassian.Issue.Priority.Get(context.Background(), priorityID)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(priority)
}
```


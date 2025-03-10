# 📭 Archiving

## Archive issues by issue ID/Key

`PUT /rest/api/{2-3}/issue/archive`

Preserve archives the given issues based on their issue IDs or keys.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	fmt.Println(host)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	archivalResult, response, err := atlassian.Archive.Preserve(context.Background(), []string{"KP-2"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Code", response.Code)
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(archivalResult.NumberOfIssuesUpdated)
}
```
{% endcode %}

## Archive issues by JQL

`POST /rest/api/{2-3}/issue/archive`

PreserveByJQL archives issues than match the provided JQL query.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	fmt.Println(host)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	taskID, response, err := atlassian.Archive.PreserveByJQL(context.Background(), "project = WORK")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Code", response.Code)
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(taskID)
}
```
{% endcode %}

## Restore issues by issue ID/Key

`PUT /rest/api/{2-3}/issue/unarchive`

Restore brings back the given archived issues using their issue IDs or keys.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN=""

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	fmt.Println(host)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	archivalResult, response, err := atlassian.Archive.Restore(context.Background(), []string{"KP-2"})
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Code", response.Code)
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(archivalResult.NumberOfIssuesUpdated)
}
```
{% endcode %}

## Export archived issues

`PUT /rest/api/{2-3}/issues/archive/export`

Export generates an export of archived issues based on the provided payload.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/v2/pkg/infra/models"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN=""

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

	exportConfigPayload := &models.IssueArchivalExportPayloadScheme{
		/*

			ArchivedBy: []string{
				"uuid-sample",
				"uuid-sample",
			},
		*/
		ArchivedDateRange: &models.DateRangeFilterRequestScheme{
			DateAfter:  "2023-01-01",
			DateBefore: "2023-01-12",
		},
		//IssueTypes: []string{"Bug", "Story"},
		Projects: []string{"WORK"},
		//Reporters:  []string{"uuid-sample"},
	}

	taskID, response, err := atlassian.Archive.Export(context.Background(), exportConfigPayload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Code", response.Code)
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		log.Fatal(err)
	}

	fmt.Println(response.Endpoint)
	fmt.Println(taskID)
}
```
{% endcode %}


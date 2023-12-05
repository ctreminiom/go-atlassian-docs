---
cover: ../../.gitbook/assets/fy22-shareholder-letter_1120x545@2x-1560x760.png
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

# 🚛 Metadata

## Get Edit Issue Metadata

`GET /rest/api/{2-3}/issue/{issueIdOrKey}/editmeta`

Returns the edit screen fields for an issue that are visible to and editable by the user. Use the information to populate the requests in [Edit issue](https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-issues/#api-rest-api-2-issue-issueidorkey-put).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

	metadata, response, err := atlassian.Issue.Metadata.Get(context.Background(), "KP-1", false, false)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)


	fields, response, err := atlassian.Issue.Field.Gets(context.Background())
	if err != nil {
		log.Fatal(err)
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range fields {

		path := fmt.Sprintf("fields.%v.required", field.ID)
		value := metadata.Get(path)

		if value.Exists() {
			fmt.Println("Field Name:", field.Name, "Required?:", value.String())
		}
	}
}
```
{% endcode %}

## Get Create Issue Metadata

`GET /rest/api/{2-3}/issue/createmeta`

Returns details of projects, issue types within projects, and, when requested, the create screen fields for each issue type for the user. Use the information to populate the requests in [Create issue](https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-issues/#api-rest-api-2-issue-post) and [Create issues](https://developer.atlassian.com/cloud/jira/platform/rest/v2/api-group-issues/#api-rest-api-2-issue-bulk-post).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := &models.IssueMetadataCreateOptions{
		ProjectIDs:     nil,
		ProjectKeys:    []string{"KP"},
		IssueTypeIDs:   nil,
		IssueTypeNames: nil,
		Expand:         "",
	}

	metadata, response, err := atlassian.Issue.Metadata.Create(context.Background(), options)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("HTTP Endpoint Used", response.Endpoint)

	fmt.Println(metadata)
}
```
{% endcode %}

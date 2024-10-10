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

## Get create metadata issue types for a project

`GET /rest/api/{2-3}/issue/createmeta/{projectIdOrKey}/issuetypes`

**FetchIssueMappings** returns a page of issue type metadata for a specified project. Use the information to populate the requests in [Create issue](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issues/#api-rest-api-3-issue-post) and [Create issues](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issues/#api-rest-api-3-issue-bulk-post). This operation can be accessed anonymously.

#### &#x20;<a href="#api-rest-api-3-issue-createmeta-projectidorkey-issuetypes-get" id="api-rest-api-3-issue-createmeta-projectidorkey-issuetypes-get"></a>

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueTypeMappings, response, err := atlassian.Issue.Metadata.FetchIssueMappings(context.Background(), "KP", 0, 50)
	if err != nil {
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(response.Endpoint)

	for _, issueType := range issueTypeMappings.Get("issueTypes").Array() {

		/*
			{
			      "description": "An error in the code",
			      "iconUrl": "https://your-domain.atlassian.net/images/icons/issuetypes/bug.png",
			      "id": "1",
			      "name": "Bug",
			      "self": "https://your-domain.atlassian.net/rest/api/3/issueType/1",
			      "subtask": false
			    }
		*/
		fmt.Println(issueType.Get("description").String())
		fmt.Println(issueType.Get("iconUrl").String())
		fmt.Println(issueType.Get("id").String())
		fmt.Println(issueType.Get("name").String())
		fmt.Println(issueType.Get("self").String())
		fmt.Println(issueType.Get("subtask").Bool())
		fmt.Println("------------")

	}
}
```
{% endcode %}

## Get create field metadata for a project and issue type id

`GET /rest/api/3/issue/createmeta/{projectIdOrKey}/issuetypes/{issueTypeId}`

\
Returns a page of field metadata for a specified project and issuetype id. Use the information to populate the requests in [Create issue](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issues/#api-rest-api-3-issue-post) and [Create issues](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-issues/#api-rest-api-3-issue-bulk-post). This operation can be accessed anonymously.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	fieldsMappings, response, err := atlassian.Issue.Metadata.FetchFieldMappings(context.Background(), "KP", "10001", 0, 50)
	if err != nil {
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(response.Endpoint)
	for _, fieldSchema := range fieldsMappings.Get("fields").Array() {

		fmt.Println("fieldId", fieldSchema.Get("fieldId"))
		fmt.Println("required", fieldSchema.Get("required"))
		fmt.Println("name", fieldSchema.Get("name"))
		fmt.Println("key", fieldSchema.Get("key"))
		fmt.Println("autoCompleteUrl", fieldSchema.Get("autoCompleteUrl"))
		fmt.Println("hasDefaultValue", fieldSchema.Get("hasDefaultValue"))
		fmt.Println("operations", fieldSchema.Get("operations").String())

		fmt.Println("schema.type", fieldSchema.Get("schema.type"))
		fmt.Println("schema.system", fieldSchema.Get("schema.system"))
		fmt.Println("schema.items", fieldSchema.Get("schema.items"))

		fmt.Println("------------")
	}

}
```
{% endcode %}



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

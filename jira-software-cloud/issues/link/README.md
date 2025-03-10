---
cover: ../../../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
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

# 🔗 Link

You can link issues to keep track of duplicate or related work. You can, for example:&#x20;

* create a new linked issue from an existing issue in a service project or business project
* create an association between an issue and a Confluence page
* link an issue to any other web page

Make sure you have the _link issues_ project permission before getting started. Note that your Jira administrator can customize the types of links that you can create.

![](../../../.gitbook/assets/KP-board-Agile-Board-Jira.png)

{% hint style="info" %}
This resource represents links between issues. Use it to get, create, and delete links between issues.To use this resource, the site must have issue linking enabled.
{% endhint %}

## Create issue link

`POST /rest/api/{2-3}/issueLink`

Creates a link between two issues. Use this operation to indicate a relationship between two issues and optionally add a comment to the from (outward) issue, the method returns the following information:

{% hint style="warning" %}
If the link request duplicates a link, the response indicates that the issue link was created. If the request included a comment, the comment is added.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	// Payload for the package v3 ---> ADF Format sample

	/*
	payloadADF := &models.LinkPayloadSchemeV3{
		Comment: &models.CommentPayloadScheme{

			Body: &models.CommentNodeScheme{
				Version: 1,
				Type:    "doc",
				Content: []*models.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*models.CommentNodeScheme{
							{
								Type: "text",
								Text: "Carlos Test",
							},
							{
								Type: "emoji",
								Attrs: map[string]interface{}{
									"shortName": ":grin",
									"id":        "1f601",
									"text":      "😁",
								},
							},
							{
								Type: "text",
								Text: " ",
							},
						},
					},
				},
			},
		},

		InwardIssue: &models.LinkedIssueScheme{
			Key: "KP-1",
		},
		OutwardIssue: &models.LinkedIssueScheme{
			Key: "KP-2",
		},
		Type: &models.LinkTypeScheme{
			Name: "Duplicate",
		},
	}
	*/


	payload := &models.LinkPayloadSchemeV2{

		Comment: &models.CommentPayloadSchemeV2{
			Body:       "test",
		},

		InwardIssue: &models.LinkedIssueScheme{
			Key: "KP-1",
		},
		OutwardIssue: &models.LinkedIssueScheme{
			Key: "KP-2",
		},
		Type: &models.LinkTypeScheme{
			Name: "Duplicate",
		},
	}

	response, err := atlassian.Issue.Link.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get issue links

`GET /rest/api/{2-3}/issue?fields=issuelinks`

Return the issue links associated with a Jira Issue, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueLinks, response, err := atlassian.Issue.Link.Gets(context.Background(), "KP-1")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, link := range issueLinks.Fields.IssueLinks {
		log.Println(link)
	}
}
```
{% endcode %}

## Get issue link

`GET /rest/api/{2-3}/issueLink/{linkId}`

Returns an issue link, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	issueLink, response, err := atlassian.Issue.Link.Get(context.Background(), "10002")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issueLink.ID)
	log.Println("----------------")
	log.Println(issueLink.Type.Name)
	log.Println(issueLink.Type.ID)
	log.Println(issueLink.Type.Self)
	log.Println(issueLink.Type.Inward)
	log.Println(issueLink.Type.Outward)
	log.Println("----------------")
	log.Println(issueLink.InwardIssue)
	log.Println(issueLink.OutwardIssue)
}
```
{% endcode %}

## Delete issue link

`DELETE /rest/api/{2-3}/issueLink/{linkId}`

Deletes an issue link, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Issue.Link.Delete(context.Background(), "10002")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

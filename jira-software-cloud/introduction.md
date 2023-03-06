# 🗃 Introduction

## Overview

The _go-atlassian_ `jira` module provides a set of functions and types for interacting with the Jira REST API. It is designed to make it easy for developers to build powerful integrations and automations that leverage the capabilities of the Jira platform.

The module is available in two versions: `v2` and `v3`. These versions correspond to different versions of the Jira REST API, with `v2` supporting Jira versions 6.4 to 7.2, and `v3` supporting Jira versions 7.3 and later.

## ADF Format

The Atlassian Document Format (ADF) is a JSON-based format used by Atlassian products, including Jira, to represent rich content such as text, tables, and lists. The `jira` module in **go-atlassian** provides support for working with ADF content in both the `v3`package.

```go
commentBody := jira.CommentNodeScheme{}
	commentBody.Version = 1
	commentBody.Type = "doc"
	
	
//Append Header
commentBody.AppendNode(&jira.CommentNodeScheme{
	Type:    "heading",
	Attrs: map[string]interface{}{"level": 1},

	Content: []*jira.CommentNodeScheme{
		{
			Type: "text",
			Text: "Header 1",
		},
	},
})
```

In `v3`, the `IssueService` and related services like `CommentService` provide functions that allow you to interact with ADF content. For example, the `CreateIssue` function in `v3` includes a parameter for `Fields`, which is a struct that includes fields for specifying various attributes of the new issue, including the issue summary, description, and any custom fields. The `Fields` struct includes a field for `Description`, which can be set to a struct representing ADF content.

{% embed url="https://developer.atlassian.com/cloud/jira/platform/apis/document/structure/" %}
Official Documentation
{% endembed %}

<table data-view="cards"><thead><tr><th></th></tr></thead><tbody><tr><td><code>body</code> in comments, including where comments are used in issue, issue link, and transition resources.</td></tr><tr><td><code>comment</code> in worklogs</td></tr><tr><td><code>description</code> and <code>environment</code> fields in issues.</td></tr></tbody></table>

To search issues using a JQL query, use the **Issue.Search** service function.

```go
var (
	jql    = "order by created DESC"
	fields = []string{"status"}
	expand = []string{"changelog", "renderedFields", "names", "schema", "transitions", "operations", "editmeta"}
)

issues, response, err := atlassian.Issue.Search.Post(context.Background(), jql, fields, expand, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

log.Println("HTTP Endpoint Used", response.Endpoint)
log.Println(issues.Total)
```

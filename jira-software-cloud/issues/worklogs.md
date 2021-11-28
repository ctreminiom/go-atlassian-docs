# 🕰 Worklogs

## Get Issue Worklogs

Returns worklogs for an issue, starting from the oldest worklog or from the worklog started on or after a date and time.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	var (
		ctx = context.Background()
		key = "KP-1"
		maxResult = 50
		after = 0
		expand = []string{""}
	)

	worklogs, response, err := atlassian.Issue.Worklog.Issue(ctx, key, 0, maxResult, after, expand)
	if err != nil {
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)

	for _, worklog := range worklogs.Worklogs {
		log.Println(worklog.ID)
	}
}
```

## Add Worklog

Adds a worklog to an issue.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	// V3 payload
	/*

		// Comment worklog
		commentBody := models.CommentNodeScheme{}
		commentBody.Version = 1
		commentBody.Type = "doc"

		//Create the Tables Headers
		tableHeaders := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 1",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 2",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 3",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

			},

		}

		row1 := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{
				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 00"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 01"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 02"},
							},
						},
					},
				},

			},

		}

		row2 := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{
				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 10"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 11"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 12"},
							},
						},
					},
				},

			},

		}

		commentBody.AppendNode(&v3.CommentNodeScheme{
			Type: "table",
			Attrs: map[string]interface{}{"isNumberColumnEnabled": false, "layout": "default"},
			Content: []*v3.CommentNodeScheme{tableHeaders, row1, row2},
		})

		var options = &v3.WorklogOptionsScheme{
			Notify:               true,
			AdjustEstimate:       "auto",
			ReduceBy:             "3h",
			//OverrideEditableFlag: true,
			Expand:               []string{"expand", "properties"},
			Payload:              &v3.WorklogPayloadScheme{
				Comment:          &commentBody,

				Visibility:       &jira.IssueWorklogVisibilityScheme{
					Type:  "group",
					Value: "jira-users",
				},
				Started:          "2021-07-16T07:01:10.774+0000",
				TimeSpentSeconds: 12000,
			},
		}
	*/

	options := &models.WorklogOptionsScheme{
		Notify:               false,
		AdjustEstimate:       "",
		NewEstimate:          "",
		ReduceBy:             "",
		OverrideEditableFlag: false,
		Expand:               nil,
	}

	payload := &models.WorklogPayloadSchemeV2{
		Comment: &models.CommentPayloadSchemeV2{
			Visibility: nil,
			Body:       "test",
		},
		Visibility:       nil,
		Started:          "2021-07-16T07:01:10.774+0000",
		TimeSpentSeconds: 12000,
	}

	worklog, response, err := atlassian.Issue.Worklog.Add(context.Background(), "KP-1", payload, options)
	if err != nil {
		log.Println(response.Endpoint, response.Code)
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
	log.Println(worklog.ID, worklog.IssueID)
}
```

## Get Worklog

Returns a worklog.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	var (
		ctx = context.Background()
		key = "KP-1"
		worklogID = "10000"
		expand = []string{"all"}
	)

	worklog, response, err := atlassian.Issue.Worklog.Get(ctx, key, worklogID, expand)
	if err != nil {
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
	log.Println(worklog.ID, worklog.Self)
}
```

## Update Worklog

Updates a worklog.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	/*
		// Comment worklog
		commentBody := v3.CommentNodeScheme{}
		commentBody.Version = 1
		commentBody.Type = "doc"

		//Create the Tables Headers
		tableHeaders := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 1",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 2",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

				{
					Type: "tableHeader",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{
									Type: "text",
									Text: "Header 3",
									Marks: []*v3.MarkScheme{
										{
											Type: "strong",
										},
									},
								},
							},
						},

					},
				},

			},

		}

		row1 := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{
				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 00"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 01"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 02"},
							},
						},
					},
				},

			},

		}

		row2 := &v3.CommentNodeScheme{
			Type: "tableRow",
			Content: []*v3.CommentNodeScheme{
				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 10"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 11"},
							},
						},
					},
				},

				{
					Type: "tableCell",
					Content: []*v3.CommentNodeScheme{
						{
							Type: "paragraph",
							Content: []*v3.CommentNodeScheme{
								{Type: "text", Text: "Row 12"},
							},
						},
					},
				},

			},

		}

		commentBody.AppendNode(&v3.CommentNodeScheme{
			Type: "table",
			Attrs: map[string]interface{}{"isNumberColumnEnabled": false, "layout": "default"},
			Content: []*v3.CommentNodeScheme{tableHeaders, row1, row2},
		})

		var options = &v3.WorklogOptionsScheme{
			Notify:               true,
			AdjustEstimate:       "auto",
			ReduceBy:             "3h",
			//OverrideEditableFlag: true,
			Expand:               []string{"expand", "properties"},
			Payload:              &v3.WorklogPayloadScheme{
				Comment:          &commentBody,
					Visibility:       &jira.IssueWorklogVisibilityScheme{
						Type:  "group",
						Value: "jira-users",
					},
				Started:          "2021-07-16T07:01:10.774+0000",
				TimeSpentSeconds: 12000,
			},
		}
	*/

	options := &models.WorklogOptionsScheme{
		Notify:               true,
		AdjustEstimate:       "auto",
		ReduceBy:             "3h",
		OverrideEditableFlag: false,
		Expand:               nil,
	}

	payload := &models.WorklogPayloadSchemeV2{
		Comment: &models.CommentPayloadSchemeV2{
			Visibility: nil,
			Body:       "test",
		},
		Visibility:       nil,
		Started:          "2021-07-16T07:01:10.774+0000",
		TimeSpentSeconds: 12000,
	}

	worklog, response, err := atlassian.Issue.Worklog.Update(context.Background(), "KP-1", "10000", payload, options)
	if err != nil {
		log.Println(response.Endpoint, response.Code)
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
	log.Println(worklog.ID, worklog.IssueID)

}
```

## Delete Worklog

Deletes a worklog from an issue.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	response, err := atlassian.Issue.Worklog.Delete(context.Background(), "KP-1", "10000", nil)
	if err != nil {
		log.Println(response.Endpoint, response.Code)
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
}

```

## Get ID's of deleted worklogs

Returns a list of IDs and delete timestamps for worklogs deleted after a date and time.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	result, response, err := atlassian.Issue.Worklog.Deleted(context.Background(), 0)
	if err != nil {
		log.Println(response.Endpoint, response.Code)
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
	log.Println(result)
}

```

## Get Worklogs

Returns worklog details for a list of worklog IDs.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	var (
		worklogsIDs = []int{10000}
		expand = []string{"all"}
	)

	worklogs, response, err := atlassian.Issue.Worklog.Gets(context.Background(), worklogsIDs, expand)
	if err != nil {
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)

	for _, worklog := range worklogs {
		log.Println(worklog)
	}
}

```

## Get ID's of updated worklogs

Returns a list of IDs and update timestamps for worklogs updated after a date and time.

This resource is paginated, with a limit of 1000 worklogs per page. Each page lists worklogs from oldest to youngest. If the number of items in the date range exceeds 1000, `until` indicates the timestamp of the youngest item on the page. Also, `nextPage` provides the URL for the next page of worklogs. The `lastPage` parameter is set to true on the last page of worklogs.c

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

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

	result, response, err := atlassian.Issue.Worklog.Updated(context.Background(), 0, nil)
	if err != nil {
		log.Println(response.Endpoint, response.Code)
		log.Fatal(err)
	}

	log.Println(response.Endpoint, response.Code)
	log.Println(result)
}
```

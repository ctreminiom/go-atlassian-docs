---
description: This resource represents issue comments.
---

# 📬 Comments

## Get comments

Returns all comments for an issue.

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

	comments, response, err := atlassian.Issue.Comment.Gets(context.Background(), "KP-2", "", nil, 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, comment := range comments.Comments {
		log.Println(comment.ID, comment.Created)
	}
}

```

## Get comment

Returns a comment.

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

	comment, response, err := atlassian.Issue.Comment.Get(context.Background(), "KP-2", "10011")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println(comment.ID, comment.Created)
}

```

## Delete comment

Deletes a comment.

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

	response, err := atlassian.Issue.Comment.Delete(context.Background(), "KP-2", "10011")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```

## Add comment

Adds a comment to an issue.

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
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   commentBody := jira.CommentNodeScheme{}
   commentBody.Version = 1
   commentBody.Type = "doc"

   commentBody.AppendNode(&jira.CommentNodeScheme{
      Type: "paragraph",
      Content: []*jira.CommentNodeScheme{
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
   })

   newComment, response, err := atlassian.Issue.Comment.Add(context.Background(), "KP-2", "role", "Administrators", &commentBody, nil)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", string(response.BodyAsBytes))
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.StatusCode)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   log.Println(newComment.ID)
}
```

## Atlassian Document Format <a id="atlassian-document-format"></a>

 The Atlassian Document Format \(ADF\) represents rich text stored in Atlassian products. For example, in Jira Cloud platform, the text in issue comments and in `textarea` custom fields is stored as ADF, here's a bundle of examples you can use in order to create custom body messages.



### Example 1

![](../../.gitbook/assets/image.png)

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

	//Append Lorem text
	commentBody.AppendNode(&jira.CommentNodeScheme{
		Type: "paragraph",
		Content: []*jira.CommentNodeScheme{
			{
				Type: "text",
				Text: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore " +
					"et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip " +
					"ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore " +
					"eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui " +
					"officia deserunt mollit anim id est laborum",
			},
		},
	})

	//Append Nested Panel Data
	commentBody.AppendNode(&jira.CommentNodeScheme{
		Type: "panel",
		Attrs: map[string]interface{}{"panelType":"note"},

		Content: []*jira.CommentNodeScheme{

			{
				Type: "paragraph",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "text",
						Text: "Excepteur ",
						Marks: []*jira.MarkScheme{
							{Type: "textColor", Attrs: map[string]interface{}{"color":"#6554c0"}},
						},
					},

					{
						Type: "text",
						Text: "sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum",
					},
				},


			},
		},

	})

	//Append Heading Title
	commentBody.AppendNode(&jira.CommentNodeScheme{
		Type: "heading",
		Attrs: map[string]interface{}{"level": 2},
		Content: []*jira.CommentNodeScheme{
			{
				Type: "text",
				Text: "Links",
			},
		},
	})

	//Append bulletList
	commentBody.AppendNode(&jira.CommentNodeScheme{
		Type: "bulletList",
		Content: []*jira.CommentNodeScheme{
			{
				Type: "listItem",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{
								Type: "text",
								Text: "Google.com",
								Marks: []*jira.MarkScheme{
									{
										Type: "link",
										Attrs: map[string]interface{}{"href": "https://www.google.com/"},
									},
								},
							},
						},
					},
				},
			},

			{
				Type: "listItem",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{
								Type: "text",
								Text: "Facebook.com",
								Marks: []*jira.MarkScheme{
									{
										Type: "link",
										Attrs: map[string]interface{}{"href": "https://www.facebook.com/"},
									},
								},
							},
						},
					},
				},
			},
		},
	})
```

### Example 2

![](../../.gitbook/assets/image%20%281%29.png)

```go
	commentBody := jira.CommentNodeScheme{}
	commentBody.Version = 1
	commentBody.Type = "doc"

	//Create the Tables Headers
	tableHeaders := &jira.CommentNodeScheme{
		Type: "tableRow",
		Content: []*jira.CommentNodeScheme{

			{
				Type: "tableHeader",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{
								Type: "text",
								Text: "Header 1",
								Marks: []*jira.MarkScheme{
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
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{
								Type: "text",
								Text: "Header 2",
								Marks: []*jira.MarkScheme{
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
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{
								Type: "text",
								Text: "Header 3",
								Marks: []*jira.MarkScheme{
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

	row1 := &jira.CommentNodeScheme{
		Type: "tableRow",
		Content: []*jira.CommentNodeScheme{
			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 00"},
						},
					},
				},
			},

			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 01"},
						},
					},
				},
			},

			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 02"},
						},
					},
				},
			},

		},

	}

	row2 := &jira.CommentNodeScheme{
		Type: "tableRow",
		Content: []*jira.CommentNodeScheme{
			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 10"},
						},
					},
				},
			},

			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 11"},
						},
					},
				},
			},

			{
				Type: "tableCell",
				Content: []*jira.CommentNodeScheme{
					{
						Type: "paragraph",
						Content: []*jira.CommentNodeScheme{
							{Type: "text", Text: "Row 12"},
						},
					},
				},
			},

		},

	}

	commentBody.AppendNode(&jira.CommentNodeScheme{
		Type: "table",
		Attrs: map[string]interface{}{"isNumberColumnEnabled": false, "layout": "default"},
		Content: []*jira.CommentNodeScheme{tableHeaders, row1, row2},
	})
```




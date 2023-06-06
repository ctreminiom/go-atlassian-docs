# 📃 Page

The Confluence Cloud V2 API includes a set of page endpoints that allow developers to manage and interact with pages in Confluence Cloud. These endpoints provide a range of features that make it easier to create, read, update, and delete pages, as well as retrieve information about them.

The page endpoints in the Confluence Cloud V2 API provide a range of features that are designed to make it easier to work with pages in Confluence Cloud. Some of the key features include:

1. Create Pages: Developers can use the page endpoints to create new pages in Confluence Cloud. This includes specifying the page title, content, and other relevant information.
2. Retrieve Page Information: The page endpoints also allow developers to retrieve information about a page, such as the page title, content, and other relevant metadata. This information can be used to populate user interfaces or other integrations.
3. Update Pages: The Confluence Cloud V2 API also allows developers to update existing pages. This includes changing the page title, content, and other relevant information.
4. Delete Pages: The page endpoints also provide the ability to delete pages in Confluence Cloud. This can be useful when managing pages that are no longer needed or when cleaning up after testing.
5. List Pages: Developers can use the page endpoints to retrieve a list of all pages in a space or for a particular user. This can be useful for building integrations that need to work with multiple pages.
6. Manage Comments: The page endpoints also provide the ability to manage comments on pages. This includes adding, retrieving, updating, and deleting comments.

## Get pages for label

GetsByLabel returns the pages of specified label. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"net/url"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var cursor string
	for {

		pages, response, err := instance.Page.GetsByLabel(context.Background(), 22, "", cursor, 20)
		if err != nil {

			if response != nil {
				log.Println(response.Bytes.String())
			}
		}

		for _, page := range pages.Results {
			fmt.Println(page.Title, page.ID, page.Version.Number)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if pages.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(pages.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```

## Get pages

Bulk returns all pages. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"net/url"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var cursor string
	for {

		pages, response, err := instance.Page.Bulk(context.Background(), cursor, 20)
		if err != nil {

			if response != nil {
				log.Println(response.Bytes.String())
			}
		}

		for _, page := range pages.Results {
			fmt.Println(page.Title, page.ID, page.Version.Number)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if pages.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(pages.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```

## Create page

Create creates a page in the space. Pages are created as published by default unless specified as a draft in the status field. If creating a published page, the title must be specified.

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	//Create the ADF body
	body := models.CommentNodeScheme{}
	body.Version = 1
	body.Type = "doc"

	// Status Macro
	body.AppendNode(&models.CommentNodeScheme{
		Type: "paragraph",
		Content: []*models.CommentNodeScheme{
			{
				Type: "status",
				Attrs: map[string]interface{}{
					"color":   "neutral",
					"style":   "bold",
					"text":    "Status neutral status",
					"localId": "3378f5db-c151-4ef2-aec4-56c18a1fbd96",
				},
			},
		},
	})

	// Action Item Macro
	body.AppendNode(&models.CommentNodeScheme{
		Type: "taskList",
		Content: []*models.CommentNodeScheme{
			{
				Type: "taskItem",
				Attrs: map[string]interface{}{
					"state":   "DONE",
					"localId": "1",
				},
				Content: []*models.CommentNodeScheme{
					{
						Type: "text",
						Text: "Action Item #1",
					},
				},
			},

			{
				Type: "taskItem",
				Attrs: map[string]interface{}{
					"state":   "DONE",
					"localId": "1",
				},
				Content: []*models.CommentNodeScheme{
					{
						Type: "text",
						Text: "Action Item #2",
					},
				},
			},

			{
				Type: "taskItem",
				Attrs: map[string]interface{}{
					"state":   "TODO",
					"localId": "1",
				},
				Content: []*models.CommentNodeScheme{
					{
						Type: "text",
						Text: "Action Item #3",
					},
				},
			},
		},
	})

	// Div Center Macro
	body.AppendNode(&models.CommentNodeScheme{
		Type: "paragraph",
		Marks: []*models.MarkScheme{
			{
				Type: "alignment",
				Attrs: map[string]interface{}{
					"align": "center",
				},
			},
		},
		Content: []*models.CommentNodeScheme{
			{
				Type: "text",
				Text: "Center DIV",
			},
		},
	})

	// Layout Macro
	body.AppendNode(&models.CommentNodeScheme{
		Type: "layoutSection",
		Content: []*models.CommentNodeScheme{
			{
				Type:  "layoutColumn",
				Attrs: map[string]interface{}{"width": 33.33},
				Content: []*models.CommentNodeScheme{
					{
						Type: "blockquote",
						Content: []*models.CommentNodeScheme{
							{
								Type: "paragraph",
								Content: []*models.CommentNodeScheme{
									{
										Type: "text",
										Text: "blockquote sample",
									},
								},
							},
						},
					},
				},
			},

			{
				Type:  "layoutColumn",
				Attrs: map[string]interface{}{"width": 33.33},
				Content: []*models.CommentNodeScheme{
					{
						Type:  "panel",
						Attrs: map[string]interface{}{"panelType": "info"},
						Content: []*models.CommentNodeScheme{
							{
								Type: "paragraph",
								Content: []*models.CommentNodeScheme{
									{
										Type: "text",
										Text: "info panel sample",
									},
								},
							},
						},
					},
				},
			},

			{
				Type:  "layoutColumn",
				Attrs: map[string]interface{}{"width": 33.33},
				Content: []*models.CommentNodeScheme{
					{
						Type: "extension",
						Attrs: map[string]interface{}{
							"layout":        "full-width",
							"extensionType": "com.atlassian.confluence.macro.core",
							"extensionKey":  "jira",
							"parameters": map[string]interface{}{

								"macroParams": map[string]interface{}{

									"server": map[string]interface{}{
										"value": "System JIRA"},

									"columns": map[string]interface{}{
										"value": "key,summary,type,created,updated,due,assignee,reporter,priority,status,resolution"},

									"maximumIssues": map[string]interface{}{
										"value": "20"},

									"jqlQuery": map[string]interface{}{
										"value": "project = KP"},

									"serverId": map[string]interface{}{
										"value": "26860604-baea-3dcc-bc9f-354ac3118c74"},
								},

								"macroMetadata": map[string]interface{}{

									"macroId": map[string]interface{}{
										"value": "2e756a2a-66cc-48be-979b-1d0c7af55923"},

									"schemaVersion": map[string]interface{}{
										"value": "1"},

									"placeholder": []map[string]interface{}{
										{
											"type": "image",
											"data": map[string]interface{}{
												"url": "/download/resources/confluence.extra.jira/jira-table.png",
											},
										},
									},
									"title": "Jira",
								},
							},
							"localId": "8d624240-7b38-4300-bbf4-ba53189f6e77",
						},
					},
				},
			},
		},
	})

	bodyValue, err := json.Marshal(&body)
	if err != nil {
		log.Fatal(err)
	}

	payload := &models.PageCreatePayloadScheme{
		SpaceID: 203718658,
		Status:  "current",
		Title:   "Page create title test",
		Body: &models.PageBodyRepresentationScheme{
			Representation: "atlas_doc_format",
			Value:          string(bodyValue),
		},
	}

	page, response, err := instance.Page.Create(context.Background(), payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(page.Title)
	fmt.Println(page.ID)
}
```

## Get page by id

Get returns a specific page.

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	page, response, err := instance.Page.Get(context.Background(), 215646235, "atlas_doc_format", false, 3)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(page.Title)
	fmt.Println(page.ID)
	fmt.Println(page.SpaceID)
	fmt.Println(page.Body.AtlasDocFormat.Value)

	adfBody := &models.CommentNodeScheme{}
	if err := json.Unmarshal([]byte(page.Body.AtlasDocFormat.Value), &adfBody); err != nil {
		log.Fatal(err)
	}

	fmt.Println("version", adfBody.Version)
	fmt.Println("type", adfBody.Type)
	fmt.Println("nodes", len(adfBody.Content))

	/*
	 data := Request{}
	    json.Unmarshal([]byte(s), &data)
	*/
}
```

## Update page

Update update a page by id.

```go
package main

import (
	"context"
	"encoding/json"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	// This example will append a status macro and a jira filter macron on the existing Confluence page

	// ----
	// Get the page adf format
	// ----
	page, response, err := instance.Page.Get(context.Background(), 215646235, "atlas_doc_format", false, 3)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
		}
	}

	// ----
	// Unmarshal the atlas_doc_format value to the ADF structs
	// ----
	adfBody := &models.CommentNodeScheme{}
	if err := json.Unmarshal([]byte(page.Body.AtlasDocFormat.Value), &adfBody); err != nil {
		log.Fatal(err)
	}

	// ----
	// Append the JQL filter macro
	// ----
	body := models.CommentNodeScheme{}
	body.Version = 1
	body.Type = "doc"

	// ----
	// Append the macro
	// ----
	adfBody.AppendNode(&models.CommentNodeScheme{
		Type: "extension",
		Attrs: map[string]interface{}{
			"layout":        "wide",
			"extensionType": "com.atlassian.confluence.macro.core",
			"extensionKey":  "jira",
			"parameters": map[string]interface{}{

				"macroParams": map[string]interface{}{

					"server": map[string]interface{}{
						"value": "System JIRA"},

					"columns": map[string]interface{}{
						"value": "key,summary,type,created,updated,due,assignee,reporter,priority,status,resolution"},

					"maximumIssues": map[string]interface{}{
						"value": "20"},

					"jqlQuery": map[string]interface{}{
						"value": "project = KP"},

					"serverId": map[string]interface{}{
						"value": "26860604-baea-3dcc-bc9f-354ac3118c74"},
				},

				"macroMetadata": map[string]interface{}{

					"macroId": map[string]interface{}{
						"value": "2e756a2a-66cc-48be-979b-1d0c7af55923"},

					"schemaVersion": map[string]interface{}{
						"value": "1"},

					"placeholder": []map[string]interface{}{
						{
							"type": "image",
							"data": map[string]interface{}{
								"url": "/download/resources/confluence.extra.jira/jira-table.png",
							},
						},
					},
					"title": "Jira",
				},
			},
			"localId": "8d624240-7b38-4300-bbf4-ba53189f6e77",
		},
	})

	bodyValue, err := json.Marshal(&adfBody)
	if err != nil {
		log.Fatal(err)
	}

	payload := &models.PageUpdatePayloadScheme{
		ID:      215646235,
		SpaceID: 203718658,
		Status:  "current",
		Title:   "Page create title test",
		Body: &models.PageBodyRepresentationScheme{
			Representation: "atlas_doc_format",
			Value:          string(bodyValue),
		},
		Version: &models.PageUpdatePayloadVersionScheme{
			Number:  4,
			Message: "Version #4",
		},
	}

	pageUpdated, response, err := instance.Page.Update(context.Background(), 215646235, payload)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(pageUpdated.Title)
	fmt.Println(pageUpdated.ID)
	fmt.Println(pageUpdated.Version)
}
```

## Delete page

Delete delete a page by id.

```go
package main

import (
	"context"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	response, err := instance.Page.Delete(context.Background(), 215646235)
	if err != nil {

		if response != nil {
			log.Println(response.Bytes.String())
		}
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
}
```

## Get pages in space

GetsBySpage returns all pages in a space. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"net/url"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var cursor string
	for {

		pages, response, err := instance.Page.GetsBySpace(context.Background(), 203718658, cursor, 20)
		if err != nil {

			if response != nil {
				log.Println(response.Bytes.String())
			}
		}

		for _, page := range pages.Results {
			fmt.Println(page.Title, page.ID, page.Version.Number)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if pages.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(pages.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```

## Get pages by parent

GetsByParent returns all direct descendent pages of a page. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"log"
	"net/url"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var cursor string
	for {

		pages, response, err := instance.Page.GetsByParent(context.Background(), 215646235, cursor, 20)
		if err != nil {

			if response != nil {
				log.Println(response.Bytes.String())
			}
		}

		for _, page := range pages.Results {
			fmt.Println(page.Title, page.ID, page.Version.Number)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if pages.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(pages.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```

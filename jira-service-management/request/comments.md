# 📬 Comments

## Get comment attachments

This method returns the attachments referenced in a comment.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-12"
		commentID    = 10015
	)

	attachments, response, err := atlassian.Request.Comment.Attachments(context.Background(), issueKeyOrID, commentID, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, attachment := range attachments.Values {
		log.Println(attachment.Filename, attachment.MimeType, attachment.Size)
	}

}
```

## Create request comment

This method creates a public or private (internal) comment on a customer request, with the comment visibility set by `public`. The user recorded as the author of the comment.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-12"
		body         = "Hello there"
	)

	newComment, response, err := atlassian.Request.Comment.Create(context.Background(), issueKeyOrID, body, true)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println("----------------------------------")
	log.Printf("Comment, ID: %v", newComment.ID)
	log.Printf("Comment, Creator Name: %v", newComment.Author.DisplayName)
	log.Printf("Comment, Created Date: %v", newComment.Created.Friendly)
	log.Printf("Comment, # of attachments: %v", newComment.Attachments.Size)
	log.Printf("Comment, is public?: %v", newComment.Public)
	log.Println("----------------------------------")

}

```

## Get request comment by id

This method returns details of a customer request's comment.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-12"
		commentID    = 10012
		expands      = []string{"attachment", "renderedBody"}
	)

	comment, response, err := atlassian.Request.Comment.Get(context.Background(), issueKeyOrID, commentID, expands)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println("----------------------------------")
	log.Printf("Comment, ID: %v", comment.ID)
	log.Printf("Comment, Creator Name: %v", comment.Author.DisplayName)
	log.Printf("Comment, Created Date: %v", comment.Created.Friendly)
	log.Printf("Comment, # of attachments: %v", comment.Attachments.Size)
	log.Printf("Comment, is public?: %v", comment.Public)
	log.Println("----------------------------------")

}
```

## Get request comments

This method returns all comments on a customer request. No permissions error is provided if, for example, the user doesn't have access to the service desk or request, the method simply returns an empty response.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := sm.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		issueKeyOrID = "DESK-12"
		expands      = []string{"attachment", "renderedBody"}
		start        = 0
		limit        = 50
	)

	comments, response, err := atlassian.Request.Comment.Gets(
		context.Background(),
		issueKeyOrID,
		true,
		expands,
		start,
		limit,
	)

	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, comment := range comments.Values {
		log.Println(comment.ID, comment.Created.Jira, comment.Body)
	}

}
```

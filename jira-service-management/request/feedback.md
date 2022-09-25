# 📮 Feedback

## Get feedback

This method retrieves a feedback of a request using it's `requestKey` or `requestId`

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
	atlassian.Auth.SetExperimentalFlag()

	var (
		issueKey = "DESK-12"
	)

	feedback, response, err := atlassian.Request.Feedback.Get(context.Background(), issueKey)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println("Feedback Rating: ", feedback.Rating)
	log.Println("Feedback Comment: ", feedback.Comment.Body)
}
```

## Post feedback

This method adds a feedback on an request using it's `requestKey` or `requestId`

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
	atlassian.Auth.SetExperimentalFlag()

	var (
		issueKey = "DESK-12"
		rating   = 2
		comment  = "Excellent service? :$"
	)

	feedback, response, err := atlassian.Request.Feedback.Post(context.Background(), issueKey, rating, comment)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Feedback Rating: ", feedback.Rating)
	log.Println("Feedback Comment: ", feedback.Comment.Body)
}

```

## Delete feedback

This method deletes the feedback of request using it's `requestKey` or `requestId`

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
	atlassian.Auth.SetExperimentalFlag()

	var (
		issueKey = "DESK-12"
	)

	response, err := atlassian.Request.Feedback.Delete(context.Background(), issueKey)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

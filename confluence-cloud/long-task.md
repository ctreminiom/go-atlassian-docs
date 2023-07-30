---
cover: ../.gitbook/assets/new-rules-of-productivity_1120x545@2x-1560x760.png
coverY: 0
---

# 🖼 Long Task

## Get long-running tasks

`GET /wiki/rest/api/longtask`

Returns information about all active long-running tasks (e.g. space export), such as how long each task has been running and the percentage of each task that has completed.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
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


	tasks, response, err := instance.LongTask.Gets(context.Background(), 0, 50)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, task := range tasks.Results {
		log.Println(task)
	}
}

```
{% endcode %}

## Get long-running task

`GET /wiki/rest/api/longtask/{id}`

Returns information about an active long-running task (e.g. space export), such as how long it has been running and the percentage of the task that has completed.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	task, response, err := instance.LongTask.Get(context.Background(), "195690524")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	log.Println(task)
}
```
{% endcode %}

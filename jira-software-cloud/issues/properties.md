---
cover: ../../.gitbook/assets/csd-7480-sustainability-work-life-t1-illo-1560x760.png
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

# 📤 Properties

### Get issue property keys

`GET /rest/api/{2-3}/issue/{issueIdOrKey}/properties`

Gets returns the URLs and keys of an issue's properties.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	properties, response, err := atlassian.Issue.Property.Gets(context.Background(), "KP-3")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, key := range properties.Keys {
		fmt.Println(key.Self, key.Key)
	}
}

```
{% endcode %}

### Get issue property

`GET /rest/api/{2-3}/issue/{issueIdOrKey}/properties/{propertyKey}`

Get returns the key and value of an issue's property.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	property, response, err := atlassian.Issue.Property.Get(context.Background(), "KP-3", "property-key")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	switch value := property.Value.(type) {

	case string:
		fmt.Println(value)

	case int:
		fmt.Println(value)

	case map[string]interface{}:

		for key, mapValue := range value {
			fmt.Println(key, mapValue)
		}

	default:
		fmt.Printf("I don't know about type %T!\n", value)
	}
}
```
{% endcode %}

### Set issue property

`PUT /rest/api/{2-3}/issue/{issueIdOrKey}/properties/{propertyKey}`

Set sets the value of an issue's property. Use this resource to store custom data against an issue.

{% hint style="info" %}
The value of the request body must be a [valid](http://tools.ietf.org/html/rfc4627), non-empty JSON blob. The maximum length is 32768 characters.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	payload := `{
		  "number": 5,
		  "string": "string-value"
		}`

	response, err := atlassian.Issue.Property.Set(context.Background(), "KP-3", "property-key", &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

### Delete issue property

`DELETE /rest/api/{2-3}/issue/{issueIdOrKey}/properties/{propertyKey}`

Deletes deletes an issue's property.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

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

	response, err := atlassian.Issue.Property.Delete(context.Background(), "KP-3", "property-key")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

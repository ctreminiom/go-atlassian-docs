---
cover: >-
  ../../.gitbook/assets/csd-222-t1illustrationrefresh-5-signs-of-a-toxic-work-culture-v4a-1560x760.png
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

## Get project properties keys

`GET /rest/api/{2-3}/project/{projectIdOrKey}/properties`

Returns all keys for the project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main()  {

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

	properties, response, err := atlassian.Project.Property.Gets(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, key := range properties.Keys {
		log.Printf("Key: %v -- Self: %v", key.Key, key.Self)
	}
}
```
{% endcode %}

## Get project property

`GET /rest/api/{2-3}/project/{projectIdOrKey}/properties/{propertyKey}`

Returns the value of a [project property](https://developer.atlassian.com/cloud/jira/platform/storing-data-without-a-database/#a-id-jira-entity-properties-a-jira-entity-properties).

<pre class="language-go" data-full-width="true"><code class="lang-go">package main
<strong>
</strong>import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main()  {

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

	property, response, err := atlassian.Project.Property.Get(context.Background(), "KP", "jswSelectedBoardType")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("Key: %v -- Value: %v", property.Key, property.Value)
}

</code></pre>

## Set project property

`PUT /rest/api/{2-3}/project/{projectIdOrKey}/properties/{propertyKey}`

Sets the value of the [project property](https://developer.atlassian.com/cloud/jira/platform/storing-data-without-a-database/#a-id-jira-entity-properties-a-jira-entity-properties). You can use project properties to store custom data against the project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main()  {

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

	response, err := atlassian.Project.Property.Set(context.Background(), "KP", "property-key", &payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endcode %}

## Delete project property

`DELETE /rest/api/{2-3}/project/{projectIdOrKey}/properties/{propertyKey}`

Deletes the [property](https://developer.atlassian.com/cloud/jira/platform/storing-data-without-a-database/#a-id-jira-entity-properties-a-jira-entity-properties) from a project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	v2 "github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main()  {

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

	response, err := atlassian.Project.Property.Delete(context.Background(), "KP", "property-key")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

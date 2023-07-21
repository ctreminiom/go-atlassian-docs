---
description: This resource represents the screens used to record issue details
cover: >-
  ../../.gitbook/assets/the-keystroke-that-changed-how-i-worked-forever-compressed-1560x760.gif
coverY: 0
---

# 📓 Screens

## Get screens for a field

`GET /rest/api/{2-3}/field/{fieldId}/screens`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of the screens a field is used in.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	var (
		fieldID   = "customfield_10014"
		startAt   = 0
		maxResult = 50
	)

	screens, response, err := atlassian.Screen.Fields(context.Background(), fieldID, startAt, maxResult)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, screen := range screens.Values {
		log.Println(screen.ID, screen.Name, screen.Description)
	}
}
```
{% endcode %}

## Get screens

`GET /rest/api/{2-3}/screens`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of all screens or those specified by one or more screen IDs.

{% code fullWidth="true" %}
```go
package main
import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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
	var (
		screenIDs  = []int{10000}
		startAt    = 0
		maxResults = 0
	)
	screens, response, err := atlassian.Screen.Gets(context.Background(), screenIDs, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	
	for _, screen := range screens.Values {
		log.Println(screen.ID, screen.Name, screen.Description)
	}
}
```
{% endcode %}

## Create screen

`POST /rest/api/{2-3}/screens`

Creates a screen with a default field tab.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	newScreen, response, err := atlassian.Screen.Create(context.Background(), "FX Screen", "sample description")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("The new screen has been created with the ID %v", newScreen.ID)
}
```
{% endcode %}

## Add field to default screen

`POST /rest/api/{2-3}/screens/addToDefault/{fieldId}`

Adds a field to the default tab of the default screen.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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
	response, err := atlassian.Screen.AddToDefault(context.Background(), "customfield_xxxx")
	if err != nil {
		log.Fatal(err)
	}
	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
</code></pre>

## Update screen

`PUT /rest/api/{2-3}/screens/{screenId}`

Updates a screen. Only screens used in classic projects can be updated.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	screenUpdated, response, err := atlassian.Screen.Update(context.Background(), 10015, "AX Screen", "")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(screenUpdated)
}
```
{% endcode %}

## Delete screen

`DELETE /rest/api/{2-3}/screens/{screenId}`

Deletes a screen. A screen cannot be deleted if it is used in a screen scheme, workflow, or workflow draft. Only screens used in classic projects can be deleted.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	response, err := atlassian.Screen.Delete(context.Background(), 10015)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Get available screen fields

`GET /rest/api/{2-3}/screens/{screenId}/availableFields`

Returns the fields that can be added to a tab on a screen.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	fields, response, err := atlassian.Screen.Available(context.Background(), 10000)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, field := range fields {
		log.Println(field.ID, field.Name)
	}
}
```
{% endcode %}

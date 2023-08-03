---
cover: ../.gitbook/assets/growthgauntletcoverillo.jpg
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

# 🚟 Search Project Boards

In this article, I would be showing you how to extract the boards associated with a Jira project using `go-atlassian`

## Step 1: Create a new Go project

Create a new directory for your project and navigate to it in your terminal or command prompt. Initialize a new Go module using the following command:

```bash
go mod init your-module-name
```

## Step 2: Install the "go-atlassian" library&#x20;

To use the "go-atlassian" library, you need to install it as a dependency in your project. Run the following command:

```bash
go get github.com/ctreminiom/go-atlassian
```

## Step 3: Import the required packages&#x20;

Create a new Go file, e.g., `main.go`, and import the necessary packages:

{% code fullWidth="true" %}
```go
package main

import (
	"fmt"
	"log"

	jira "github.com/ctreminiom/go-atlassian/agile"
)
```
{% endcode %}

## Step 4: Set up Jira Agile API client&#x20;

Initialize the Jira Agile API client with your Jira base URL and API token:

{% code fullWidth="true" %}
```go
func main() {
	jiraClient, err := jira.New(nil, "https://your-jira-instance.atlassian.net")
	if err != nil {
		log.Fatal(err)
	}

	// Set API token for authentication
	jiraClient.Auth.SetBasicAuth(mail, token)
}
```
{% endcode %}

## Step 5: Extract the boards

In this step, you're going to use the Jira Agile API, more specifically, the Board service, Gets() method. This methods supports multiple query parameters, but in this example, we're going to use the `ProjectKeyOrID` param to filter the board linked to a Jira project.



The following example iterates the Board.Gets() method and appends the boards into a slice.

{% code fullWidth="true" %}
```go
options := &models.GetBoardsOptions{
		ProjectKeyOrID: "KP",
	}

var boards []*models.BoardScheme
var startAt int

for {

	fmt.Println("Pagination #", startAt)

	page, response, err := instance.Board.Gets(context.Background(), options, startAt, 50)
	if err != nil {

		if response != nil {
			log.Fatal(response.Bytes.String())
		}
		log.Fatal(err)
	}

	boards = append(boards, page.Values...)

	if page.IsLast {
		break
	}

	startAt = +50
}
```
{% endcode %}


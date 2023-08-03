---
cover: ../.gitbook/assets/jql-functions-history-and-sorting0d@3x-1560x760.png
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

# ⏱ Export Issue History

In this article, I would be showing you how to extract the Jira history information using a JQL query and saves it into a .csv file.

## Step 1: Set up the project

1. Create a new directory for your project.
2. Open a terminal and navigate to the project directory.
3. Initialize a new Go module using the following command:

```bash
go mod init <module-name>
```

## Step 2: Install the "go-atlassian" library

In the terminal, run the following command to install the "go-atlassian" library:

```bash
go get -v github.com/ctreminiom/go-atlassian
```

## Step 3: Import the necessary packages

1. Create a new Go file (e.g., `main.go`) in your project directory.
2. Open the file and import the required packages:

{% code fullWidth="true" %}
```go
package main

import (
	"encoding/csv"
	"fmt"
	"log"
	"os"

	jira "github.com/ctreminiom/go-atlassian/jira/v2"
)
```
{% endcode %}

## Step 4: Authenticate with Jira

In the `main` function, create a new Jira client and authenticate using your Jira URL, username, and API token:

{% code fullWidth="true" %}
```go
func main() {

	jiraHost := "https://<_jira_instance_>.atlassian.net"
	mailAddress := "<your_mail>"
	apiToken := "<your_api_token>"

	client, err := jira.New(nil, jiraHost)
	if err != nil {
		log.Fatal(err)
	}

	client.Auth.SetBasicAuth(mailAddress, apiToken)
}
```
{% endcode %}

## Step 5: Execute the JQL query

Add the following code inside the `main` function to execute the JQL query and retrieve the issues:

{% code fullWidth="true" %}
```go
jql := "order by created DESC"

var startAt int
var issues []*models.IssueSchemeV2

for {
	log.Printf("Executing the pagination #%v", startAt)

	issuePage, _, err := client.Issue.Search.Post(
		context.Background(),
		jql,
		nil,
		[]string{"changelog"},
		startAt,
		50,
		"")

	if err != nil {
		log.Fatal(err)
	}

	issues = append(issues, issuePage.Issues...)

	if len(issuePage.Issues) == 0 {
		break
	}

	startAt += 50
}
```
{% endcode %}

## Step 6: Extract the issue history

Iterate over the retrieved issues and extract the changelog information.

{% code fullWidth="true" %}
```go
var changelogs [][]string
for _, issue := range issues {

	for _, history := range issue.Changelog.Histories {

		for _, item := range history.Items {

			var from string
			if item.From == "" {
				from = item.FromString
			} else {
				from = item.From
			}

			var to string
			if item.To == "" {
				to = item.ToString
			} else {
				to = item.To
			}

			changelogs = append(changelogs, []string{
				issue.Key,
				issue.Fields.Project.Name,
				issue.Fields.Summary,
				item.Field,
				from,
				to,
				history.Created,
				history.Author.EmailAddress,
			})
		}
	}
}
```
{% endcode %}

## Step 7: Save the issue history to a CSV file

Create a new CSV file and write the issue history data to it:

{% code fullWidth="true" %}
```go
file, err := os.Create("issue_history.csv")
if err != nil {
	log.Fatal(err)
}
defer file.Close()

writer := csv.NewWriter(file)
defer writer.Flush()

// Write header
if err := writer.Write([]string{"Issue Key", "Project Name", "Issue Summary", "Issue Field", "From", "To", "When", "Who?"}); err != nil {
	log.Fatal(err)
}

// Write the information
if err := writer.WriteAll(changelogs); err != nil {
	log.Fatal(err)
}

log.Println("Issue history saved to issue_history.csv")
```
{% endcode %}

## Step 8: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

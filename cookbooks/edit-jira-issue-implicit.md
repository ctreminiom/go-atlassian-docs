---
cover: ../.gitbook/assets/team_personality_tests_1120x545@2x-1560x760.jpeg
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

# 🚛 Edit Jira Issue (Implicit)

In this article, I would be showing you how to edit a Jira issue with the VERB operation using the "go-atlassian" library.

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
	"fmt"
	"log"
	"os"

	jira "github.com/ctreminiom/go-atlassian/jira/v2"
)
```
{% endcode %}

{% hint style="warning" %}
You can use the V2 and V3 Jira endpoint versions.
{% endhint %}

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

## Step 5: Create Issue Payload

The fields of an issue may also be updated in more flexible ways using the <mark style="color:blue;">**SET**</mark>, <mark style="color:blue;">**ADD**</mark> and <mark style="color:blue;">**REMOVE**</mark> operations. Not all fields support all operations, but as a general rule single value fields support <mark style="color:blue;">**SET**</mark>, whereas multi-value fields support <mark style="color:blue;">**SET**</mark>, <mark style="color:blue;">**ADD**</mark> and <mark style="color:blue;">**REMOVE**</mark>, where <mark style="color:blue;">**SET**</mark> replaces the field contents while <mark style="color:blue;">**ADD**</mark> and <mark style="color:blue;">**REMOVE**</mark> add or remove one or more values from the the current list of values.

{% code fullWidth="true" %}
```go
var operations = &models.UpdateOperations{}

err = operations.AddStringOperation("summary", "set", "Big block Chevy")
if err != nil {
	log.Fatal(err)
}

err = operations.AddArrayOperation("components", map[string]string{
	"name": "Trans/A",
})

err = operations.AddArrayOperation("labels", map[string]string{
	"triaged":   "remove",
	"triaged-2": "add",
})
```
{% endcode %}

{% code fullWidth="true" %}
```go
var payload = models.IssueSchemeV2{
	Fields: &models.IssueFieldsSchemeV2{
		Summary:  "New summary test",
		Priority: &models.PriorityScheme{Name: "Minor"},
	},
}
```
{% endcode %}

* Finally, call the Issue.Update() method and update the issue

{% code fullWidth="true" %}
```go
_, err = atlassian.Issue.Update(context.Background(), "KP-2", false, &payload, nil, operations)
if err != nil {
	log.Fatal(err)
}
```
{% endcode %}

## Step 6: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory.
3. Execute the following command to run the program:

```bash
go run main.go
```

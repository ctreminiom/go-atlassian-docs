# 👾 Create Jira Issue

In this article, I would be showing you how to create a Jira issue using the "go-atlassian" library.

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

```go
package main

import (
	"fmt"
	"log"
	"os"

	jira "github.com/ctreminiom/go-atlassian/jira/v2"
)
```

{% hint style="warning" %}
You can use the V2 and V3 Jira endpoint versions.
{% endhint %}

## Step 4: Authenticate with Jira

In the `main` function, create a new Jira client and authenticate using your Jira URL, username, and API token:

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

## Step 5: Custom fields definition

**OPTIONAL,** you can define custom-fields values you want to set on the issue creation. This library supports the following custom-fields types:

<table><thead><tr><th data-type="select" data-multiple>Custom-field Types</th><th data-type="select" data-multiple>Custom-field Types</th></tr></thead><tbody><tr><td></td><td></td></tr><tr><td></td><td></td></tr></tbody></table>

```go
var customFields = models.CustomFields{}
err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
if err != nil {
	log.Fatal(err)
}

err = customFields.Number("customfield_10043", 1000.3232)
if err != nil {
	log.Fatal(err)
}
```

## Step 6: Create an issue

Create a new issue using the `Create` method and set the custom fields:

```go
payload := &models.IssueSchemeV2{
	Fields: &models.IssueFieldsSchemeV2{
		Summary:   "New summary test",
		Project:   &models.ProjectScheme{ID: "10000"},
		IssueType: &models.IssueTypeScheme{Name: "Story"},
	},
}

newIssue, _, err := client.Issue.Create(context.Background(), payload, &customFields)
if err != nil {
	log.Fatal(err)
}
```

{% hint style="info" %}
Make sure to replace **`YOUR_PROJECT_KEY`** with the actual project key, and set the appropriate values for the other required fields like `Summary`, `Type`, `Assignee`, `Reporter`, etc.
{% endhint %}

## Step 7: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory.
3. Execute the following command to run the program:

This will create a new issue in Jira with the specified custom field values. Please note that you may need to modify the code according to your specific Jira configuration and custom field types.

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>


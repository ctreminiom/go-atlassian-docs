---
cover: ../.gitbook/assets/atla0623_rb_vs_lb_web_2240x1080-1560x760.jpg
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

## Step 5: Custom fields definition

**OPTIONAL,** you can define custom-fields values you want to set on the issue creation. This library supports the following custom-fields types:

<table><thead><tr><th>Custom-field Types<select multiple><option value="ca61adce4a714a1686153f2794c10e43" label="Groups" color="blue"></option><option value="894f4b6ba00545eaab5ac53d6e1f018d" label="Group" color="blue"></option><option value="c265f3ce819c441194679709c4728a8c" label="URL&#x27;s" color="blue"></option><option value="22578cc0c04f41c897ef4825b184875f" label="Text" color="blue"></option><option value="525132e454db46e285e53926caf6da23" label="DateTime" color="blue"></option><option value="7f88f2d3a7f44e70a5cebc5904c84dc3" label="Date" color="blue"></option><option value="6a978dc0976a45b8acaade7f5ca67f54" label="Cascading" color="blue"></option></select></th><th>Custom-field Types<select multiple><option value="edaf8f53ecf94c25a1f7f9c7e80f4b4f" label="MultiSelect" color="blue"></option><option value="76443721591f437f8cc465df959915f0" label="Select" color="blue"></option><option value="db958f7edd4742f29fa7bba3d6d12287" label="RadioButton" color="blue"></option><option value="65039e04087f46f4a833bcf94728620b" label="User" color="blue"></option><option value="e833285c3acb45cf9d8f106d72e5b5df" label="Users" color="blue"></option><option value="3bd70347b9c8468a8fe32a1c4a33bdd1" label="Number" color="blue"></option><option value="a6abbf07433245d3bf1c66a58182d0b2" label="CheckBox" color="blue"></option></select></th></tr></thead><tbody><tr><td><span data-option="ca61adce4a714a1686153f2794c10e43">Groups, </span><span data-option="894f4b6ba00545eaab5ac53d6e1f018d">Group, </span><span data-option="c265f3ce819c441194679709c4728a8c">URL's, </span><span data-option="22578cc0c04f41c897ef4825b184875f">Text, </span><span data-option="525132e454db46e285e53926caf6da23">DateTime, </span><span data-option="7f88f2d3a7f44e70a5cebc5904c84dc3">Date, </span><span data-option="6a978dc0976a45b8acaade7f5ca67f54">Cascading</span></td><td><span data-option="edaf8f53ecf94c25a1f7f9c7e80f4b4f">MultiSelect, </span><span data-option="76443721591f437f8cc465df959915f0">Select, </span><span data-option="db958f7edd4742f29fa7bba3d6d12287">RadioButton, </span><span data-option="65039e04087f46f4a833bcf94728620b">User, </span><span data-option="e833285c3acb45cf9d8f106d72e5b5df">Users, </span><span data-option="3bd70347b9c8468a8fe32a1c4a33bdd1">Number, </span><span data-option="a6abbf07433245d3bf1c66a58182d0b2">CheckBox</span></td></tr><tr><td></td><td></td></tr></tbody></table>

{% code fullWidth="true" %}
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
{% endcode %}

## Step 6: Create an issue

Create a new issue using the `Create` method and set the custom fields:

{% code fullWidth="true" %}
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
{% endcode %}

{% hint style="info" %}
Make sure to replace **`YOUR_PROJECT_KEY`** with the actual project key, and set the appropriate values for the other required fields like `Summary`, `Type`, `Assignee`, `Reporter`, etc.
{% endhint %}

## Step 7: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory.
3. Execute the following command to run the program:

This will create a new issue in Jira with the specified custom field values. Please note that you may need to modify the code according to your specific Jira configuration and custom field types.

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>


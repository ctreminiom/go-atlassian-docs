# 🚜 Edit Jira Issue (Explicit)

In this article, I would be showing you how to edit a Jira issue using the "go-atlassian" library.

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

## Step 5: Create Issue Payload

You do not need to send all the fields inside "fields". You can just send the fields you want to update. Absent fields are left unchanged and some fields cannot be updated this way (for example, comments). Instead you must use explicit-verb updates.

* For system-fields, you can use the \&models.IssueSchemeV2 struct, something like this:

```go
var payload = models.IssueSchemeV2{
	Fields: &models.IssueFieldsSchemeV2{
		Summary:  "New summary test",
		Priority: &models.PriorityScheme{Name: "Minor"},
	},
}
```

* For custom-fields, you can use the \&models.CustomFields struct to inyect the customfield's by type, something like this:

```go
var customFields = models.CustomFields{}
err = customFields.Groups("customfield_10052", []string{"jira-administrators", "jira-administrators-system"})
if err != nil {
	log.Fatal(err)
}

err = customFields.Number("customfield_10043", 9000)
if err != nil {
	log.Fatal(err)
}
```

* Finally, call the Issue.Update() method and update the issue

```go
_, err = atlassian.Issue.Update(context.Background(), "KP-2", false, &payload, &customFields, nil)
if err != nil {
	log.Fatal(err)
}
```

## Step 6: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory.
3. Execute the following command to run the program:

```bash
go run main.go
```

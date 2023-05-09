# 🚦 Create Jira Workflow

In this article, I would be showing you how to create Jira workflow and append transitions using `go-atlassian`

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

```go
package main

import (
	"fmt"
	"log"

	jira "github.com/ctreminiom/go-atlassian/jira/v2"
)
```

## Step 4: Set up Jira API client&#x20;

Initialize the Jira API client with your Jira base URL and API token:

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

## Step 5: Create a workflow&#x20;

To create a new workflow, use the `CreateWorkflow` method of the `WorkflowsService`. Provide the necessary parameters like the project key, name, and description:

# 🗃 Add CustomField to Screen

In this article, I would be showing you how to add custom-fields on a Jira project screens using `go-atlassian.` Before we get started, let's first understand the Jira entities involved in this process: screens, screen tabs, and screen tab fields.

1. **Screens**:
   * Screens in Jira define the layout and configuration of different views, such as issue creation, editing, and viewing.
   * Each screen consists of one or more screen tabs.
2. **Screen Tabs:**
   * Screen tabs are sections within a screen that group related fields together.
   * A screen can have multiple screen tabs, and each tab can contain multiple fields.
3. **Screen Tab Fields:**
   * Screen tab fields are the individual fields (e.g., standard or custom fields) displayed within a screen tab.

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

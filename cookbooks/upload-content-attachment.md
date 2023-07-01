# 🌄 Upload Content Attachment

In this article, I would be showing you how to upload an attachment in a Confluence content using the "go-atlassian" library.

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
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"os"
)
```

## Step 4: Authenticate with Confluence

In the `main` function, create a new Confluence client and authenticate using your Atlassian URL, username, and API token:

```go
func main() {

	atlassianHost := "https://<_jira_instance_>.atlassian.net"
	mailAddress := "<your_mail>"
	apiToken := "<your_api_token>"

	client, err := confluence.New(nil, atlassianHost )
	if err != nil {
		log.Fatal(err)
	}

	client.Auth.SetBasicAuth(mailAddress, apiToken)
}
```

## Step 5: Upload an attachment to a Confluence page

* Define the necessary variables for the page ID, file path, and file name:

```go
pageID := "76513281"
filePath := "confluence/mocks/mock.png"
fileName := "mock-00.png"
```

* Open the file using the provided file path:

```go
absolutePath, err := filepath.Abs(filePath)
if err != nil {
	log.Fatal(err)
}

log.Println("Using the path", absolutePath)

reader, err := os.Open(absolutePath)
if err != nil {
	log.Fatal(err)
}

defer reader.Close()
```

* Upload the attachment using the `Content.Attachment.Create()`method and provide the page ID, file name, and file content:

```go
attachmentsPage, response, err := client.Content.Attachment.Create(context.Background(), pageID, "current", fileName, reader)
if err != nil {

	if response != nil {
		if response.Code == http.StatusBadRequest {
			log.Println(response.Code)
		}
	}
	log.Println(response.Endpoint)
	log.Fatal(err)
}

log.Println("Endpoint:", response.Endpoint)
log.Println("Status Code:", response.Code)

for _, attachment := range attachmentsPage.Results {
	log.Println(attachment.ID, attachment.Title)
}
```

{% hint style="info" %}
Make sure to replace `<CONFLUENCE_PAGE_ID>`, `<FILE_PATH>`, and `<FILE_NAME>` with the actual values for your use case.
{% endhint %}

## Step 6: Run the program

1. Save the `main.go` file.
2. In the terminal, navigate to your project directory.
3. Execute the following command to run the program:

```bash
go run main.go
```

This will upload the specified file as an attachment to the Confluence page with the provided page ID.

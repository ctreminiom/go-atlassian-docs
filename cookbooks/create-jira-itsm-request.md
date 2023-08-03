---
cover: ../.gitbook/assets/ai-trends_1120x545@2x-1560x760.jpg
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

# 🚮 Create Jira ITSM Request

In this article, I would be showing you how to create a Service Management customer request with custom-fields.

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
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	client, err := sm.New(nil, jiraHost)
	if err != nil {
		log.Fatal(err)
	}

	client.Auth.SetBasicAuth(mailAddress, apiToken)
}
```
{% endcode %}

## Step 5: Create an ITSM customer request with custom fields

Define the fields you want to set:

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>form := &#x26;models.CustomerRequestFields{}
</strong>
if err := form.Text("summary", "Summary Sample"); err != nil {
	log.Fatal(err)
}

if err := form.Date("duedate", time.Now()); err != nil {
	log.Fatal(err)
}

if err := form.Components([]string{"Intranet"}); err != nil {
	log.Fatal(err)
}

if err := form.Labels([]string{"label-00", "label-01"}); err != nil {
	log.Fatal(err)
}
</code></pre>

{% hint style="info" %}
A list of the fields required by a customer request type can be obtained using the `sm.RequestType.Fields` method.
{% endhint %}

* Create a new issue using the `Create` method and set the custom fields:

{% code fullWidth="true" %}
```go
payload := &models.CreateCustomerRequestPayloadScheme{
	RequestParticipants: nil,
	ServiceDeskID:       "1",
	RequestTypeID:       "10",
}

ticket, response, err := atlassian.Request.Create(context.Background(), payload, form)
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

This will create a new ITSM customer request in Jira with the specified custom field values.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

---
cover: >-
  ../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
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

# 📅 Get User Last-Login Info

In this article, I would be showing you how to extract user last-activity information from the Atlassian Admin API's using `go-atlassian`

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

	"github.com/ctreminiom/go-atlassian/admin"
)
```
{% endcode %}

## Step 4: Set up Cloud Admin client&#x20;

Initialize the Atlassian Admin client with your API token:

{% code fullWidth="true" %}
```go
func main() {
	//https://support.atlassian.com/organization-administration/docs/manage-an-organization-with-the-admin-apis/
	var apiKey = os.Getenv("ATLASSIAN_ADMIN_TOKEN")

	cloudAdmin, err := admin.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	cloudAdmin.Auth.SetBearerToken(apiKey)
	cloudAdmin.Auth.SetUserAgent("curl/7.54.0")
}
```
{% endcode %}

## Step 5: Extract the organization ID

It's required to extract the ID of the organization you're using, the organization ID can be extract on the admin URL, as the image below:

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

## Step 6: Extract the organization users

In this step, we're going to extract the organization users and store them into an array variable, please use the following code block to extract them.

{% code fullWidth="true" %}
```go
// Extract the organization users
var (
	organizationID = "9a1jj823-jac8-123d-jj01-63315k059cb2"
	cursor         string
	users          []*models.AdminOrganizationUserScheme
)

for {

	page, response, err := cloudAdmin.Organization.Users(context.Background(), organizationID, cursor)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	users = append(users, page.Data...)

	if len(page.Links.Next) == 0 {
		break
	}

	//extract the next cursor pagination
	nextAsURL, err := url.Parse(page.Links.Next)
	if err != nil {
		log.Fatal(err)
	}

	cursor = nextAsURL.Query().Get("cursor")
}

for _, user := range users {
	log.Println("Name:", user.Name, " -- ID:", user.AccountID)
}
```
{% endcode %}

## Step 7: Extract the last-login date

With the organization users extracted, we need to iterate each user and extract the last-login information, you can use the following code to do that:

{% code fullWidth="true" %}
```go
for _, user := range users {

	lastLogin, response, err := cloudAdmin.Organization.Directory.Activity(context.Background(), organizationID, user.AccountID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	// The products are iterated because an Atlassian user may contain multiple products
	// If you're looking to a specific product, you can modify the loop logic and extract
	// the last-login for Jira or Confluence
	for _, product := range lastLogin.Data.ProductAccess {

		fmt.Println(lastLogin.Data.AddedToOrg, product)
	}
}
```
{% endcode %}

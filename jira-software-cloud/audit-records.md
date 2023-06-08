# 🛡️ Audit records

The Jira audit logs are a set of records that document all the activities and changes made in Jira. These logs provide a detailed record of who did what, when, and where within Jira. Here are a few examples of the type of activities that are logged in the Jira audit logs:

<table data-view="cards"><thead><tr><th></th></tr></thead><tbody><tr><td>User login and logout events</td></tr><tr><td>Issue creation, modification, and deletion events</td></tr><tr><td>Workflow transition events</td></tr><tr><td>Project creation, modification, and deletion events</td></tr><tr><td>User management events such as user creation, modification, and deletion</td></tr><tr><td>User management events such as user creation, modification, and deletion</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (8) (2) (1).png" alt=""><figcaption></figcaption></figure>

The audit logs are useful for several reasons. For example, they can be used to track changes made to sensitive data, to identify who made specific changes to issues or projects, or to diagnose issues with Jira.

## Get audit records

This method allows you to retrieve the audit records for specific activities that have occurred within Jira.&#x20;

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
	"time"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	auditRecordOption := &models.AuditRecordGetOptions{

		//Filter the records by a word, in that case, the custom field history
		Filter: "",

		//Filter the records by the last month
		From: time.Now().AddDate(0, -1, 0),

		// Today
		To: time.Now(),
	}

	auditRecords, response, err := jira.Audit.Get(context.Background(), auditRecordOption, 0, 500)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, record := range auditRecords.Records {

		log.Printf("Record ID: %v", record.ID)
		log.Printf("Record Category: %v", record.Category)
		log.Printf("Record Created: %v", record.Created)
		log.Printf("Record RemoteAddress: %v", record.RemoteAddress)
		log.Printf("Record Summary: %v", record.Summary)
		log.Printf("Record AuthorKey: %v", record.AuthorKey)
		log.Printf("\n")
	}
}

```

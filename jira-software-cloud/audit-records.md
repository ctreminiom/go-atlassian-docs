---
description: >-
  This resource represents audits that record activities undertaken in Jira. Use
  it to get a list of audit records.
---

# 🔎 Audit records

## Get audit records

Returns a list of audit records. The list can be filtered to include items:

* containing a string in at least one field. For example, providing _up_ will return all audit records where one or more fields contain words such as _update_.
* created on or after a date and time.
* created or before a date and time.
* created during a time period.

### Example

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
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

	jiraCloud, err := jira.New(nil, host)
	if err != nil {
		return
	}

	jiraCloud.Auth.SetBasicAuth(mail, token)
	jiraCloud.Auth.SetUserAgent("curl/7.54.0")

	auditRecordOption := &jira.AuditRecordGetOptions{

		//Filter the records by a word, in that case, the custom field history
		Filter: "custom field",

		//Filter the records by the last month
		From: time.Now().AddDate(0, -1, 0).Format(jira.DateFormatJira),

		// Today
		To: time.Now().Format(jira.DateFormatJira),
	}

	auditRecords, response, err := jiraCloud.Audit.Get(context.Background(), auditRecordOption, 0, 500)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
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


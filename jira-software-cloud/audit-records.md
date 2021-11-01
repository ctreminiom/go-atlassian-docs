# 🛡️ Audit records

The audit log tracks key activities that occur within the **Atlassian organization**. Use these activities to diagnose problems or questions related to user details, product access, managed accounts, and organization settings.

This resource represents audits that record activities undertaken in Jira. Use it to get a list of audit records.

{% hint style="warning" %}
**Tip: **The audit log includes activities for up to **180 days**. To save activities before they pass 180 days, [export the audit log](https://support.atlassian.com/security-and-access-policies/docs/track-organization-activities-from-the-audit-log/#Auditlogging-export) periodically.
{% endhint %}

* 🗡️ **Product audit logs:** Audit logs already exist in Jira Software Cloud and Confluence Cloud. Refer to the table to understand the different types of activities you can find in each audit log.



* 🏹 **Access audit log activities: **The audit log includes these types of activities.

![](<../.gitbook/assets/image (4).png>)

#### View the audit log

To access your organization's audit log, you must be an organization admin. From your organization at [admin.atlassian.com](http://admin.atlassian.com), select **Security** and then **Audit log**.

You’ll see a table of activities, organized by the date and time the activity happened, the actor who took the action, and the action itself.

![Atlassian Audit Log](<../.gitbook/assets/atlassian-api-audig-logs (1).gif>)



### Get audit records

Returns a list of audit records. The list can be filtered to include items:

* 🔒 **Permissions required**:  Administer Jira [global permission](https://confluence.atlassian.com/x/x4dKLg)

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
		Filter: "",

		//Filter the records by the last month
		From: time.Now().AddDate(0, -1, 0),

		// Today
		To: time.Now(),
	}

	auditRecords, response, err := jiraCloud.Audit.Get(context.Background(), auditRecordOption, 0, 500)
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

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type AuditRecordPageScheme struct {
	Offset  int                  `json:"offset,omitempty"`
	Limit   int                  `json:"limit,omitempty"`
	Total   int                  `json:"total,omitempty"`
	Records []*AuditRecordScheme `json:"records,omitempty"`
}

type AuditRecordScheme struct {
	ID              int                                `json:"id,omitempty"`
	Summary         string                             `json:"summary,omitempty"`
	RemoteAddress   string                             `json:"remoteAddress,omitempty"`
	AuthorKey       string                             `json:"authorKey,omitempty"`
	Created         string                             `json:"created,omitempty"`
	Category        string                             `json:"category,omitempty"`
	EventSource     string                             `json:"eventSource,omitempty"`
	Description     string                             `json:"description,omitempty"`
	ObjectItem      *AuditRecordObjectItemScheme       `json:"objectItem,omitempty"`
	ChangedValues   []*AuditRecordChangedValueScheme   `json:"changedValues,omitempty"`
	AssociatedItems []*AuditRecordAssociatedItemScheme `json:"associatedItems,omitempty"`
}

type AuditRecordObjectItemScheme struct {
	ID         string `json:"id,omitempty"`
	Name       string `json:"name,omitempty"`
	TypeName   string `json:"typeName,omitempty"`
	ParentID   string `json:"parentId,omitempty"`
	ParentName string `json:"parentName,omitempty"`
}

type AuditRecordChangedValueScheme struct {
	FieldName   string `json:"fieldName,omitempty"`
	ChangedFrom string `json:"changedFrom,omitempty"`
	ChangedTo   string `json:"changedTo,omitempty"`
}

type AuditRecordAssociatedItemScheme struct {
	ID         string `json:"id,omitempty"`
	Name       string `json:"name,omitempty"`
	TypeName   string `json:"typeName,omitempty"`
	ParentID   string `json:"parentId,omitempty"`
	ParentName string `json:"parentName,omitempty"`
}
```

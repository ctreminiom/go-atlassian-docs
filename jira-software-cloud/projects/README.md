---
description: >-
  This resource represents projects. Use this resource to get, create, update,
  and delete projects. Also get statuses available to a project, a project's
  notification schemes, and update a project's typ
---

# 📚 Projects

## Create project

Creates a project based on a project type template

```go
const (
	BusinessContentManagement    = "com.atlassian.jira-core-project-templates:jira-core-simplified-content-management"
	BusinessDocumentApproval     = "com.atlassian.jira-core-project-templates:jira-core-simplified-document-approval"
	BusinessLeadTracking         = "com.atlassian.jira-core-project-templates:jira-core-simplified-lead-tracking"
	BusinessProcessControl       = "com.atlassian.jira-core-project-templates:jira-core-simplified-process-control"
	BusinessProcurement          = "com.atlassian.jira-core-project-templates:jira-core-simplified-procurement"
	BusinessProjectManagement    = "com.atlassian.jira-core-project-templates:jira-core-simplified-project-management"
	BusinessRecruitment          = "com.atlassian.jira-core-project-templates:jira-core-simplified-recruitment"
	BusinessTaskTracking         = "com.atlassian.jira-core-project-templates:jira-core-simplified-task-tracking"
	ITSMServiceDesk              = "com.atlassian.servicedesk:simplified-it-service-desk"
	ITSMInternalServiceDesk      = "com.atlassian.servicedesk:simplified-internal-service-desk"
	ITSMExternalServiceDesk      = "com.atlassian.servicedesk:simplified-external-service-desk"
	SoftwareTeamManagedKanban    = "com.pyxis.greenhopper.jira:gh-simplified-agility-kanban"
	SoftwareTeamManagedScrum     = "com.pyxis.greenhopper.jira:gh-simplified-agility-scrum"
	SoftwareCompanyManagedKanban = "com.pyxis.greenhopper.jira:gh-simplified-kanban-classic"
	SoftwareCompanyManagedScrum  = "com.pyxis.greenhopper.jira:gh-simplified-scrum-classic"
)
```

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &jira.ProjectPayloadScheme{
		NotificationScheme:  10021,
		Description:         "Example Project description",
		LeadAccountID:       "5b86be50b8e3cb5895860d6d",
		URL:                 "http://atlassian.com",
		ProjectTemplateKey:  "com.pyxis.greenhopper.jira:gh-simplified-agility-kanban",
		AvatarID:            10200,
		IssueSecurityScheme: 10001,
		Name:                "Project DUMMY #3",
		PermissionScheme:    10011,
		AssigneeType:        "PROJECT_LEAD",
		ProjectTypeKey:      "software",
		Key:                 "DUMMY3",
		CategoryID:          10120,
	}

	newProject, response, err := atlassian.Project.Create(context.Background(), payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println("-------------------")
	log.Println(newProject.ID)
	log.Println(newProject.Self)
	log.Println(newProject.Key)
	log.Println("-------------------")
}
```

## Get projects paginated

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of projects visible to the user.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := &jira.ProjectSearchOptionsScheme{
		OrderBy: "issueCount",
		Action:  "browse",
		Expand:  []string{"insight", "lead", "issueTypes", "projectKeys", "description"},
	}

	var (
		startAt    = 0
		maxResults = 50
	)

	projects, response, err := atlassian.Project.Search(context.Background(), options, startAt, maxResults)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, project := range projects.Values {
		log.Println(project)
	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type ProjectSearchScheme struct {
	Self       string           `json:"self,omitempty"`
	NextPage   string           `json:"nextPage,omitempty"`
	MaxResults int              `json:"maxResults,omitempty"`
	StartAt    int              `json:"startAt,omitempty"`
	Total      int              `json:"total,omitempty"`
	IsLast     bool             `json:"isLast,omitempty"`
	Values     []*ProjectScheme `json:"values,omitempty"`
}
```

## Get project

&#x20;Returns the [project details](https://confluence.atlassian.com/x/ahLpNw)

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	project, response, err := atlassian.Project.Get(context.Background(), "KP", []string{"issueTypes"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(project)

	for _, issueType := range project.IssueTypes {
		log.Println(issueType.Name)
	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags  &#x20;
{% endhint %}

```go
type ProjectScheme struct {
   Expand            string                    `json:"expand,omitempty"`
   Self              string                    `json:"self,omitempty"`
   ID                string                    `json:"id,omitempty"`
   Key               string                    `json:"key,omitempty"`
   Description       string                    `json:"description,omitempty"`
   URL               string                    `json:"url,omitempty"`
   Email             string                    `json:"email,omitempty"`
   AssigneeType      string                    `json:"assigneeType,omitempty"`
   Name              string                    `json:"name,omitempty"`
   ProjectTypeKey    string                    `json:"projectTypeKey,omitempty"`
   Simplified        bool                      `json:"simplified,omitempty"`
   Style             string                    `json:"style,omitempty"`
   Favourite         bool                      `json:"favourite,omitempty"`
   IsPrivate         bool                      `json:"isPrivate,omitempty"`
   UUID              string                    `json:"uuid,omitempty"`
   Lead              *UserScheme               `json:"lead,omitempty"`
   Components        []*ProjectComponentScheme `json:"components,omitempty"`
   IssueTypes        []*IssueTypeScheme        `json:"issueTypes,omitempty"`
   Versions          []*ProjectVersionScheme   `json:"versions,omitempty"`
   Roles             *ProjectRolesScheme       `json:"roles,omitempty"`
   AvatarUrls        *AvatarURLScheme          `json:"avatarUrls,omitempty"`
   ProjectKeys       []string                  `json:"projectKeys,omitempty"`
   Insight           *ProjectInsightScheme     `json:"insight,omitempty"`
   Category          *ProjectCategoryScheme    `json:"projectCategory,omitempty"`
   Deleted           bool                      `json:"deleted,omitempty"`
   RetentionTillDate string                    `json:"retentionTillDate,omitempty"`
   DeletedDate       string                    `json:"deletedDate,omitempty"`
   DeletedBy         *UserScheme               `json:"deletedBy,omitempty"`
   Archived          bool                      `json:"archived,omitempty"`
   ArchivedDate      string                    `json:"archivedDate,omitempty"`
   ArchivedBy        *UserScheme               `json:"archivedBy,omitempty"`
}
```

## Update project

&#x20;Updates the [project details](https://confluence.atlassian.com/x/ahLpNw) of a project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &jira.ProjectUpdateScheme{
		Description: "Example Project description",
	}

	projectUpdated, response, err := atlassian.Project.Update(context.Background(), "KP", payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(projectUpdated)
}
```

## Delete project

Deletes a project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
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

	response, err := jiraCloud.Project.Delete(context.Background(), "DUM", true)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Archive project

Archives a project. Archived projects cannot be deleted. To delete an archived project, restore the project and then delete it. To restore a project, use the Jira UI.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Project.Archive(context.Background(), "PK")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

## Delete project asynchronously

Deletes a project asynchronously.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
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

	task, response, err := jiraCloud.Project.DeleteAsynchronously(context.Background(), "DUM")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(task.ID)
	log.Println(task)
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags  &#x20;
{% endhint %}

```go
type TaskScheme struct {
   Self           string `json:"self"`
   ID             string `json:"id"`
   Description    string `json:"description"`
   Status         string `json:"status"`
   Result         string `json:"result"`
   SubmittedBy    int    `json:"submittedBy"`
   Progress       int    `json:"progress"`
   ElapsedRuntime int    `json:"elapsedRuntime"`
   Submitted      int64  `json:"submitted"`
   Started        int64  `json:"started"`
   Finished       int64  `json:"finished"`
   LastUpdate     int64  `json:"lastUpdate"`
}
```

## Restore deleted project

Restores a project from the Jira recycle bin.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
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

	projectRestored, response, err := jiraCloud.Project.Restore(context.Background(), "DUM")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(projectRestored)

}
```

## Get all statuses for project

Returns the valid statuses for a project. The statuses are grouped by issue type, as each project has a set of valid issue types and each issue type has a set of valid statuses.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	statuses, response, err := atlassian.Project.Statuses(context.Background(), "KP")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, status := range statuses {
		log.Println(status)
	}
}
```

## Get project issue type hierarchy

Get the issue type hierarchy for a next-gen project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	hierarchy, response, err := atlassian.Project.Hierarchy(context.Background(), "PK")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(hierarchy)
}

```

## Get project notification scheme

&#x20;Gets a [notification scheme](https://confluence.atlassian.com/x/8YdKLg) associated with the project.&#x20;

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := jira.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	notificationScheme, response, err := atlassian.Project.NotificationScheme(context.Background(), "KP", []string{"all"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(notificationScheme)

	log.Println(notificationScheme.Name, notificationScheme.ID)

	for _, event := range notificationScheme.NotificationSchemeEvents {

		log.Println(event.Event.ID, event.Event.Name)

		for index, notification := range event.Notifications {
			log.Println(index, event.Event.Name, notification.ID, notification.NotificationType, notification.Parameter)
		}

	}
}
```

{% hint style="info" %}
🧚‍♀️ **Tips: **You can extract the following struct tags
{% endhint %}

```go
type NotificationSchemeScheme struct {
   Expand                   string                                  `json:"expand,omitempty"`
   ID                       int                                     `json:"id,omitempty"`
   Self                     string                                  `json:"self,omitempty"`
   Name                     string                                  `json:"name,omitempty"`
   Description              string                                  `json:"description,omitempty"`
   NotificationSchemeEvents []*ProjectNotificationSchemeEventScheme `json:"notificationSchemeEvents,omitempty"`
   Scope                    *TeamManagedProjectScopeScheme          `json:"scope,omitempty"`
}

type ProjectNotificationSchemeEventScheme struct {
   Event         *NotificationEventScheme   `json:"event,omitempty"`
   Notifications []*EventNotificationScheme `json:"notifications,omitempty"`
}

type NotificationEventScheme struct {
   ID            int                      `json:"id,omitempty"`
   Name          string                   `json:"name,omitempty"`
   Description   string                   `json:"description,omitempty"`
   TemplateEvent *NotificationEventScheme `json:"templateEvent,omitempty"`
}

type EventNotificationScheme struct {
   Expand           string             `json:"expand,omitempty"`
   ID               int                `json:"id,omitempty"`
   NotificationType string             `json:"notificationType,omitempty"`
   Parameter        string             `json:"parameter,omitempty"`
   EmailAddress     string             `json:"emailAddress,omitempty"`
   Group            *GroupScheme       `json:"group,omitempty"`
   Field            *IssueFieldScheme  `json:"field,omitempty"`
   ProjectRole      *ProjectRoleScheme `json:"projectRole,omitempty"`
   User             *UserScheme        `json:"user,omitempty"`
}
```

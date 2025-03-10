---
cover: >-
  ../../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
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

# 📚 Projects

## Create project

`POST /rest/api/{2-3}/project`

Creates a project based on a project type template

{% code fullWidth="true" %}
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
{% endcode %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.ProjectPayloadScheme{
		NotificationScheme:  10021,
		Description:         "Example Project description",
		LeadAccountID:       "5b86be50b8e3cb5895860d6d",
		URL:                 "https://atlassian.com",
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
{% endcode %}

## Get projects paginated

`GET /rest/api/{2-3}/project/search`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of projects visible to the user.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	options := &models.ProjectSearchOptionsScheme{
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
{% endcode %}

## Get project

`GET /rest/api/{2-3}/project/{projectIdOrKey}`&#x20;

Returns the [project details](https://confluence.atlassian.com/x/ahLpNw)

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
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
{% endcode %}

## Update project

`PUT /rest/api/{2-3}/project/{projectIdOrKey}`

Updates the [project details](https://confluence.atlassian.com/x/ahLpNw) of a project.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.ProjectUpdateScheme{
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
{% endcode %}

## Delete project

`DELETE /rest/api/{2-3}/project/{projectIdOrKey}`

Deletes a project.

{% code fullWidth="true" %}
```go

package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jiraCloud, err := v2.New(nil, host)
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
{% endcode %}

## Archive project

`POST /rest/api/{2-3}/project/{projectIdOrKey}/archive`

Archives a project. Archived projects cannot be deleted. To delete an archived project, restore the project and then delete it. To restore a project, use the Jira UI.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
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
{% endcode %}

## Delete project asynchronously

`POST /rest/api/{2-3}/project/{projectIdOrKey}/delete`

Deletes a project asynchronously.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jiraCloud, err := v2.New(nil, host)
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
{% endcode %}

## Restore deleted project

`POST /rest/api/{2-3}/project/{projectIdOrKey}/restore`

Restores a project from the Jira recycle bin.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jiraCloud, err := v2.New(nil, host)
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
{% endcode %}

## Get all statuses for project

`GET /rest/api/{2-3}/project/{projectIdOrKey}/statuses`

Returns the valid statuses for a project. The statuses are grouped by issue type, as each project has a set of valid issue types and each issue type has a set of valid statuses.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
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
{% endcode %}

## Get project notification scheme

`GET /rest/api/{2-3}/project/{projectKeyOrId}/notificationscheme`

Gets a [notification scheme](https://confluence.atlassian.com/x/8YdKLg) associated with the project.&#x20;

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
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

	atlassian, err := v2.New(nil, host)
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
{% endcode %}

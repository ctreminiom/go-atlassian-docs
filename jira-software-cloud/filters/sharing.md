---
description: >-
  This resource represents options for sharing filters. Use it to get share
  scopes as well as add and remove share scopes from filters.
---

# 📐Sharing

{% page-ref page="sharing.md" %}

{% tabs %}
{% tab title="Get default share scope" %}
Returns the default sharing settings for new filters and dashboards for a user.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-defaultsharescope-get).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	scope, response, err := atlassian.Filter.Share.Scope(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("Scope", scope)
}

```
{% endtab %}

{% tab title="Set default share scope" %}
Sets the default sharing for new filters and dashboards for a user.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-defaultsharescope-put).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Filter.Share.SetScope(context.Background(), "GLOBAL")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println(response)
}

```
{% endtab %}

{% tab title="Get share permissions" %}
Returns the share permissions for a filter. A filter can be shared with groups, projects, all logged-in users, or the public. Sharing with all logged-in users or the public is known as global share permission.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-id-permission-get).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	permissions, response, err := atlassian.Filter.Share.Gets(context.Background(), filterID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for index, permission := range *permissions {
		log.Println(index, permission.ID, permission.Type)
	}
}

```
{% endtab %}

{% tab title="Add share permission" %}
Add a share permissions to a filter. If you add global share permission \(one for all logged-in users or the public\) it will overwrite all share permissions for the filter.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](%20https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-id-permission-get).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	/*
	We can add different share permissions, for example:

	---- Project ID only
	payload := jira.PermissionFilterBodyScheme{
			Type:      "project",
			ProjectID: "10000",
		}

	---- Project ID and role ID
	payload := jira.PermissionFilterBodyScheme{
			Type:          "project",
			ProjectID:     "10000",
			ProjectRoleID: "222222",
		}

	==== Group Name
	payload := jira.PermissionFilterBodyScheme{
			Type:          "group",
			GroupName: "jira-users",
		}
	*/

	payload := jira.PermissionFilterBodyScheme{
		Type:          "project",
		ProjectID:     "10000",
	}

	permissions, response, err := atlassian.Filter.Share.Add(context.Background(), filterID, &payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println("Response HTTP Code", response.StatusCode)
			log.Println("HTTP Endpoint Used", response.Endpoint)
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for index, permission := range *permissions {
		log.Println(index, permission.ID, permission.Type)
	}
}

```
{% endtab %}

{% tab title="Get share permission" %}
Returnsshare permission for a filter. A filter can be shared with groups, projects, all logged-in users, or the public. Sharing with all logged-in users or the public is known as global share permission.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-id-permission-permissionid-get).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	permission, response, err := atlassian.Filter.Share.Get(context.Background(), "$Filter_ID", permissionID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(permission)
}

```
{% endtab %}

{% tab title="Delete share permission" %}
Deletes share permission from a filter.

{% hint style="info" %}
Jira Cloud API endpoint documentation [here](https://developer.atlassian.com/cloud/jira/platform/rest/v3/api-group-filter-sharing/#api-rest-api-3-filter-id-permission-permissionid-delete).
{% endhint %}

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

	atlassian, err := jira.New(nil, host)
	if err != nil {
		return
	}

	response, err := atlassian.Filter.Share.Delete(context.Background(), "filterID", permissionID)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}

```
{% endtab %}
{% endtabs %}


---
cover: ../../../../.gitbook/assets/atla0623_rb_vs_lb_web_2240x1080-1560x760.jpg
coverY: 229
---

# 🔃 Schemes

The Issue field configuration schemes let you apply a field configuration to all issues of a certain type. When you want to change the fields that appear on all Bug issue types in a certain project, you can do so by simply configuring the associated field configuration scheme. You can also save time by reusing the same field configuration for issue types across multiple projects.

## Get Field Configuration Schemes

`GET /rest/api/{2-3}/fieldconfigurationscheme`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration schemes.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)


	fieldSchemes, response, err := atlassian.Issue.Field.Configuration.Scheme.Gets(context.Background(), nil, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, scheme := range fieldSchemes.Values {

		fmt.Println("-------------------")
		fmt.Println("ID ", scheme.ID)
		fmt.Println("Name ", scheme.Name)
		fmt.Println("Description ", scheme.Description)
	}

}
```
{% endcode %}

## Create Field Configuration Scheme

`POST /rest/api/{2-3}/fieldconfigurationscheme`

Creates a field configuration scheme.

{% hint style="info" %}
This operation can only create field configuration schemes used in company-managed (classic) projects.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	schemeCreated, response, err := atlassian.Issue.Field.Configuration.Scheme.Create(context.Background(), "test", "")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	fmt.Println("ID ", schemeCreated.ID)
	fmt.Println("Name ", schemeCreated.Name)
	fmt.Println("Description ", schemeCreated.Description)
}
```
{% endcode %}

## Get Field Configuration Scheme Mapping

`GET /rest/api/{2-3}/fieldconfigurationscheme/mapping`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration issue type items.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	v2 "github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	items, response, err := atlassian.Issue.Field.Configuration.Scheme.Mapping(context.Background(), []int{10000}, 0, 50)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range items.Values {
		fmt.Println(item)
	}
}
```
{% endcode %}

## Get Field Configuration Schemes by Project

`GET /rest/api/{2-3}/fieldconfigurationscheme/project`

Returns a [paginated](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#pagination) list of field configuration schemes and, for each scheme, a list of the projects that use it.

The list is sorted by field configuration scheme ID. The first item contains the list of project IDs assigned to the default field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	items, response, err := atlassian.Issue.Field.Configuration.Scheme.Project(context.Background(), []int{10003}, 0, 50)
	if err != nil {
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, item := range items.Values {
		fmt.Println(item.ProjectIds)
		fmt.Println(item.FieldConfigurationScheme)
	}
}
```
{% endcode %}

## Assign Field Configuration Scheme

`PUT /rest/api/{2-3}/fieldconfigurationscheme/project`

Assigns a field configuration scheme to a project. If the field configuration scheme ID is `null`, the operation assigns the default field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	
	payload := &models.FieldConfigurationSchemeAssignPayload{
		FieldConfigurationSchemeID: "10000",
		ProjectID:                  "10003",
	}

	response, err := atlassian.Issue.Field.Configuration.Scheme.Assign(context.Background(), payload)
	if err != nil {
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Update Field Configuration Scheme

`PUT /rest/api/{2-3}/fieldconfigurationscheme/{id}`

Updates a field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Issue.Field.Configuration.Scheme.Update(context.Background(),10002, "test updated", "")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Delete Field Configuration Scheme

`DELETE /rest/api/{2-3}/fieldconfigurationscheme/{id}`

Deletes a field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Issue.Field.Configuration.Scheme.Delete(context.Background(),10002)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Assign issue types to field configuration

`PUT /rest/api/{2-3}/fieldconfigurationscheme/{id}/mapping`

Assigns issue types to field configurations on field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.FieldConfigurationToIssueTypeMappingPayloadScheme{
		Mappings: []*models.FieldConfigurationToIssueTypeMappingScheme{
			{
				IssueTypeID:          "10002",
				FieldConfigurationID: "10003",
			},
		},
	}

	response, err := atlassian.Issue.Field.Configuration.Scheme.Link(context.Background(),10003, payload)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Remove issue types to field configuration

`POST /rest/api/{2-3}/fieldconfigurationscheme/{id}/mapping/delete`

Removes issue types from the field configuration scheme.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Issue.Field.Configuration.Scheme.Unlink(context.Background(),10003, []string{"10002"})
	if err != nil {
		log.Fatal(err)
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

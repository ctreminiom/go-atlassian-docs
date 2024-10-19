---
cover: ../.gitbook/assets/interpersonal-alchemy_2240x1090-1560x760.jpg
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
    visible: false
  pagination:
    visible: true
---

# 🗃️ Template

The Confluence REST API template section provides a flexible and dynamic way to interact programmatically with Confluence templates, enabling users to automate and streamline content creation, modification, and management. Through this API, you can:\


• **Create templates:** Automate the creation of page templates that can be reused across spaces or within a specific space, ensuring consistency and efficiency in content generation.

• **Retrieve existing templates:** Access detailed information about available templates, including their content, layout, and associated metadata.

• **Update templates:** Modify existing templates programmatically, allowing teams to keep templates up-to-date and aligned with changing documentation standards or guidelines.

• **Delete templates (TODO):** Remove outdated or unused templates, keeping the Confluence environment clean and relevant.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Integrate Confluence templates into custom workflows using the Confluence REST API to streamline content management, ensure documentation consistency, and automate tasks.

## Update content template

`PUT /wiki/rest/api/template`

Update modifies a content template. Note that blueprint templates cannot be updated through

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := confluence.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.UpdateTemplateScheme{
		TemplateID:   "272760834",
		Name:         "Template #1 - UPDATED",
		TemplateType: "page",
		Body: &models.ContentTemplateBodyCreateScheme{
			Storage: &models.ContentBodyCreateScheme{
				Value:          "<at:declarations><at:list at:name=\"variable_name\"><at:option at:value=\"\" /></at:list></at:declarations><h1>H1 UPDATED</h1><p /><p><at:var at:name=\"variable_name\" /> </p><p />",
				Representation: "storage",
			},
		},
		Description: "Template description sample",
		Labels: []models.LabelValueScheme{
			{
				Name: "template-tracking",
			},
			{
				Name: "template-tracking-new",
			},
		},
		Space: &models.SpaceScheme{
			Key: "DUMMY",
		},
	}

	templateUpdated, response, err := atlassian.Template.Update(context.Background(), payload)
	if err != nil {
		log.Println(response.Bytes.String())
		log.Println(response.Status, response.Endpoint)
		log.Fatal(err)
	}

	fmt.Println(templateUpdated.TemplateID)
	fmt.Println(templateUpdated.TemplateType)
	fmt.Println(templateUpdated.Description)
	fmt.Println(templateUpdated.Labels)
	fmt.Println(templateUpdated.Name)
	fmt.Println(templateUpdated.ReferencingBlueprint)
	fmt.Println(templateUpdated.TemplateID)
}
```
{% endcode %}

## Create content template

`POST /wiki/rest/api/template`

Create a new content template. Note: Blueprint templates cannot be created using the REST API.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := confluence.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	payload := &models.CreateTemplateScheme{
		Name:         "Template #1",
		TemplateType: "page",
		Body: &models.ContentTemplateBodyCreateScheme{
			Storage: &models.ContentBodyCreateScheme{
				Value:          "<at:declarations><at:list at:name=\"variable_name\"><at:option at:value=\"\" /></at:list></at:declarations><h1>asdsadsadsa</h1><p /><p><at:var at:name=\"variable_name\" /> </p><p />",
				Representation: "storage",
			},
		},
		Description: "Template description sample",
		Labels: []models.LabelValueScheme{
			{
				Name: "template-tracking",
			},
		},
		Space: &models.SpaceScheme{
			Key: "DUMMY",
		},
	}

	templateCreated, response, err := atlassian.Template.Create(context.Background(), payload)
	if err != nil {
		log.Println(response.Status, response.Endpoint)
		log.Fatal(err)
	}

	fmt.Println(templateCreated.TemplateID)
	fmt.Println(templateCreated.TemplateType)
	fmt.Println(templateCreated.Description)
	fmt.Println(templateCreated.Labels)
	fmt.Println(templateCreated.Name)
	fmt.Println(templateCreated.ReferencingBlueprint)
	fmt.Println(templateCreated.TemplateID)
}

```
{% endcode %}

## Get content template

`GET /wiki/rest/api/template/{contentTemplateId}`

Retrieve a content template with detailed information, including the template's name, the associated space or blueprint, the template body,

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := confluence.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	template, response, err := atlassian.Template.Get(context.Background(), "272760834")
	if err != nil {
		log.Println(response.Status, response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	fmt.Println(template.TemplateID)
	fmt.Println(template.TemplateType)
	fmt.Println(template.Description)
	fmt.Println(template.Labels)
	fmt.Println(template.Name)
	fmt.Println(template.ReferencingBlueprint)
	fmt.Println(template.TemplateID)
}

```
{% endcode %}

## Get content templates

{% synced-block url="https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb_U27oB" %}
[NoteThis feature is current...](https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb\_U27oB)
{% endsynced-block %}

## Get blueprints content

{% synced-block url="https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb_U27oB" %}
[NoteThis feature is current...](https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb\_U27oB)
{% endsynced-block %}

## Remove template

{% synced-block url="https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb_U27oB" %}
[NoteThis feature is current...](https://app.gitbook.com/o/-MWwHNnYKpjtw7-EMhwM/blocks/syb\_U27oB)
{% endsynced-block %}

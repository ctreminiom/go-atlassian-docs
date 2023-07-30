---
cover: ../../.gitbook/assets/new-rules-of-productivity_1120x545@2x-1560x760.png
coverY: 0
---

# 📢 Content

## Get content

`GET /wiki/rest/api/content`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns all content in a Confluence instance. By default, the following objects are expanded: `space`, `history`, `version`.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "os"
)

func main()  {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   instance, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   instance.Auth.SetBasicAuth(mail, token)
   instance.Auth.SetUserAgent("curl/7.54.0")


   var (
      contentID = "64290828"
      expand = []string{"any"}
      version = 1
   )

   content, response, err := instance.Content.Get(context.Background(), contentID, expand, version)
   if err != nil {

      if response != nil {
         log.Println(response.Code)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:",    response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(content)
}
```
{% endcode %}

## Create content

`POST /wiki/rest/api/content`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Creates a new piece of content or publishes an existing draft. To publish a draft, add the `id` and `status` properties to the body of the request.&#x20;

* Set the `id` to the ID of the draft and set the `status` to 'current'.&#x20;
* When the request is sent, a new piece of content will be created and the metadata from the draft will be transferred into it.&#x20;
* By default, the following objects are expanded: `space`, `history`, `version`.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var payload = &models.ContentScheme{
		Type:  "page", // Valid values: page, blogpost, comment
		Title: "Confluence Page Title",
		Space: &models.SpaceScheme{Key: "DUMMY"},
		Body: &models.BodyScheme{
			Storage: &models.BodyNodeScheme{
				Value:          "<p>This is <br/> a new page</p>",
				Representation: "storage",
			},
		},
		Ancestors: []*models.ContentScheme{
			{
				ID: "78643265",
			},
		},
	}

	newConfluence, response, err := instance.Content.Create(context.Background(), payload)
	if err != nil {

		if response != nil {
			log.Fatal(response.Code)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	log.Println("The new content has been created")
	log.Println(newConfluence.ID)
	log.Println(newConfluence.Links.Self)
	log.Println(newConfluence.Title)
	log.Println(newConfluence.Space.Name)
}
```
{% endcode %}

## Get content by ID

`GET /wiki/rest/api/content/{id}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns a single piece of content, like a page or a blog post. By default, the following objects are expanded: `space`, `history`, `version`.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "os"
)

func main()  {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   instance, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   instance.Auth.SetBasicAuth(mail, token)
   instance.Auth.SetUserAgent("curl/7.54.0")


   var (
      contentID = "64290828"
      expand = []string{"any"}
      version = 1
   )

   content, response, err := instance.Content.Get(context.Background(), contentID, expand, version)
   if err != nil {

      if response != nil {
         log.Println(response.Code)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:",    response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(content)
}
```
{% endcode %}

## Update content

`PUT /wiki/rest/api/content/{id}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Updates a piece of content. Use this method to update the title or body of a piece of content, change the status, change the parent page, and more.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var payload = &models.ContentScheme{
		Type:  "page", // Valid values: page, blogpost, comment
		Title: "Confluence Page Title - Updated",
		Body: &models.BodyScheme{
			Storage: &models.BodyNodeScheme{
				Value:          "<p>This is <br/> a new page - updated</p>",
				Representation: "storage",
			},
		},
		Version: &models.ContentVersionScheme{Number: 2},
	}

	content, response, err := instance.Content.Update(context.Background(), "64290828", payload)
	if err != nil {

		if response != nil {
			log.Println(response.Code)
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:",	 response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(content)
}
```
{% endcode %}

## Delete content

`DELETE /wiki/rest/api/content/{id}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Moves a piece of content to the space's trash or purges it from the trash, depending on the content's type and status:

* If the content's type is `page` or `blogpost` and its status is `current`, it will be trashed.
* If the content's type is `page` or `blogpost` and its status is `trashed`, the content will be purged from the trash and deleted permanently. Note, you must also set the `status` query parameter to `trashed` in your request.
* If the content's type is `comment` or `attachment`, it will be deleted permanently without being trashed.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "os"
)

func main()  {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   instance, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   instance.Auth.SetBasicAuth(mail, token)
   instance.Auth.SetUserAgent("curl/7.54.0")

   var contentID = "74219528"

   response, err := instance.Content.Delete(context.Background(), contentID, "")
   if err != nil {

      if response != nil {
         log.Println(response.Code)
      }

      log.Fatal(err)
   }

   log.Println(response)

}
```
{% endcode %}

## Get content history

`GET /wiki/rest/api/content/{id}/history`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns the most recent update for a piece of content.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "os"
)

func main()  {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   instance, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   instance.Auth.SetBasicAuth(mail, token)
   instance.Auth.SetUserAgent("curl/7.54.0")

   var (
      contentID = "76513281"
      expand = []string{"lastUpdated", "previousVersion"}
   )

   contentHistory, response, err := instance.Content.History(context.Background(), contentID, expand)
   if err != nil {

      if response != nil {
         log.Println(response.Code)
      }

      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(contentHistory)
}
```
{% endcode %}

## Search Contents by CQL

`GET /wiki/rest/api/content/search`

Returns the list of content that matches a Confluence Query Language (CQL) query. For information on CQL, see: [Advanced searching using CQL](https://developer.atlassian.com/cloud/confluence/advanced-searching-using-cql/).

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"net/http"
	"os"
)

func main()  {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	var (
		cql = "type=page"
		cqlContext = ""
		expand = []string{"childTypes.all", "metadata.labels"}
		maxResults = 50
	)

	contentPage, response, err := instance.Content.Search(context.Background(), cql, cqlContext, expand, "", maxResults)
	if err != nil {

		if response.Code == http.StatusBadRequest {
			log.Println(response.Code)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(contentPage.Links.Next)


	for _, content := range contentPage.Results {
		log.Println(content.Title, content.ID)
	}
}
```
{% endcode %}

## Archive Pages

`POST /wiki/rest/api/content/archive`

Archives a list of pages. The pages to be archived are specified as a list of content IDs. This API accepts the archival request and returns a task ID.&#x20;

{% hint style="info" %}
The archival process happens asynchronously. Use the **/longtask/** REST API to get the copy task status.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	instance, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	instance.Auth.SetBasicAuth(mail, token)
	instance.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.ContentArchivePayloadScheme{
		Pages: []*models.ContentArchiveIDPayloadScheme{
			{
				ID: 78675984,
			},
		},
	}

	task, response, err := instance.Content.Archive(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(task.ID, task.Links.Status)
}
```
{% endcode %}

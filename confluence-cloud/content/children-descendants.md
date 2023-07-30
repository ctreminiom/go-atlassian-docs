---
cover: ../../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
coverY: 0
---

# 🎎 Children/Descendants

## Get Content Children

`GET /wiki/rest/api/content/{id}/child`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Children returns a map of the direct children of a piece of content. A piece of content has different types of child content, depending on its type. These are the default parent-child content type relationships:

* `page`: child content is `page`, `comment`, `attachment`
* `blogpost`: child content is `comment`, `attachment`
* `attachment`: child content is `comment`
* `comment`: child content is `attachment`

{% hint style="info" %}
The map will always include all child content types that are valid for the content. However, if the content has no instances of a child content type, the map will contain an empty array for that child content type.
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		contentID = "203718725"
		expand    = []string{"any", "body.storage"}
		version   = 1
	)

	children, response, err := instance.Content.ChildrenDescendant.Children(context.Background(), contentID, expand, version)
	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(children)
}
```
{% endcode %}

## Get Content Children By Type

`GET /wiki/rest/api/content/{id}/child/{type}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

ChildrenByType returns all children of a given type, for a piece of content. A piece of content has different types of child content, depending on its type:

* `page`: child content is `page`, `comment`, `attachment`
* `blogpost`: child content is `comment`, `attachment`
* `attachment`: child content is `comment`
* `comment`: child content is `attachment`

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		contentID = "203718725"
		expand    = []string{"any", "body.storage"}
		version   = 1
	)

	children, response, err := instance.Content.ChildrenDescendant.ChildrenByType(context.Background(), contentID, "page", version, expand, 0, 50)
	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, page := range children.Results {
		fmt.Print(page)
	}
}
```
{% endcode %}

## Get Content Descendants

`GET /wiki/rest/api/content/{id}/descendant`

Returns a map of the descendants of a piece of content. This is similar to Get content children, except that this method returns child pages at all levels, rather than just the direct child pages.

A piece of content has different types of descendants, depending on its type:

* `page`: descendant is `page`, `comment`, `attachment`
* `blogpost`: descendant is `comment`, `attachment`
* `attachment`: descendant is `comment`
* `comment`: descendant is `attachment`

The map will always include all descendant types that are valid for the content. However, if the content has no instances of a descendant type, the map will contain an empty array for that descendant type.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var contentID = "203718717"

	contentChildren, response, err := instance.Content.ChildrenDescendant.Descendants(context.Background(), contentID, []string{"page"})
	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	// For pages
	for _, page := range contentChildren.Page.Results {
		fmt.Println(page.Version, page.ID, page.Title)
	}
}
```
{% endcode %}

## Get Content Descendants By Type

`GET /wiki/rest/api/content/{id}/descendant/{type}`

Returns all descendants of a given type, for a piece of content. This is similar to Get content children by type, except that this method returns child pages at all levels, rather than just the direct child pages.

A piece of content has different types of descendants, depending on its type:

* `page`: descendant is `page`, `comment`, `attachment`
* `blogpost`: descendant is `comment`, `attachment`
* `attachment`: descendant is `comment`
* `comment`: descendant is `attachment`

Custom content types that are provided by apps can also be returned.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
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

	var (
		contentID = "203718717"
		expand    = []string{"any", "body.storage"}
	)

	contentChildren, response, err := instance.Content.ChildrenDescendant.DescendantsByType(
		context.Background(),
		contentID,
		"page",
		"all", expand,
		0,
		50)

	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, page := range contentChildren.Results {
		fmt.Print(page)
	}
}
```
{% endcode %}

## Copy Page Hierarchy

`POST /wiki/rest/api/content/{id}/pagehierarchy/copy`

CopyHierarchy allows the copying of an entire hierarchy of pages and their associated properties, permissions and attachments.&#x20;

The id path parameter refers to the content id of the page to copy, and the new parent of this copied page is defined using the destinationPageId in the request body. The titleOptions object defines the rules of renaming page titles during the copy; for example, search and replace can be used in conjunction to rewrite the copied page titles.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	var contentID = "203718717"

	options := &models.CopyOptionsScheme{
		CopyAttachments:    true,
		CopyPermissions:    true,
		CopyProperties:     true,
		CopyLabels:         true,
		CopyCustomContents: true,
		DestinationPageID:  "203718717",
		TitleOptions: &models.CopyTitleOptionScheme{
			Prefix:  "copied-",
			Replace: "",
			Search:  "",
		},
	}

	task, response, err := instance.Content.ChildrenDescendant.CopyHierarchy(context.Background(), contentID, options)
	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Println(task.ID)
}
```
{% endcode %}

## Copy Single Page

`POST /wiki/rest/api/content/{id}/copy`

CopyPage copies a single page and its associated properties, permissions, attachments, and custom contents. The `id` path parameter refers to the content ID of the page to copy. The target of the page to be copied is defined using the `destination` in the request body and can be one of the following types.

* `space`: page will be copied to the specified space as a root page on the space
* `parent_page`: page will be copied as a child of the specified parent page
* `existing_page`: page will be copied and replace the specified page

By default, the following objects are expanded: `space`, `history`, `version`.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
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

	var contentID = "203718717"

	options := &models.CopyOptionsScheme{
		CopyAttachments:    true,
		CopyPermissions:    true,
		CopyProperties:     true,
		CopyLabels:         true,
		CopyCustomContents: true,
		TitleOptions:       nil,
		Destination: &models.CopyPageDestinationScheme{
			Type:  "parent_page",
			Value: "203718717",
		},
		PageTitle: "TEST - COPY",
	}

	contentCloned, response, err := instance.Content.ChildrenDescendant.CopyPage(context.Background(), contentID, nil, options)
	if err != nil {

		if response != nil {
			log.Println(response.Code)
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	fmt.Print(contentCloned.Title)
	fmt.Print(contentCloned.ID)
}
```
{% endcode %}

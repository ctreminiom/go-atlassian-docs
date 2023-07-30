---
cover: ../../.gitbook/assets/artboard-2@3x-1560x760.png
coverY: 0
---

# 💾 Space

## Get content for space

`GET /wiki/rest/api/space/{spaceKey}/content`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns all content in a space. The returned content is grouped by type (pages then blogposts), then ordered by content ID in ascending order.

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
      spaceKey = "DUMMY"
      depth = "all"
      expand = []string{"operations"}
      startAt = 0
      maxResults = 50
   )

   contents, response, err := instance.Space.Content(context.Background(), spaceKey, depth, expand, startAt, maxResults)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)


   log.Println(contents.Links)
   log.Println(contents.Page)
}
```
{% endcode %}

## Get content by type for space

`GET /wiki/rest/api/space/{spaceKey}/content/{type}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns all content of a given type, in a space. The returned content is ordered by content ID in ascending order.

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
      spaceKey = "DUMMY"
      contentType = "page"
      depth = "all"
      expand = []string{"operations"}
      startAt = 0
      maxResults = 50
   )

   contents, response, err := instance.Space.ContentByType(context.Background(), spaceKey, contentType, depth, expand, startAt, maxResults)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)


   for _, content := range contents.Results {
      log.Println(content.ID, content.Title)
   }
}
```
{% endcode %}

## Create space

`POST /wiki/rest/api/space`

Creates a new space. Note, currently you cannot set space labels when creating a space.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	var payload = &models.CreateSpaceScheme{
		Key:              "DUM",
		Name:             "Dum Confluence Space",
		Description:      &models.CreateSpaceDescriptionScheme{
			Plain: &models.CreateSpaceDescriptionPlainScheme{
				Value:          "Confluence Space Description Sample",
				Representation: "plain",
			},
		},
		AnonymousAccess:  true,
		UnlicensedAccess: false,
	}

	space, response, err := instance.Space.Create(context.Background(), payload, false)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(space)
}
```
{% endcode %}

## Delete space

`DELETE /wiki/rest/api/space/{spaceKey}`

Deletes a space. Note, the space will be deleted in a long running task. Therefore, the space may not be deleted yet when this method has returned. Clients should poll the status link that is returned in the response until the task completes.

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

   task, response, err := instance.Space.Delete(context.Background(), "DS")
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   log.Println(task.ID)
   log.Println(task.Links.Status)
}
```
{% endcode %}

## Get space

`GET /wiki/rest/api/space/{spaceKey}`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns a space. This includes information like the name, description, and permissions, but not the content in the space.

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

   space, response, err := instance.Space.Get(context.Background(), "DUMMY", []string{"operations"})
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println("Endpoint:", response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)
   log.Println(space)
}
```
{% endcode %}

## Get Spaces

`GET /wiki/rest/api/space`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns all spaces. The returned spaces are ordered alphabetically in ascending order by space key.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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
		options = &models.GetSpacesOptionScheme{
			SpaceKeys:       nil,
			SpaceIDs:        []int{80838666, 196613},
			SpaceType:       "",
			Status:          "",
			Labels:          nil,
			Favorite:        false,
			FavoriteUserKey: "",
			Expand:          nil,
		}
		startAt = 0
		maxResults = 25
	)

	spaces, response, err := instance.Space.Gets(context.Background(), options, startAt, maxResults)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, space := range spaces.Results {
		log.Println(space.Name, space.Key, space.ID)
	}
}
```
{% endcode %}

## Update space

`PUT /wiki/rest/api/space/{spaceKey}`

Updates the name, description, or homepage of a space.

* For security reasons, permissions cannot be updated via the API and must be changed via the user interface instead.
* Currently, you cannot set space labels when updating a space.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
import (
	"context"
	"github.com/ctreminiom/go-atlassian/confluence"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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
		spaceKey = "DUMMY"
		payload = &#x26;models.UpdateSpaceScheme{
			Name:        "DUMMY Space - Updated",
			Description: &#x26;models.CreateSpaceDescriptionScheme{
				Plain: &#x26;models.CreateSpaceDescriptionPlainScheme{
					Value:          "Dummy Space - Description - Updated",
					Representation: "plain",
				},
			},
			Homepage:    &#x26;models.UpdateSpaceHomepageScheme{ID: "65798145"},
		}
	)

	spaceUpdated, response, err := instance.Space.Update(context.Background(), spaceKey, payload)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
		}
		log.Println("Endpoint:", response.Endpoint)
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)
	log.Println(spaceUpdated)
}
</code></pre>

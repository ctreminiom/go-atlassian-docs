---
cover: ../../.gitbook/assets/growthgauntletcoverillo.jpg
coverY: 0
---

# 🚩 Labels

## Get labels for content

`GET /wiki/rest/api/content/{id}/label`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns the labels on a piece of content.

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

   atlassian, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   var (
      contentID = "80412692"
      prefix = ""
      startAt = 0
      maxResults = 50
   )

   labels, response, err := atlassian.Content.Label.Gets(context.Background(), contentID, prefix, startAt, maxResults)
   if err != nil {
      log.Println(response.Endpoint)
      log.Println(response.Bytes.String())
      log.Fatal(err)
   }


   for _, label := range labels.Results {
      log.Println(label)
   }
}
```
{% endcode %}

## Add labels to content

`POST /wiki/rest/api/content/{id}/label`

Adds labels to a piece of content. It does not modify the existing labels.

* Labels can also be added when creating content ([Create content](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-labels/)).
* Labels can be updated when updating content ([Update content](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-labels/)). This will delete the existing labels and replace them with the labels in the request.

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

	atlassian, err := confluence.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)
	atlassian.Auth.SetUserAgent("curl/7.54.0")

	var (
		contentID = "80412692"
		payload = []*models.ContentLabelPayloadScheme{
			{
				Prefix: "global",
				Name:   "label-02",
			},
		}
	)

	labels, response, err := atlassian.Content.Label.Add(context.Background(), contentID, payload, false)
	if err != nil {
		log.Println(response.Endpoint)
		log.Println(response.Bytes.String())
		log.Fatal(err)
	}

	for _, label := range labels.Results {
		log.Println(label)
	}
}
```
{% endcode %}

## Remove label from content

`DELETE /wiki/rest/api/content/{id}/label/{label}`

Removes a label from a piece of content. This is similar to [Remove label from content using query parameter](https://developer.atlassian.com/cloud/confluence/rest/api-group-content-labels/) except that the label name is specified via a path parameter.

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

   atlassian, err := confluence.New(nil, host)
   if err != nil {
      log.Fatal(err)
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")


   response, err := atlassian.Content.Label.Remove(context.Background(), "80412692", "label-02")
   if err != nil {
      log.Println(response.Endpoint)
      log.Println(response.Bytes.String())
      log.Fatal(err)
   }
}
```
{% endcode %}

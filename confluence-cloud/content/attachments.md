---
cover: ../../.gitbook/assets/mb-personalities_1120x545-@2xcompressed-1560x760.png
coverY: 0
---

# 📂 Attachments

## Get attachments

`GET /wiki/rest/api/content/{id}/child/attachment`

{% hint style="info" %}
Deprecated, use [Confluence's v2 API](../v2/).
{% endhint %}

Returns the attachments for a piece of content. By default, the following objects are expanded: `metadata`.

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

	var options = &models.GetContentAttachmentsOptionsScheme{
		Expand:    nil,
		FileName:  "",
		MediaType: "",
	}

	attachments, response, err := instance.Content.Attachment.Gets(context.Background(), "76513281", 0, 50, options)
	if err != nil {

		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.CodeCode)
			}
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, attachment := range attachments.Results {
		log.Println(attachment.Metadata.MediaType, attachment.Title)
	}
}

```
{% endcode %}

## Create or update attachment

`PUT /wiki/rest/api/content/{id}/child/attachment`

Adds an attachment to a piece of content. If the attachment already exists for the content, then the attachment is updated (i.e. a new version of the attachment is created).

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
   "os"
   "path/filepath"
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


   //filepath.Abs("jira/mocks/image.png")

   absolutePath, err := filepath.Abs("confluence/mocks/mock.png")
   if err != nil {
      log.Fatal(err)
   }

   log.Println("Using the path", absolutePath)

   reader, err := os.Open(absolutePath)
   if err != nil {
      log.Fatal(err)
   }

   defer reader.Close()

   var (
      attachmentID = "76513281"
      fileName = "mock.png"
   )

   attachmentsPage, response, err := instance.Content.Attachment.CreateOrUpdate(context.Background(), attachmentID, "", fileName, reader)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println(response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   for _, attachment := range attachmentsPage.Results {
      log.Println(attachment.ID, attachment.Title)
   }
}
```
{% endcode %}

## Create attachment

`POST /wiki/rest/api/content/{id}/child/attachment`

Adds an attachment to a piece of content. This method only adds a new attachment. If you want to update an existing attachment, use [Create or update attachments](https://developer.atlassian.com/cloud/confluence/rest/api-group-content---attachments/).\
package main

{% code fullWidth="true" %}
```go

import (
   "context"
   "github.com/ctreminiom/go-atlassian/confluence"
   "log"
   "net/http"
   "os"
   "path/filepath"
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

   absolutePath, err := filepath.Abs("confluence/mocks/mock.png")
   if err != nil {
      log.Fatal(err)
   }

   log.Println("Using the path", absolutePath)

   reader, err := os.Open(absolutePath)
   if err != nil {
      log.Fatal(err)
   }

   defer reader.Close()

   var (
      attachmentID = "76513281"
      fileName = "mock-00.png"
   )

   attachmentsPage, response, err := instance.Content.Attachment.Create(context.Background(), attachmentID, "", fileName, reader)
   if err != nil {

      if response != nil {
         if response.Code == http.StatusBadRequest {
            log.Println(response.Code)
         }
      }
      log.Println(response.Endpoint)
      log.Fatal(err)
   }

   log.Println("Endpoint:", response.Endpoint)
   log.Println("Status Code:", response.Code)

   for _, attachment := range attachmentsPage.Results {
      log.Println(attachment.ID, attachment.Title)
   }
}
```
{% endcode %}

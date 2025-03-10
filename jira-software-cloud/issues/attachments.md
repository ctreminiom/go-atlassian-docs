---
cover: ../../.gitbook/assets/blog-cmpt-migrates-hero@2x-1560x760.png
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

# 📎 Attachments

## Get Jira attachment settings

`GET /rest/api/{2-3}/attachment/meta`

Returns the attachment settings, that is, whether attachments are enabled and the maximum attachment size allowed, the method returns the following information:

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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	settings, response, err := atlassian.Issue.Attachment.Settings(context.Background())
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("settings", settings.Enabled, settings.UploadLimit)
}
```
{% endcode %}

## Get attachment metadata

`GET /rest/api/{2-3}/attachment/{id}`

Returns the metadata for an attachment. Note that the attachment itself is not returned, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/davecgh/go-spew/spew"
	"log"
	"os"
)

func main() {

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

	metadata, response, err := atlassian.Issue.Attachment.Metadata(context.Background(), "10016")
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	spew.Dump(metadata)
}
```
{% endcode %}

## Get all metadata for an expanded attachment

`GET /rest/api/{2-3}/attachment/{id}/expand/human`

{% hint style="warning" %}
This is an experimental endpoint
{% endhint %}

Returns the metadata for the contents of an attachment, if it is an archive, and metadata for the attachment itself.&#x20;

For example, if the attachment is a ZIP archive, then information about the files in the archive is returned and metadata for the ZIP archive. Currently, only the ZIP archive format is supported, the method returns the following information:

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"github.com/davecgh/go-spew/spew"
	"log"
	"os"
)

func main() {

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

	humanMetadata, response, err := atlassian.Issue.Attachment.Human(context.Background(), "10016")
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	spew.Dump(humanMetadata)
}
```
{% endcode %}

## Delete attachment

`DELETE /rest/api/{2-3}/attachment/{id}`

Deletes an attachment from an issue, the method returns the following information:

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

	atlassian, err := v2.New(nil, host)
	if err != nil {
		return
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	response, err := atlassian.Issue.Attachment.Delete(context.Background(), "attachmentID")
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```
{% endcode %}

## Add attachment

`POST /rest/api/{2-3}/issue/{issueIdOrKey}/attachments`&#x20;

Adds one or more attachments to an issue. Attachments are posted as multipart/form-data ([RFC 1867](https://www.ietf.org/rfc/rfc1867.txt)).

* The request must have a `X-Atlassian-Token: no-check` header, if not it is blocked. See [Special headers](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#special-request-headers) for more information.
* The name of the multipart/form-data parameter that contains the attachments must be `file`.

{% hint style="warning" %}
It only accepts one attachment at once
{% endhint %}

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
	"path/filepath"
)

func main() {

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


	absolutePath, err := filepath.Abs("jira/mocks/image.png")
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
		issueKeyOrID = "KP-2"
		fileName = "image-mock-00.png"
	)

	attachments, response, err := atlassian.Issue.Attachment.Add(context.Background(), issueKeyOrID, fileName, reader)
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("We've found %v attachments", len(attachments))

	for _, attachment := range attachments {
		log.Println(attachment.ID, attachment.Filename)
	}
}
```
{% endcode %}

## Download Attachment

`GET /rest/api/{2-3}/attachment/content/{id}`

This endpoint returns the contents of an attachment. A `Range` header can be set to define a range of bytes within the attachment to download.&#x20;

If successful, Jira will return a response with the attachment file. You can save the file to your local filesystem or process it in your application as needed.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/v2/jira/v2"
	_ "github.com/ctreminiom/go-atlassian/v2/jira/v3"
	"log"
	"os"
)

func main() {

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

	attachmentMetadata, response, err := atlassian.Issue.Attachment.Metadata(context.Background(), "10016")
	if err != nil {
		log.Fatal(err)
	}

	response, err = atlassian.Issue.Attachment.Download(context.Background(), "10016", false)
	if err != nil {
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	if err := os.WriteFile(attachmentMetadata.Filename, response.Bytes.Bytes(), 0666); err != nil {
		log.Fatal(err)
	}
}
```
{% endcode %}

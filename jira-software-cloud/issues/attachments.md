---
description: >-
  This resource represents issue attachments and the attachment settings for
  Jira. Use it to get the metadata for an attachment, delete an attachment, and
  view the metadata for the contents of an attach
---

# 📎 Attachments

## Get Jira attachment settings

Returns the attachment settings, that is, whether attachments are enabled and the maximum attachment size allowed, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

## Get attachment metadata

Returns the metadata for an attachment. Note that the attachment itself is not returned, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	metadata, response, err := atlassian.Issue.Attachment.Metadata(context.Background(), "attachmentID")
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(metadata)
}
```

## Get all metadata for an expanded attachment

Returns the metadata for the contents of an attachment, if it is an archive, and metadata for the attachment itself. For example, if the attachment is a ZIP archive, then information about the files in the archive is returned and metadata for the ZIP archive. Currently, only the ZIP archive format is supported, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

	humanMetadata, response, err := atlassian.Issue.Attachment.Human(context.Background(), "attachmentID")
	if err != nil {
		log.Fatal(err)
		return
	}

	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(humanMetadata)
}
```

## Delete attachment

Deletes an attachment from an issue, the method returns the following information:

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

## Add attachment

&#x20;Adds one or more attachments to an issue. Attachments are posted as multipart/form-data ([RFC 1867](https://www.ietf.org/rfc/rfc1867.txt)).

* The request must have a `X-Atlassian-Token: no-check` header, if not it is blocked. See [Special headers](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#special-request-headers) for more information.
* The name of the multipart/form-data parameter that contains the attachments must be `file`.

{% hint style="warning" %}
It only accepts one attachment at once
{% endhint %}

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
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

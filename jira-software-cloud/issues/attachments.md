---
description: >-
  This resource represents issue attachments and the attachment settings for
  Jira. Use it to get the metadata for an attachment, delete an attachment, and
  view the metadata for the contents of an attach
---

# 📎 Attachments

## Get Jira attachment settings

Returns the attachment settings, that is, whether attachments are enabled and the maximum attachment size allowed, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `AttachmentSettingScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |

### Example

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

	settings, response, err := atlassian.Issue.Attachment.Settings(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println("settings", settings.Enabled, settings.UploadLimit)
}

```

## Get attachment metadata

Returns the metadata for an attachment. Note that the attachment itself is not returned, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | An `AttachmentMetadataScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| attachmentID | the attachment ID |

### Example

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

	metadata, response, err := atlassian.Issue.Attachment.Metadata(context.Background(), "attachmentID")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(metadata)
	return
}

```

### AttachmentMetadataScheme

```go
type AttachmentMetadataScheme struct {
	ID       int    `json:"id"`
	Self     string `json:"self"`
	Filename string `json:"filename"`
	Author   struct {
		Self       string `json:"self"`
		Key        string `json:"key"`
		AccountID  string `json:"accountId"`
		Name       string `json:"name"`
		AvatarUrls struct {
			Four8X48  string `json:"48x48"`
			Two4X24   string `json:"24x24"`
			One6X16   string `json:"16x16"`
			Three2X32 string `json:"32x32"`
		} `json:"avatarUrls"`
		DisplayName string `json:"displayName"`
		Active      bool   `json:"active"`
	} `json:"author"`
	Created   string `json:"created"`
	Size      int    `json:"size"`
	MimeType  string `json:"mimeType"`
	Content   string `json:"content"`
	Thumbnail string `json:"thumbnail"`
}
```

## Get all metadata for an expanded attachment

Returns the metadata for the contents of an attachment, if it is an archive, and metadata for the attachment itself. For example, if the attachment is a ZIP archive, then information about the files in the archive is returned and metadata for the ZIP archive. Currently, only the ZIP archive format is supported, the method returns the following information:

| variable | description |
| :--- | :--- |
| result | A `AttachmentHumanMetadataScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| attachmentID | the attachment ID |

### Example

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

	humanMetadata, response, err := atlassian.Issue.Attachment.Human(context.Background(), "attachmentID")
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Println(humanMetadata)
}

```

### AttachmentHumanMetadataScheme

```go
type AttachmentHumanMetadataScheme struct {
   ID      int    `json:"id"`
   Name    string `json:"name"`
   Entries []struct {
      Path      string `json:"path"`
      Index     int    `json:"index"`
      Size      string `json:"size"`
      MediaType string `json:"mediaType"`
      Label     string `json:"label"`
   } `json:"entries"`
   TotalEntryCount int    `json:"totalEntryCount"`
   MediaType       string `json:"mediaType"`
}
```

## Delete attachment

Deletes an attachment from an issue, the method returns the following information:

| variable | description |
| :--- | :--- |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Params

| name | description |
| :--- | :--- |
| ctx | a context.Context instance |
| attachmentID | the attachment ID |

### Example

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

	response, err := atlassian.Issue.Attachment.Delete(context.Background(), "attachmentID")
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

## Add attachment

 Adds one or more attachments to an issue. Attachments are posted as multipart/form-data \([RFC 1867](https://www.ietf.org/rfc/rfc1867.txt)\).

* The request must have a `X-Atlassian-Token: no-check` header, if not it is blocked. See [Special headers](https://developer.atlassian.com/cloud/jira/platform/rest/v3/intro/#special-request-headers) for more information.
* The name of the multipart/form-data parameter that contains the attachments must be `file`.

{% hint style="warning" %}
It only accepts one attachment at once
{% endhint %}

The method returns the following information:

| variable | description |
| :--- | :--- |
| result | A slice of the `AttachmentScheme` struct |
| response | The HTTP callback response parsed with the endpoint used, the response bytes, the status response code, and the response headers. |
| error | An error interface if something happens. |

### Parameters

| name | description |
| :--- | :--- |
| issueKeyOrID | the issue key  |
| path | the **absolute  path** for the file to upload. |

### Example

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

	absolutePath, err := filepath.Abs("jira/mocks/image.png")
	if err != nil {
		return
	}

	attachments, response, err := atlassian.Issue.Attachment.Add("KP-1", absolutePath)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
		}
		return
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)
	log.Printf("We've found %v attachments", len(*attachments))

	for _, attachment := range *attachments {
		log.Println(attachment.ID, attachment.Filename)
	}
}

```


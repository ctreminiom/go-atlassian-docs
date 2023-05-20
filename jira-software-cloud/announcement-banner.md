# 💹 Announcement Banner

## Overview

System Administrators can configure an announcement banner to display pertinent information on all Jira pages. The banner can be used to relate important information (e.g. scheduled server maintenance, approaching project deadlines, etc.) to all users. Further, the banner visibility level can be configured to display to all users or just logged-in users.

### Get announcement banner configuration

Get returns the current announcement banner configuration.

```go
package main

import (
	"context"
	"fmt"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	banner, response, err := jira.Banner.Get(context.Background())
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("Status HTTP Response", response.Status)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	fmt.Print(banner)

	return
}
```

### Update announcement banner configuration

Update updates the announcement banner configuration.

```go
package main

import (
	"context"
	_ "github.com/ctreminiom/go-atlassian/jira/v2"
	"github.com/ctreminiom/go-atlassian/jira/v3"
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

	jira, err := v3.New(nil, host)
	if err != nil {
		return
	}

	jira.Auth.SetBasicAuth(mail, token)
	jira.Auth.SetUserAgent("curl/7.54.0")

	payload := &models.AnnouncementBannerPayloadScheme{
		IsDismissible: true,
		IsEnabled:     true,
		Message:       "This is a public, enabled, non-dismissible banner, set using the API",
		Visibility:    "public",
	}

	response, err := jira.Banner.Update(context.Background(), payload)
	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
			log.Println("Status HTTP Response", response.Status)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	return
}

```

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

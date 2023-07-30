---
cover: ../.gitbook/assets/leadership-principles_1120x545@2x-1560x760.png
coverY: 0
---

# 🔰 Label

## Get label information

`GET /wiki/rest/api/label`

Returns label information and a list of contents associated with the label.

{% code fullWidth="true" %}
```go
package main

import (
	"context"
	"fmt"
	"github.com/ctreminiom/go-atlassian/confluence"
	"log"
	"net/http"
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

	labels, response, err := instance.Label.Get(context.Background(), "test", "page", 0, 50)
	if err != nil {
		if response != nil {
			if response.Code == http.StatusBadRequest {
				log.Println(response.Code)
			}
			log.Println(response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
	log.Println("Status Code:", response.Code)

	for _, content := range labels.AssociatedContents.Results {
		fmt.Print(content)
	}

}
```
{% endcode %}

\

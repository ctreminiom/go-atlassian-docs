# 📉 Analytics

## Get views

Views get the total number of views a piece of content has.

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

	views, response, err := instance.Analytics.Get(context.Background(), "203718725", "2022-10-01")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
		}
		log.Fatal(err)
	}

	fmt.Print(views)
}
```

## Get viewers

Viewers get the total number of distinct viewers a piece of content has.

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

	views, response, err := instance.Analytics.Distinct(context.Background(), "203718725", "2022-10-01")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
		}
		log.Fatal(err)
	}

	fmt.Print(views)
}
```

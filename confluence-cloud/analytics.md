---
cover: >-
  ../.gitbook/assets/csd-222-t1illustrationrefresh-5-signs-of-a-toxic-work-culture-v4a-1560x760.png
coverY: 0
---

# 📉 Analytics

## Get views

`GET /wiki/rest/api/analytics/content/{contentId}/views`

Views get the total number of views a piece of content has.

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
{% endcode %}

## Get viewers

`GET /wiki/rest/api/analytics/content/{contentId}/viewers`

Viewers get the total number of distinct viewers a piece of content has.

<pre class="language-go" data-full-width="true"><code class="lang-go"><strong>package main
</strong>
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
</code></pre>

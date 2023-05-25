# 🪟 Space

The Confluence Cloud V2 API space endpoints are a set of new features and functionalities that have been introduced in the Confluence Cloud platform to provide users with enhanced control and management of their Confluence spaces. These endpoints allow users to perform various space-related operations programmatically, such as creating new spaces, updating space properties, retrieving space information, and managing space permissions.

## Get spaces

Bulk returns all spaces. The results will be sorted by id ascending. The number of results is limited by the `limit` parameter and additional results (if available) will be available through the `next` URL present in the `Link` response header.

<table><thead><tr><th width="154">Param</th><th width="247">Description</th><th width="119">Type</th><th>Reference</th></tr></thead><tbody><tr><td><strong>ids</strong></td><td>Filter the results to spaces based on their IDs.</td><td><code>[]int</code></td><td><code>*models.GetSpacesOptionSchemeV2.IDs</code></td></tr><tr><td><strong>keys</strong></td><td>Filter the results to spaces based on their keys.</td><td><code>[]string</code></td><td><code>*models.GetSpacesOptionSchemeV2.Keys</code></td></tr><tr><td><strong>type</strong></td><td><p>Filter the results to spaces based on their type.</p><p><br>Valid values: <strong><code>global</code></strong>, <strong><code>personal</code></strong></p></td><td><code>string</code></td><td><code>*models.GetSpacesOptionSchemeV2.Type</code></td></tr><tr><td><strong>status</strong></td><td><p>Filter the results to spaces based on their status.</p><p></p><p>Valid values: <strong><code>current</code></strong>, <strong><code>archived</code></strong></p></td><td><code>string</code></td><td><code>*models.GetSpacesOptionSchemeV2.Status</code></td></tr><tr><td><strong>labels</strong></td><td>Filter the results to spaces based on their labels.</td><td><code>[]string</code></td><td><code>*models.GetSpacesOptionSchemeV2.Labels</code></td></tr><tr><td><strong>sort</strong></td><td><p>Used to sort the result by a particular field.</p><p><br>Valid values: <strong><code>id</code></strong>, <strong><code>-id</code></strong>, <strong><code>key</code></strong>, <strong><code>-key</code></strong>, <strong><code>name</code></strong>, <strong><code>-name</code></strong></p><p><br></p></td><td><code>string</code></td><td><code>*models.GetSpacesOptionSchemeV2.Sort</code></td></tr><tr><td><strong>description-format</strong><br></td><td><p>The content format type to be returned in the <code>description</code> field of the response.</p><p></p><p>Valid values: <strong><code>plain</code></strong>, <strong><code>view</code></strong></p></td><td><code>string</code></td><td><code>*models.GetSpacesOptionSchemeV2.DescriptionFormat</code></td></tr><tr><td><strong>serialize-ids-as-strings</strong><br></td><td>Due to JavaScript's max integer representation of 2^53-1, the type of any IDs returned in the response body for this endpoint will be changed from a numeric type to a string type at the end of the deprecation period.</td><td><code>bool</code></td><td><code>*models.GetSpacesOptionSchemeV2.SerializeIDs</code></td></tr><tr><td><strong>cursor</strong></td><td>Used for pagination, this opaque cursor will be returned in the <code>next</code> </td><td><code>string</code></td><td><code>N/A</code></td></tr><tr><td><strong>limit</strong></td><td>Maximum number of spaces per result to return.</td><td><code>int</code></td><td><code>N/A</code></td></tr></tbody></table>

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
	"log"
	"net/url"
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

	options := &models.GetSpacesOptionSchemeV2{
		IDs:               nil,
		Keys:              nil,
		Type:              "",
		Status:            "",
		Labels:            nil,
		Sort:              "",
		DescriptionFormat: "",
		SerializeIDs:      false,
	}

	var cursor string
	for {

		spaces, response, err := instance.Space.Bulk(context.Background(), options, cursor, 20)
		if err != nil {
			log.Fatal(err)
		}

		for _, space := range spaces.Results {
			fmt.Println(space)
		}

		log.Println("Endpoint:", response.Endpoint)
		log.Println("Status Code:", response.Code)

		if spaces.Links.Next == "" {
			break
		}

		values, err := url.ParseQuery(spaces.Links.Next)
		if err != nil {
			log.Fatal(err)
		}

		_, containsCursor := values["cursor"]
		if containsCursor {
			cursor = values["cursor"][0]
		}
	}
}
```

## Get space by id

Get returns a specific space.

```go
package main

import (
	"context"
	"fmt"
	confluence "github.com/ctreminiom/go-atlassian/confluence/v2"
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

	space, response, err := instance.Space.Get(context.Background(), 196613, "plain")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
		}

		log.Fatal(err)
	}

	fmt.Println(space)
}
```

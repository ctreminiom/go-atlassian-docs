# 🧑🚀 Attribute

## Overview

**Attributes** are fields that let you add important information to your objects—it can really be anything that you need. You specify attributes for an object type, and then they're passed on to all child objects, requiring users to fill them in. An object type comes with a default set of attributes.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## Attribute types <a href="#addingattributestoobjecttypes-attributetypes" id="addingattributestoobjecttypes-attributetypes"></a>

You can select the attribute type, this setting determines how the attribute value should be managed (and if its allowed).

<table><thead><tr><th width="159">Attribute type</th><th width="125">Type value</th><th width="257">Additional value</th><th>Description</th></tr></thead><tbody><tr><td>Default</td><td>Text</td><td>-</td><td>Data type with text representation, often used to show normal text. Max 450 characters.</td></tr><tr><td>Boolean</td><td>-</td><td>Data type with only two possible values: true or false.</td><td></td></tr><tr><td>Integer</td><td>-</td><td><p>Commonly known as a "whole number", is a number that can be written without a fractional component. For example, 21, 4, and −2048 are integers.<br></p><p>This Attributes supports the <strong>Long Integer</strong> and will allow numbers that range from -2,147,483,648 to +2,147,483,647</p></td><td></td></tr><tr><td>Float</td><td>-</td><td>Data type representing numbers with decimals.</td><td></td></tr><tr><td>Date</td><td>-</td><td>Data type representing a Date field.</td><td></td></tr><tr><td>DateTime</td><td>-</td><td>Data type representing a Date and Time field.</td><td></td></tr><tr><td>URL</td><td>-</td><td>Data type representing an URL field. May be used for URL Ping service that pings the address every 5 minutes from the server side. Watch object to get email notifications on URL Ping Up/Down.</td><td></td></tr><tr><td>Email</td><td>-</td><td>Data type representing an email field.</td><td></td></tr><tr><td>Textarea</td><td>-</td><td><p>Data type with text representation, often used when showing large text. Use Assets rich editor to customize the content. Unlimited characters in comparison to the Text attribute.</p><p>For developers: Missing attribute beans</p><p>Here’s some info for developers trying to retrieve beans for this attribute using Java API.</p><p><a href="https://confluence.atlassian.com/servicemanagementserver/adding-attributes-to-object-types-1044784524.html">Tell me more...</a></p><ol><li></li><li></li></ol></td><td></td></tr><tr><td>Select</td><td>-</td><td>Data type to represent text values, predefined as options.</td><td></td></tr><tr><td>IP Address</td><td>-</td><td>Data type to represent IP Addresses (IPv4)</td><td></td></tr><tr><td>Object</td><td>Assets object</td><td>Reference Type</td><td>This type enables a reference to another object.</td></tr><tr><td>User</td><td>Jira group</td><td>Show on Profile</td><td>This type makes it possible to select a user from the selected Jira group and connect objects with users.</td></tr><tr><td>Confluence</td><td>Confluence instance</td><td>Confluence page</td><td>This type enables a link to a Confluence page.</td></tr><tr><td>Group</td><td><em>-</em></td><td>Show on Profile</td><td>This type makes it possible to select a Jira group and connect object(s) with user(s) in specified groups.</td></tr><tr><td>Version</td><td>Jira Project</td><td><em>-</em></td><td>This type makes it possible to reference a Jira Version from a specific Jira Project to your object(s).</td></tr><tr><td>Project</td><td><em>None</em></td><td><em>-</em></td><td>This type makes it possible to reference a Jira Project to your object(s).</td></tr><tr><td>Status</td><td>Allowed Statuses</td><td><em>-</em></td><td>This type is used to set a Status of an object. Define the statuses that should be allowed, and left empty means all statuses allowed.</td></tr></tbody></table>

### Create object type attribute

The _**Create**_ method creates a new attribute on the given object type.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId

	// Instance the Assets Cloud client
	asset, err := assets.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	asset.Auth.SetBasicAuth(mail, token)

	payload := &models.ObjectTypeAttributeScheme{
		Name: "User Responsible",
		Type: 2,
	}

	objectTypeAttribute, response, err := asset.ObjectTypeAttribute.Create(context.Background(), workSpaceID, "2", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(objectTypeAttribute)
	log.Println("Endpoint:", response.Endpoint)
}
```

### Update object type attribute

The _**Update**_ method updates an existing object type attribute

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"github.com/ctreminiom/go-atlassian/pkg/infra/models"
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

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId

	// Instance the Assets Cloud client
	asset, err := assets.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	asset.Auth.SetBasicAuth(mail, token)

	payload := &models.ObjectTypeAttributeScheme{
		Name: "User Responsible - updated",
		Type: 2,
	}

	objectTypeAttribute, response, err := asset.ObjectTypeAttribute.Update(context.Background(), workSpaceID, "2", "29", payload)
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	spew.Dump(objectTypeAttribute)
	log.Println("Endpoint:", response.Endpoint)
}
```

### Delete object type attribute

The _**Delete**_ method deletes an existing object type attribute

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/assets"
	"github.com/ctreminiom/go-atlassian/jira/sm"
	"log"
	"os"
)

func main() {

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	serviceManagement, err := sm.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	serviceManagement.Auth.SetBasicAuth(mail, token)
	serviceManagement.Auth.SetUserAgent("curl/7.54.0")

	// Get the workspace ID
	workspaces, response, err := serviceManagement.WorkSpace.Gets(context.Background())
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	workSpaceID := workspaces.Values[0].WorkspaceId

	// Instance the Assets Cloud client
	asset, err := assets.New(nil)
	if err != nil {
		log.Fatal(err)
	}

	asset.Auth.SetBasicAuth(mail, token)

	response, err = asset.ObjectTypeAttribute.Delete(context.Background(), workSpaceID, "29")
	if err != nil {
		if response != nil {
			log.Println(response.Bytes.String())
			log.Println("Endpoint:", response.Endpoint)
		}
		log.Fatal(err)
	}

	log.Println("Endpoint:", response.Endpoint)
}
```

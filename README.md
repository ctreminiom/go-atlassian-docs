# 📑 Introduction

<div align="center"><img src="https://github.com/ctreminiom/go-atlassian/assets/16035390/f73c7a54-ff48-454a-9821-f3d391ccd9d8" alt=""></div>

[![Releases](https://img.shields.io/github/v/release/ctreminiom/go-atlassian)](https://github.com/ctreminiom/go-atlassian/releases/latest) [![Testing](https://github.com/ctreminiom/go-atlassian/actions/workflows/test.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/test.yml) [![codecov](https://codecov.io/gh/ctreminiom/go-atlassian/branch/main/graph/badge.svg?token=G0KPNMTIRV)](https://codecov.io/gh/ctreminiom/go-atlassian) [![Go Reference](https://pkg.go.dev/badge/github.com/ctreminiom/go-atlassian.svg)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![Go Report Card](https://goreportcard.com/badge/ctreminiom/go-atlassian)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge_shield) [![Codacy Badge](https://app.codacy.com/project/badge/Grade/fe5c1b3c9fd64f84989ae51c42803456)](https://app.codacy.com/gh/ctreminiom/go-atlassian/dashboard?utm_source=gh\&utm_medium=referral\&utm_content=\&utm_campaign=Badge_grade) ![GitHub](https://img.shields.io/github/license/ctreminiom/go-atlassian) [![Mentioned in Awesome Go-Atlassian](https://awesome.re/mentioned-badge-flat.svg)](https://github.com/avelino/awesome-go#third-party-apis) [![OpenSSF Best Practices](https://bestpractices.coreinfrastructure.org/projects/4861/badge)](https://bestpractices.coreinfrastructure.org/projects/4861) [![OpenSSF Best Practices](https://img.shields.io/ossf-scorecard/github.com/ctreminiom/go-atlassian?label=openssf%20scorecard\&style=flat)](https://img.shields.io/ossf-scorecard/github.com/ctreminiom/go-atlassian?label=openssf%20scorecard\&style=flat) [![Documentation](https://img.shields.io/badge/%F0%9F%92%A1%20go-documentation-00ACD7.svg?style=flat)](https://docs.go-atlassian.io/) [![Sourcegraph](https://sourcegraph.com/github.com/ctreminiom/go-atlassian/-/badge.svg)](https://sourcegraph.com/github.com/ctreminiom/go-atlassian?badge) [![Dependency Review](https://github.com/ctreminiom/go-atlassian/actions/workflows/dependency-review.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/dependency-review.yml) [![Analysis](https://github.com/ctreminiom/go-atlassian/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/codeql-analysis.yml)

**go-atlassian** is a Go library that provides a simple and convenient way to interact with various Atlassian products' REST APIs. [Atlassian](https://developer.atlassian.com/cloud/) is a leading provider of software and tools for software development, project management, and collaboration. Some of the products that **go-atlassian** supports include Jira, Confluence, Jira Service Management, and more.

The **go-atlassian** library is designed to simplify the process of building Go applications that interact with Atlassian products. It provides a set of functions and data structures that can be used to easily send HTTP requests to the Atlassian APIs, parse the responses, and work with the data returned.

***

### 🚀Features

* Easy-to-use functions and data structures that abstract away much of the complexity of working with the APIs.
* Comprehensive support for various Atlassian products' APIs.
* Support for common operations like creating, updating, and deleting entities in Atlassian products.
* Active development and maintenance by the community, with regular updates and bug fixes.
* Comprehensive [documentation](https://docs.go-atlassian.io/jira-software-cloud/introduction) and examples to help developers get started with using the library.

***

### 📁 Installation

If you do not have [Go](https://golang.org/) installed yet, you can find installation instructions [here](https://golang.org/doc/install). Please note that the package requires Go version 1.20 or later for module support.

To pull the most recent version of **go-atlassian**, use `go get`.

```
go get github.com/ctreminiom/go-atlassian/v2
```

***

### 📪 Packages

Then import the package into your project as you normally would. You can import the following packages:

<table data-full-width="true"><thead><tr><th>Module</th><th>Path</th><th>URL's</th></tr></thead><tbody><tr><td>Jira v2</td><td><code>github.com/ctreminiom/go-atlassian/v2/jira/v2</code></td><td><a href="https://docs.go-atlassian.io/jira-software-cloud/introduction">Getting Started</a></td></tr><tr><td>Jira v3</td><td><code>github.com/ctreminiom/go-atlassian/v2/jira/v3</code></td><td><a href="https://docs.go-atlassian.io/jira-software-cloud/introduction">Getting Started</a></td></tr><tr><td>Jira Software Agile</td><td><code>github.com/ctreminiom/go-atlassian/v2/jira/agile</code></td><td><a href="https://docs.go-atlassian.io/jira-agile/introduction">Getting Started</a></td></tr><tr><td>Jira Service Management</td><td><code>github.com/ctreminiom/go-atlassian/v2/jira/sm</code></td><td><a href="https://docs.go-atlassian.io/jira-service-management/introduction">Getting Started</a></td></tr><tr><td>Jira Assets</td><td><code>github.com/ctreminiom/go-atlassian/v2/assets</code></td><td><a href="https://docs.go-atlassian.io/jira-assets/overview">Getting Started</a></td></tr><tr><td>Confluence</td><td><code>github.com/ctreminiom/go-atlassian/v2/confluence</code></td><td><a href="https://docs.go-atlassian.io/confluence-cloud/introduction">Getting Started</a></td></tr><tr><td>Confluence v2</td><td><code>github.com/ctreminiom/go-atlassian/v2/confluence/v2</code></td><td><a href="https://docs.go-atlassian.io/confluence-cloud/v2/introduction">Getting Started</a></td></tr><tr><td>Admin Cloud</td><td><code>github.com/ctreminiom/go-atlassian/v2/admin</code></td><td><a href="https://docs.go-atlassian.io/atlassian-admin-cloud/overview">Getting Started</a></td></tr><tr><td>Bitbucket Cloud <em>(In Progress)</em><br></td><td><code>github.com/ctreminiom/go-atlassian/v2/bitbucket</code></td><td><a href="https://docs.go-atlassian.io/bitbucket-cloud/introduction">Getting Started</a></td></tr></tbody></table>

***

### 🔨 Usage

Before using the **go-atlassian** package, you need to have an Atlassian API key. If you do not have a key yet, you can sign up [here](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).

Create a client with your instance host and access token to start communicating with the Atlassian API's. In this example, we're going to instance a new Confluence Cloud client.

```go
instance, err := confluence.New(nil, "INSTANCE_HOST")
if err != nil {
    log.Fatal(err)
}
instance.Auth.SetBasicAuth("YOUR_CLIENT_MAIL", "YOUR_APP_ACCESS_TOKEN")
```

If you need to use a preconfigured HTTP client, simply pass its address to the `New` function.

```go
transport := http.Transport{
	Proxy: http.ProxyFromEnvironment,
	Dial: (&net.Dialer{
		// Modify the time to wait for a connection to establish
		Timeout:   1 * time.Second,
		KeepAlive: 30 * time.Second,
	}).Dial,
	TLSHandshakeTimeout: 10 * time.Second,
}
client := http.Client{
	Transport: &transport,
	Timeout:   4 * time.Second,
}
instance, err := confluence.New(&client, "INSTANCE_HOST")
if err != nil {
	log.Fatal(err)
}
instance.Auth.SetBasicAuth("YOUR_CLIENT_MAIL", "YOUR_APP_ACCESS_TOKEN")
```

### ☕Cookbooks

For detailed examples and usage of the go-atlassian library, please refer to our [**Cookbook**](https://docs.go-atlassian.io/cookbooks). This section provides step-by-step guides and code samples for common tasks and scenarios.

***

### 🌍 Services

The library uses the services interfaces to provide a modular and flexible way to interact with Atlassian products' REST APIs. It defines a set of services interfaces that define the functionality of each API, and then provides implementations of those interfaces that can be used to interact with the APIs.

```go
// BoardConnector represents the Jira boards.
// Use it to search, get, create, delete, and change boards.
type BoardConnector interface {
	Get(ctx context.Context, boardID int) (*model.BoardScheme, *model.ResponseScheme, error)
	Create(ctx context.Context, payload *model.BoardPayloadScheme) (*model.BoardScheme, *model.ResponseScheme, error)
	Filter(ctx context.Context, filterID, startAt, maxResults int) (*model.BoardPageScheme, *model.ResponseScheme, error)
	Backlog(ctx context.Context, boardID int, opts *model.IssueOptionScheme, startAt, maxResults int) (*model.BoardIssuePageScheme, *model.ResponseScheme, error)
	Configuration(ctx context.Context, boardID int) (*model.BoardConfigurationScheme, *model.ResponseScheme, error)
	Epics(ctx context.Context, boardID, startAt, maxResults int, done bool) (*model.BoardEpicPageScheme, *model.ResponseScheme, error)
	IssuesWithoutEpic(ctx context.Context, boardID int, opts *model.IssueOptionScheme, startAt, maxResults int) (
		*model.BoardIssuePageScheme, *model.ResponseScheme, error)
	IssuesByEpic(ctx context.Context, boardID, epicID int, opts *model.IssueOptionScheme, startAt, maxResults int) (
		*model.BoardIssuePageScheme, *model.ResponseScheme, error)
	Issues(ctx context.Context, boardID int, opts *model.IssueOptionScheme, startAt, maxResults int) (*model.BoardIssuePageScheme,
		*model.ResponseScheme, error)
	Move(ctx context.Context, boardID int, payload *model.BoardMovementPayloadScheme) (*model.ResponseScheme, error)
	Projects(ctx context.Context, boardID, startAt, maxResults int) (*model.BoardProjectPageScheme, *model.ResponseScheme, error)
	Sprints(ctx context.Context, boardID, startAt, maxResults int, states []string) (*model.BoardSprintPageScheme,
		*model.ResponseScheme, error)
	IssuesBySprint(ctx context.Context, boardID, sprintID int, opts *model.IssueOptionScheme, startAt, maxResults int) (
		*model.BoardIssuePageScheme, *model.ResponseScheme, error)
	Versions(ctx context.Context, boardID, startAt, maxResults int, released bool) (*model.BoardVersionPageScheme,
		*model.ResponseScheme, error)
	Delete(ctx context.Context, boardID int) (*model.ResponseScheme, error)
	Gets(ctx context.Context, opts *model.GetBoardsOptions, startAt, maxResults int) (*model.BoardPageScheme,
		*model.ResponseScheme, error)
}
```

Each service interface includes a set of methods that correspond to the available endpoints in the corresponding API. For example, the `IssueService` interface includes methods like `Create`, `Update`, and `Get` that correspond to the `POST`, `PUT`, and `GET` endpoints in the Jira Issues API.

***

### 🎉 Implementation

Behind the scenes, the `Create` method on the `IssueService` interface is implemented by the `issueService.Create` function in the go-atlassian library. This function sends an HTTP request to the relevant endpoint in the Jira Issues API, using the credentials and configuration provided by the client, and then parses the response into a usable format.

Here's a little example about how to get the issue transitions using the Issue service.

```go
ctx := context.Background()
issueKey := "KP-2"
expand := []string{"transitions"}
issue, response, err := atlassian.Issue.Get(ctx,issueKey, nil, expand)
if err != nil {
	log.Fatal(err)
}
log.Println(issue.Key)
for _, transition := range issue.Transitions {
	log.Println(transition.Name, transition.ID, transition.To.ID, transition.HasScreen)
}
```

The rest of the service functions work much the same way; they are concise and behave as you would expect. The [documentation](https://docs.go-atlassian.io/) contains several examples on how to use each service function.

### 📪Call a RAW API Endpoint

If you need to interact with an Atlassian API endpoint that hasn't been implemented in the `go-atlassian` library yet, you can make a custom API request using the built-in `Client.Call` method to execute raw HTTP requests.

> Please raise an issue in order to implement the endpoint

```go
package main  
  
import (  
    "context"  
    "fmt" 
    "github.com/ctreminiom/go-atlassian/v2/jira/v3" 
    "log" 
    "net/http" 
    "os"
 )  
  
type IssueTypeMetadata struct {  
    IssueTypes []struct {  
       ID          string `json:"id"`  
  Name        string `json:"name"`  
  Description string `json:"description"`  
  } `json:"issueTypes"`  
}  
  
func main() {  
  
    var (  
       host  = os.Getenv("SITE")  
       mail  = os.Getenv("MAIL")  
       token = os.Getenv("TOKEN")  
    )  
  
    atlassian, err := v3.New(nil, host)  
    if err != nil {  
       log.Fatal(err)  
    }  
  
    atlassian.Auth.SetBasicAuth(mail, token)  
  
    // Define the RAW endpoint  
    apiEndpoint := "rest/api/3/issue/createmeta/KP/issuetypes"  
  
    request, err := atlassian.NewRequest(context.Background(), http.MethodGet, apiEndpoint, "", nil)  
    if err != nil {  
       log.Fatal(err)  
    }  
  
    customResponseStruct := new(IssueTypeMetadata)  
    response, err := atlassian.Call(request, &customResponseStruct)  
    if err != nil {  
       log.Fatal(err)  
    }  
  
    fmt.Println(response.Status)  
}
```

***

### ✍️ Contributions

If you would like to contribute to this project, please adhere to the following guidelines.

* Submit an issue describing the problem.
* Fork the repo and add your contribution.
* Follow the basic Go conventions found [here](https://github.com/golang/go/wiki/CodeReviewComments).
* Create a pull request with a description of your changes.

Again, contributions are greatly appreciated!

***

### 💡 Inspiration

The project was created with the purpose to provide a unique point to provide an interface for interacting with Atlassian products.

This module is highly inspired by the Go library https://github.com/andygrunwald/go-jira but focused on Cloud solutions.

The library shares many similarities with go-jira, including its use of service interfaces to define the functionality of each API, its modular and flexible approach to working with Atlassian products' API's. However, go-atlassian also adds several new features and improvements that are not present in go-jira.

Despite these differences, go-atlassian remains heavily inspired by go-jira, and many of the core design principles and patterns used in go-jira can be found in go-atlassian as well.

***

### 📝 License

Copyright © 2023 [Carlos Treminio](https://github.com/ctreminiom). This project is [MIT](https://opensource.org/licenses/MIT) licensed.

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge_large)

***

### 🤝 Special Thanks

We would like to extend our sincere thanks to the following sponsors for their generous support:

* [Atlassian](https://www.atlassian.com/) for providing us Atlassian Admin/Jira/Confluence Standard licenses.
* [JetBrains](https://www.jetbrains.com/) for providing us with free licenses of [GoLand](https://www.jetbrains.com/pycharm/)
* [GitBook](https://www.gitbook.com/) for providing us non-profit / open-source plan so hence I would like to express my thanks here.

&#x20;

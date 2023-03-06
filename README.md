# 📑 Introduction

[![](https://img.shields.io/github/v/release/ctreminiom/go-atlassian)](https://github.com/ctreminiom/go-atlassian/releases/latest) [![](https://pkg.go.dev/badge/github.com/ctreminiom/go-atlassian?utm\_source=godoc)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![](https://goreportcard.com/badge/ctreminiom/go-atlassian)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge\_shield) [![](https://codecov.io/gh/ctreminiom/go-atlassian/branch/main/graph/badge.svg?token=G0KPNMTIRV)](https://codecov.io/gh/ctreminiom/go-atlassian) [![](https://app.codacy.com/project/badge/Grade/fe5c1b3c9fd64f84989ae51c42803456)](https://www.codacy.com/gh/ctreminiom/go-atlassian/dashboard?utm\_source=github.com\&utm\_medium=referral\&utm\_content=ctreminiom/go-atlassian\&utm\_campaign=Badge\_Grade) [![](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ctreminiom/go-atlassian/blob/master/LICENSE)  [![](https://img.shields.io/badge/%F0%9F%92%A1%20go-documentation-00ACD7.svg?style=flat)](https://docs.go-atlassian.io/) [![](https://bestpractices.coreinfrastructure.org/projects/4861/badge)](https://bestpractices.coreinfrastructure.org/projects/4861)

**go-atlassian** is a Go library that provides a simple and convenient way to interact with various Atlassian products' REST APIs. <mark style="color:blue;">**Atlassian**</mark> is a leading provider of software and tools for software development, project management, and collaboration. Some of the products that **go-atlassian** supports include Jira, Confluence, Jira Service Management, and more.

The **go-atlassian** library is designed to simplify the process of building Go applications that interact with Atlassian products. It provides a set of functions and data structures that can be used to easily send HTTP requests to the Atlassian APIs, parse the responses, and work with the data returned.&#x20;

One of the advantages of using **go-atlassian** is that it abstracts away much of the complexity of working with the Atlassian REST APIs, allowing developers to focus on the logic of their application rather than the details of the API. Additionally, **go-atlassian** is well-documented and actively maintained, making it a reliable choice for integrating with Atlassian products in a Go-based project.

{% embed url="https://github.com/ctreminiom/go-atlassian" %}
GitHub Repository
{% endembed %}

## Installation

```
go get -v github.com/ctreminiom/go-atlassian
```

## Features

<table data-column-title-hidden data-view="cards"><thead><tr><th></th></tr></thead><tbody><tr><td>Easy-to-use functions and data structures that abstract away much of the complexity of working with the APIs.</td></tr><tr><td>Comprehensive support for various Atlassian products' APIs.</td></tr><tr><td>Support for common operations like creating, updating, and deleting entities in Atlassian products.</td></tr><tr><td>Active development and maintenance by the community, with regular updates and bug fixes.</td></tr><tr><td>Comprehensive documentation and examples to help developers get started with using the library.</td></tr></tbody></table>

## Packages

You can import the package you need into your project as you normally would. You can import the following packages:

<table><thead><tr><th>Product</th><th>Package</th><th data-type="content-ref"></th></tr></thead><tbody><tr><td>Jira V2</td><td><code>github.com/ctreminiom/go-atlassian/jira/v2</code></td><td><a href="./">.</a></td></tr><tr><td>Jira V3</td><td><code>github.com/ctreminiom/go-atlassian/jira/v3</code></td><td></td></tr><tr><td>Jira Agile</td><td><code>github.com/ctreminiom/go-atlassian/jira/agile</code></td><td></td></tr><tr><td>Jira ITSM</td><td><code>github.com/ctreminiom/go-atlassian/jira/sm</code></td><td></td></tr><tr><td>Confluence</td><td><code>github.com/ctreminiom/go-atlassian/confluence</code></td><td></td></tr><tr><td>Cloud Admin</td><td><code>github.com/ctreminiom/go-atlassian/admin</code></td><td></td></tr></tbody></table>

## Usage

Before using the **go-atlassian** package, you need to have an Atlassian API key. If you do not have a key yet, you can sign up [here](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).&#x20;

Create a client with your instance host and access token to start communicating with the Atlassian API's. In this example, we're going to instance a new Confluence Cloud client.

{% code lineNumbers="true" %}
```
instance, err := confluence.New(nil, "INSTANCE_HOST")
if err != nil {
    log.Fatal(err)
}

instance.Auth.SetBasicAuth("YOUR_CLIENT_MAIL", "YOUR_APP_ACCESS_TOKEN")
```
{% endcode %}

If you need to use a preconfigured HTTP client, simply pass its address to the `New` function.

{% code lineNumbers="true" %}
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
{% endcode %}

## Services

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

### Implementation

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

## Contributions

If you would like to contribute to this project, please adhere to the following guidelines.

* Submit an issue describing the problem.
* Fork the repo and add your contribution.
* Follow the basic Go conventions found [here](https://github.com/golang/go/wiki/CodeReviewComments).
* Create a pull request with a description of your changes.

Again, contributions are greatly appreciated!

## Inspiration

The project was created with the purpose to provide a unique point to provide an interface for interacting with Atlassian products. This module is highly inspired by the Go library [https://github.com/andygrunwald/go-jira](https://github.com/andygrunwald/go-jira) but focused on Cloud solutions.

The library shares many similarities with _go-jira_, including its use of service interfaces to define the functionality of each API, its modular and flexible approach to working with Atlassian products' API's. However, _go-atlassian_ also adds several new features and improvements that are not present in _go-jira_**.**

For example, _go-atlassian_ provides support for additional Atlassian products like Confluence, Jira Agile, Atlassian Admin, and Jira Service Management, while _go-jira_ is focused exclusively on Jira.&#x20;

Despite these differences, _go-atlassian_ remains heavily inspired by _go-jira_, and many of the core design principles and patterns used in _go-jira_ can be found in _go-atlassian_ as well.

Copyright © 2021 [Carlos Treminio](https://github.com/ctreminiom). This project is [MIT](https://opensource.org/licenses/MIT) licensed.

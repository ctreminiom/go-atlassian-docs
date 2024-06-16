# 📑 Introduction

<div data-full-width="true">

<figure><img src="https://github.com/ctreminiom/go-atlassian/assets/16035390/f73c7a54-ff48-454a-9821-f3d391ccd9d8" alt=""><figcaption></figcaption></figure>

</div>

[![Releases](https://camo.githubusercontent.com/89628bea84f875f4e387eea9629d5f6308152ae08a8234813c67e0335007507a/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f762f72656c656173652f637472656d696e696f6d2f676f2d61746c61737369616e)](https://github.com/ctreminiom/go-atlassian/releases/latest) [![Testing](https://github.com/ctreminiom/go-atlassian/actions/workflows/test.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/test.yml) [![codecov](https://camo.githubusercontent.com/0065bf89d9ff9ba3a261b223782cf4141045283fa19d596e617868c2ec255ad9/68747470733a2f2f636f6465636f762e696f2f67682f637472656d696e696f6d2f676f2d61746c61737369616e2f6272616e63682f6d61696e2f67726170682f62616467652e7376673f746f6b656e3d47304b504e4d54495256)](https://codecov.io/gh/ctreminiom/go-atlassian) [![Go Reference](https://camo.githubusercontent.com/03d771bf4109ba1a130755aa0ec7639bbe343a475338c14b77d2e2f0abce8b89/68747470733a2f2f706b672e676f2e6465762f62616467652f6769746875622e636f6d2f637472656d696e696f6d2f676f2d61746c61737369616e2e737667)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![Go Report Card](https://camo.githubusercontent.com/888fbf9fb9feb511d9505d1b439d530b4c9c5efc3a162230bf0770c740d230eb/68747470733a2f2f676f7265706f7274636172642e636f6d2f62616467652f637472656d696e696f6d2f676f2d61746c61737369616e)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![FOSSA Status](https://camo.githubusercontent.com/54c93439bdaf2b3a3a4f0a9cc8ef4ce783a6ab41f93077ba63898783b035d63e/68747470733a2f2f6170702e666f7373612e636f6d2f6170692f70726f6a656374732f6769742532426769746875622e636f6d253246637472656d696e696f6d253246676f2d61746c61737369616e2e7376673f747970653d736869656c64)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge\_shield) [![Codacy Badge](https://camo.githubusercontent.com/17d6f0f4246cb94f84457ddbebdd31a6f79b8fa1e99a64d52fdbb638b0b0f15d/68747470733a2f2f6170702e636f646163792e636f6d2f70726f6a6563742f62616467652f47726164652f6665356331623363396664363466383439383961653531633432383033343536)](https://app.codacy.com/gh/ctreminiom/go-atlassian/dashboard?utm\_source=gh\&utm\_medium=referral\&utm\_content=\&utm\_campaign=Badge\_grade) [![GitHub](https://camo.githubusercontent.com/66963caceb1237797cc2dc8309f8bd3bfec16a1894c38323e35716583fe4fbe0/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f637472656d696e696f6d2f676f2d61746c61737369616e)](https://camo.githubusercontent.com/66963caceb1237797cc2dc8309f8bd3bfec16a1894c38323e35716583fe4fbe0/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f637472656d696e696f6d2f676f2d61746c61737369616e) [![Mentioned in Awesome Go-Atlassian](https://camo.githubusercontent.com/9dbe56f475e0abb1d07a9a18d841378c3d40325ae7df300f29491158cf38d013/68747470733a2f2f617765736f6d652e72652f6d656e74696f6e65642d62616467652d666c61742e737667)](https://github.com/avelino/awesome-go#third-party-apis) [![OpenSSF Best Practices](https://camo.githubusercontent.com/c820f9e9737f8dbf62fa1d3a65a1b15d44097f785b6112894a3edc71528e3d3d/68747470733a2f2f626573747072616374696365732e636f7265696e6672617374727563747572652e6f72672f70726f6a656374732f343836312f6261646765)](https://bestpractices.coreinfrastructure.org/projects/4861) [![Documentation](https://camo.githubusercontent.com/aa8d63e0da45c4c228f40f18c32dd1f896c9e9d5f8debcc45f2fe8d3625e3051/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f254630253946253932254131253230676f2d646f63756d656e746174696f6e2d3030414344372e7376673f7374796c653d666c6174)](https://docs.go-atlassian.io/) [![Dependency Review](https://github.com/ctreminiom/go-atlassian/actions/workflows/dependency-review.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/dependency-review.yml) [![Analysis](https://github.com/ctreminiom/go-atlassian/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/ctreminiom/go-atlassian/actions/workflows/codeql-analysis.yml)

**go-atlassian** is a Go library that provides a simple and convenient way to interact with various Atlassian products' REST APIs. [Atlassian](https://developer.atlassian.com/cloud/) is a leading provider of software and tools for software development, project management, and collaboration. Some of the products that **go-atlassian** supports include Jira, Confluence, Jira Service Management, and more.

The **go-atlassian** library is designed to simplify the process of building Go applications that interact with Atlassian products. It provides a set of functions and data structures that can be used to easily send HTTP requests to the Atlassian APIs, parse the responses, and work with the data returned.

### 🚀Features

* Easy-to-use functions and data structures that abstract away much of the complexity of working with the APIs.
* Comprehensive support for various Atlassian products' APIs.
* Support for common operations like creating, updating, and deleting entities in Atlassian products.
* Active development and maintenance by the community, with regular updates and bug fixes.
* Comprehensive [documentation](https://docs.go-atlassian.io/jira-software-cloud/introduction) and examples to help developers get started with using the library.

### 📁 Installation

If you do not have [Go](https://golang.org/) installed yet, you can find installation instructions [here](https://golang.org/doc/install). Please note that the package requires Go version 1.17 or later for module support.

To pull the most recent version of **go-atlassian**, use `go get`.

```
go get github.com/ctreminiom/go-atlassian
```

***

### 📪 Packages

Then import the package into your project as you normally would. You can import the following packages:

| Module                  | Path                                               | URL's                                                                                |
| ----------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Jira v2                 | `github.com/ctreminiom/go-atlassian/jira/v2`       | [Getting Started](https://docs.go-atlassian.io/jira-software-cloud/introduction)     |
| Jira v3                 | `github.com/ctreminiom/go-atlassian/jira/v3`       | [Getting Started](https://docs.go-atlassian.io/jira-software-cloud/introduction)     |
| Jira Software Agile     | `github.com/ctreminiom/go-atlassian/jira/agile`    | [Getting Started](https://docs.go-atlassian.io/jira-agile/introduction)              |
| Jira Service Management | `github.com/ctreminiom/go-atlassian/jira/sm`       | [Getting Started](https://docs.go-atlassian.io/jira-service-management/introduction) |
| Jira Assets             | `github.com/ctreminiom/go-atlassian/assets`        | [Getting Started](https://docs.go-atlassian.io/jira-assets/overview)                 |
| Confluence              | `github.com/ctreminiom/go-atlassian/confluence`    | [Getting Started](https://docs.go-atlassian.io/confluence-cloud/introduction)        |
| Confluence v2           | `github.com/ctreminiom/go-atlassian/confluence/v2` | [Getting Started](https://docs.go-atlassian.io/confluence-cloud/v2/introduction)     |
| Admin Cloud             | `github.com/ctreminiom/go-atlassian/admin`         | [Getting Started](https://docs.go-atlassian.io/atlassian-admin-cloud/overview)       |

***

### 🔨 Usage

Before using the **go-atlassian** package, you need to have an Atlassian API key. If you do not have a key yet, you can sign up [here](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/).

Create a client with your instance host and access token to start communicating with the Atlassian API's. In this example, we're going to instance a new Confluence Cloud client.

```
instance, err := confluence.New(nil, "INSTANCE_HOST")
if err != nil {
    log.Fatal(err)
}
instance.Auth.SetBasicAuth("YOUR_CLIENT_MAIL", "YOUR_APP_ACCESS_TOKEN")
```

If you need to use a preconfigured HTTP client, simply pass its address to the `New` function.

```
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

For detailed examples and usage of the go-atlassian library, please refer to our [Cookbook](cookbooks/). This section provides step-by-step guides and code samples for common tasks and scenarios.

***

### 🌍 Services

The library uses the services interfaces to provide a modular and flexible way to interact with Atlassian products' REST APIs. It defines a set of services interfaces that define the functionality of each API, and then provides implementations of those interfaces that can be used to interact with the APIs.

```
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

```
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

This module is highly inspired by the Go library [https://github.com/andygrunwald/go-jira](https://github.com/andygrunwald/go-jira) but focused on Cloud solutions.

The library shares many similarities with go-jira, including its use of service interfaces to define the functionality of each API, its modular and flexible approach to working with Atlassian products' API's. However, go-atlassian also adds several new features and improvements that are not present in go-jira.

Despite these differences, go-atlassian remains heavily inspired by go-jira, and many of the core design principles and patterns used in go-jira can be found in go-atlassian as well.

***

### 📝 License

Copyright © 2024 [Carlos Treminio](https://github.com/ctreminiom). This project is [MIT](https://opensource.org/licenses/MIT) licensed.

[![FOSSA Status](https://camo.githubusercontent.com/225779948e4b6b38821405cda256693d1c12977647d0667a19851c34602103ee/68747470733a2f2f6170702e666f7373612e636f6d2f6170692f70726f6a656374732f6769742532426769746875622e636f6d253246637472656d696e696f6d253246676f2d61746c61737369616e2e7376673f747970653d6c61726765)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge\_large)

***

### 🤝 Special Thanks

We would like to extend our sincere thanks to the following sponsors for their generous support:

* [Atlassian](https://www.atlassian.com/) for providing us Atlassian Admin/Jira/Confluence Standard licenses.
* [JetBrains](https://www.jetbrains.com/) for providing us with free licenses of [GoLand](https://www.jetbrains.com/pycharm/)
* [GitBook](https://www.gitbook.com/) for providing us non-profit / open-source plan so hence I would like to express my thanks here.

##

##


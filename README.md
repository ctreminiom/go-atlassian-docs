---
cover: >-
  https://images.unsplash.com/photo-1517814179247-2f8e4c9288cb?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw5fHxtaW5pbWFsaXN0JTIwYmx1ZXxlbnwwfHx8fDE2MzU3NDM4OTU&ixlib=rb-1.2.1&q=85
coverY: -477.5
---

# 📑 Introduction

![](.gitbook/assets/131232958-022b0382-e6bc-42db-97b6-82fbd190e19a.png)

&#x20;[![](https://img.shields.io/github/v/release/ctreminiom/go-atlassian)](https://github.com/ctreminiom/go-atlassian/releases/latest) [![](https://pkg.go.dev/badge/github.com/ctreminiom/go-atlassian)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![](https://goreportcard.com/badge/ctreminiom/go-atlassian)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge\_shield) [![](https://codecov.io/gh/ctreminiom/go-atlassian/branch/main/graph/badge.svg?token=G0KPNMTIRV)](https://codecov.io/gh/ctreminiom/go-atlassian) [![](https://app.codacy.com/project/badge/Grade/fe5c1b3c9fd64f84989ae51c42803456)](https://www.codacy.com/gh/ctreminiom/go-atlassian/dashboard) [![](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ctreminiom/go-atlassian/blob/master/LICENSE) [![](https://img.shields.io/github/workflow/status/ctreminiom/go-atlassian/Testing?label=%F0%9F%A7%AA%20tests\&style=flat\&color=75C46B)](https://github.com/ctreminiom/go-atlassian/actions?query=workflow%3ATesting) [![](https://img.shields.io/badge/%F0%9F%92%A1%20go-documentation-00ACD7.svg?style=flat)](https://docs.go-atlassian.io) [![](https://bestpractices.coreinfrastructure.org/projects/4861/badge)](https://bestpractices.coreinfrastructure.org/projects/4861) [![chat on Discord](https://img.shields.io/discord/838149936101064724.svg?label=\&logo=discord\&logoColor=ffffff\&color=7389D8\&labelColor=6A7EC2)](https://discord.gg/yqaQFYHS)

go-atlassian is a Go module that enables the interaction with the Atlassian Cloud Services.

## ✨ Features

* Supports Jira Software v3.0 API. **(88% mapped)**
* Interacts with the Jira Agile entities such as: Epics, Board, Backlog, Ranks, etc.
* Interacts with the Jira Service Management entities.
* Manages the Atlassian Admin Cloud organizations.
* Manages the Atlassian Admin SCIM workflow.
* Checks Confluence Cloud content permissions.
* CRUD Confluence Cloud content (page, blogpost, comment, question).
* Add attachment into Confluence Cloud contents.
* Search contents and spaces.
* Support the Atlassian Document Format (ADF). &#x20;
* 100% of code coverage
* Every method has their corresponding example documented.
* 3036 Unit Test Cases created and passed.

## 🔰 Installation

Make sure you have Go installed (download). Version `1.13` or higher is required.

```bash
## Jira Software Cloud / Service Management Cloud / Jira Agile Cloud
$ go get -u -v github.com/ctreminiom/go-atlassian/jira/

## Atlassian Cloud Admin
$ go get -u -v github.com/ctreminiom/go-atlassian/admin/

## Confluence Cloud
$ go get -u -v github.com/ctreminiom/go-atlassian/confluence/
```

## 📓 Documentation

Documentation is hosted live at [https://docs.go-atlassian.io/](https://docs.go-atlassian.io)

## 📝 Usage

More examples in `jira/examples` `admin/examples` `jira/sm/examples` directories. Here's a short example of how to get a Jira Issue:

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

    issue, response, err := atlassian.Issue.Get(context.Background(), "KP-12", nil, []string{"transitions"})
    if err != nil {
        if response != nil {
            log.Println("Response HTTP Response", response.Bytes.String())
            log.Println(response.Code)
        }
        log.Fatal(err)
    }

    log.Println("Response HTTP Code", response.Code)
    log.Println("HTTP Endpoint Used", response.Endpoint)

    log.Println(issue.Key)
    log.Println(issue.Fields.Reporter.AccountID)

    for _, transition := range issue.Transitions {
        log.Println(transition.Name, transition.ID, transition.To.ID, transition.HasScreen)
    }

}
```

## 🧳 JetBrains OS licenses

`go-atlassian` had been being developed with GoLand under the **free JetBrains Open Source license(s)** granted by JetBrains s.r.o., hence I would like to express my thanks here.

## 🪐 GitBook Host

`go-atlassian` documentation is hosted using the GitBook non-profit / open-source plan so hence I would like to express my thanks here.

## ⭐️ Project assistance

If you want to say **thank you** or/and support active development of `go-atlassian`:

* Add a [GitHub Star](https://github.com/ctreminiom/go-atlassian) to the project.
* Write interesting articles about project on [Dev.to](https://dev.to), [Medium](https://medium.com) or personal blog.
* Support the project by donating a cup of coffee.
* Contributions, issues and feature requests are welcome!
* Feel free to check [issues page](https://github.com/ctreminiom/go-atlassian/issues).

[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom\_images/orange\_img.png)](https://www.buymeacoffee.com/ctreminiom)

## 💡 Inspiration

The project was created with the purpose to provide a unique point to provide an interface for interacting with Atlassian products. This module is highly inspired by the Go library [https://github.com/andygrunwald/go-jira](https://github.com/andygrunwald/go-jira) but focused on Cloud solutions.

## 🧪 Run Test Cases

```bash
go test -v ./...
```

## 📝 License

Copyright © 2021 [Carlos Treminio](https://github.com/ctreminiom). This project is [MIT](https://opensource.org/licenses/MIT) licensed.

[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge\_large)

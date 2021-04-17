# 🎎 Introduction

![](.gitbook/assets/go-atlassian-logo.svg)

[![](https://pkg.go.dev/badge/github.com/ctreminiom/go-atlassian?utm_source=godoc)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![](https://goreportcard.com/badge/ctreminiom/go-atlassian)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![](https://codecov.io/gh/ctreminiom/go-atlassian/branch/main/graph/badge.svg?token=G0KPNMTIRV)](https://codecov.io/gh/ctreminiom/go-atlassian) [![](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ctreminiom/go-atlassian/blob/master/LICENSE) [![](https://img.shields.io/github/workflow/status/ctreminiom/go-atlassian/Testing?label=%F0%9F%A7%AA%20tests&style=flat&color=75C46B)](https://github.com/ctreminiom/go-atlassian/actions?query=workflow%3ATesting) [![](https://img.shields.io/badge/%F0%9F%92%A1%20go-documentation-00ACD7.svg?style=flat)](https://docs.go-atlassian.io/)

go-atlassian is a library written in Go programming language that enables the interaction with the Atlassian Cloud API's. It consists of the following services that Atlassian provide us:

* Jira Software Cloud
* Jira Service Management Cloud
* Confluence Cloud
* Atlassian Access
* Opsgenie
* Trello
* Bitbucket Cloud.

{% hint style="warning" %}
Right now, the library supports the Jira Software Cloud and Jira Service Management Cloud services. This project's still in progress, and the remaining services will be mapped and documented.
{% endhint %}

## Jira Software Cloud 📘

Plan, track, and release world-class software with the \#1 software development tool used by agile teams.

### Features

* Create/Edit/Delete/View issues
* Support the Jira issue custom-fields interactions
* Manage the project screen, screen screens, issue type scheme screens, etc.
* Change the issue status, retrieve the issue changelogs, search issues based on the JQL query and more!!.

## Jira Service Management Cloud 📘

Collaborate at high-velocity, respond to business changes and deliver great customer and employee service experiences fast.

### Features

* Create/Edit/Delete/View Service Desk Organizations
* Create Request Types, Customers.
* Get the Service Desk Articles, Queues, Request Comments, Participants, etc

## Installation 📖

```bash
$ go get -u -v github.com/ctreminiom/go-atlassian/jira
```

## Usage ✒️

All interaction starts with a `jira.Client` struct. Create one with your Atlassian site host URL and a custom HTTP client if it's necessary.

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

    // You can set custom *http.Client here
    jiraCloud, err := jira.New(nil, host)
    if err != nil {
        return
    }

    jiraCloud.Auth.SetBasicAuth(mail, token)
    jiraCloud.Auth.SetUserAgent("curl/7.54.0")

    applicationRoles, response, err := jiraCloud.Role.Gets(context.Background())
    if err != nil {
        if response != nil {
            log.Println("Response HTTP Response", string(response.BodyAsBytes))
        }
        log.Fatal(err)
    }

    log.Println("Response HTTP Code", response.StatusCode)
    log.Println("HTTP Endpoint Used", response.Endpoint)

    for _, applicationRole := range *applicationRoles {
        log.Printf("Application Role Name: %v", applicationRole.Name)
        log.Printf("Application Role Key: %v", applicationRole.Key)
        log.Printf("Application Role User Count: %v", applicationRole.UserCount)
    }

}
```

## Run tests

```bash
go test -v ./...
```


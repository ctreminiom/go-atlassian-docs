---
description: >-
  This resource represents the users assigned to project roles. Use this
  resource to get, add, and remove default users from project roles. Also use
  this resource to add and remove users from a project
---

# 👨👩👧👧 Actors

## Add actors to project role

Adds actors to a project role for the project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		projectKeyOrID = "KP"
		projectRoleID  = 10005
		accountIDs     = []string{"5b86be50b8e3cb5895860d6d"}
		groupsNames    = []string{"scrum-masters"}
	)

	role, response, err := atlassian.Project.Role.Actor.Add(context.Background(),
		projectKeyOrID, projectRoleID, accountIDs, groupsNames)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	for _, actor := range role.Actors {
		log.Println(actor)
	}
}
```

## Delete actors from project role

Deletes actors from a project role for the project.

```go
package main

import (
	"context"
	"github.com/ctreminiom/go-atlassian/jira/v2"
	"log"
	"os"
)

func main() {

	/*
		----------- Set an environment variable in git bash -----------
		export HOST="https://ctreminiom.atlassian.net/"
		export MAIL="MAIL_ADDRESS"
		export TOKEN="TOKEN_API"

		Docs: https://stackoverflow.com/questions/34169721/set-an-environment-variable-in-git-bash
	*/

	var (
		host  = os.Getenv("HOST")
		mail  = os.Getenv("MAIL")
		token = os.Getenv("TOKEN")
	)

	atlassian, err := v2.New(nil, host)
	if err != nil {
		log.Fatal(err)
	}

	atlassian.Auth.SetBasicAuth(mail, token)

	var (
		projectKeyOrID = "KP"
		projectRoleID  = 10005
		accountID      = "5b86be50b8e3cb5895860d6d"
		groupName      = "scrum-masters"
	)

	response, err := atlassian.Project.Role.Actor.Delete(context.Background(),
		projectKeyOrID,
		projectRoleID,
		accountID,
		groupName,
	)

	if err != nil {
		if response != nil {
			log.Println("Response HTTP Response", response.Bytes.String())
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.Code)
	log.Println("HTTP Endpoint Used", response.Endpoint)
}
```

# 🏓 Teams

{% hint style="info" %}
This document refers to Advanced Roadmaps API's, which is a cross-project planning tool only available as part of Jira Software Cloud Premium and Enterprise.
{% endhint %}

{% hint style="warning" %}
This is an experimental API,
{% endhint %}

## Overview

Teams in Advanced Roadmaps are different from the teams found in the rest of Jira Software Cloud. In Advanced Roadmaps, they act as a label applied to issues that designates which team will eventually pick up the work on your timeline. By adding the Team field to your Jira issues, you can save this value back to your Jira issues, which makes sprint planning easier.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Since Advanced Roadmaps is a planning tool, the Team field is a way to use features like capacity management without assigning issues to individuals, which happens in sprint grooming or planning sessions.

You can also use Advanced Roadmaps' view settings to focus on work assigned to a specific team. **For example**, you can choose to color issues based on the team to which they’re assigned, group issues by team on your timeline, or hide teams from your view.

### Get Teams

`POST /rest/teams/1.0/teams/find`

Gets returns the Teams information from the _Jira Advanced Roadmaps_ application.&#x20;

* Teams in Advanced Roadmaps are different from the teams found in the rest of Jira Software Cloud. In Advanced Roadmaps, they act as a label applied to issues that designates which team will eventually pick up the work on your timeline.
* By adding the Team field to your Jira issues, you can save this value back to your Jira issues, which makes sprint planning easier

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   "fmt"
   v2 "github.com/ctreminiom/go-atlassian/jira/v2"
   "log"
   "os"
)

func main() {

   var (
      host  = os.Getenv("HOST")
      mail  = os.Getenv("MAIL")
      token = os.Getenv("TOKEN")
   )

   atlassian, err := v2.New(nil, host)
   if err != nil {
      return
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   teams, response, err := atlassian.Team.Gets(context.Background(), 1000)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", response.Bytes.String())
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.Code)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   for _, team := range teams.Teams {
      fmt.Println(team.Title, team.Id, team.ExternalId, team.Shareable)
   }

   for _, person := range teams.Persons {
      fmt.Println(person.PersonId, person.JiraUser)
   }
}
```
{% endcode %}

### Create Team

`POST /rest/teams/1.0/teams/create`

Create create creates a team on the Advanced Roadmaps.

{% code fullWidth="true" %}
```go
package main

import (
   "context"
   v2 "github.com/ctreminiom/go-atlassian/jira/v2"
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

   atlassian, err := v2.New(nil, host)
   if err != nil {
      return
   }

   atlassian.Auth.SetBasicAuth(mail, token)
   atlassian.Auth.SetUserAgent("curl/7.54.0")

   payload := &models.JiraTeamCreatePayloadScheme{
      Title:     "Team Voldemort",
      Shareable: true,
      Resources: nil,
   }

   team, response, err := atlassian.Team.Create(context.Background(), payload)
   if err != nil {
      if response != nil {
         log.Println("Response HTTP Response", response.Bytes.String())
      }
      log.Fatal(err)
   }

   log.Println("Response HTTP Code", response.Code)
   log.Println("HTTP Endpoint Used", response.Endpoint)

   spew.Dump(team)
}
```
{% endcode %}

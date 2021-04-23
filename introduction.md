# ☘️ Introduction



![](.gitbook/assets/go-atlassian-logo.svg)

 [![](https://camo.githubusercontent.com/d937be9c0735b463af1b599bb221b811c701fcc815b8fdd69fb3fca371ed21e9/68747470733a2f2f706b672e676f2e6465762f62616467652f6769746875622e636f6d2f637472656d696e696f6d2f676f2d61746c61737369616e3f75746d5f736f757263653d676f646f63)](https://pkg.go.dev/github.com/ctreminiom/go-atlassian) [![](https://camo.githubusercontent.com/dce12148ca523ebc03c1171d4bf4e072e9413ee8035aa4dfc80cdd4e357c134e/68747470733a2f2f676f7265706f7274636172642e636f6d2f62616467652f637472656d696e696f6d2f676f2d61746c61737369616e)](https://goreportcard.com/report/github.com/ctreminiom/go-atlassian) [![](https://camo.githubusercontent.com/fa2639b5d8dd26b923aea47ba4ce1490c5d78662faa570468ca922abe5237af1/68747470733a2f2f6170702e666f7373612e636f6d2f6170692f70726f6a656374732f6769742532426769746875622e636f6d253246637472656d696e696f6d253246676f2d61746c61737369616e2e7376673f747970653d736869656c64)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge_shield) [![](https://camo.githubusercontent.com/4be14314593355db96f4198d84ad5ea634269fe544d635a52f125b855e32219b/68747470733a2f2f636f6465636f762e696f2f67682f637472656d696e696f6d2f676f2d61746c61737369616e2f6272616e63682f6d61696e2f67726170682f62616467652e7376673f746f6b656e3d47304b504e4d54495256)](https://codecov.io/gh/ctreminiom/go-atlassian) [![](https://camo.githubusercontent.com/d01759d87aaba86cf8c3f2e23bab2b03d285b550c2893e39060c28f01b8f9d35/68747470733a2f2f6170702e636f646163792e636f6d2f70726f6a6563742f62616467652f47726164652f6665356331623363396664363466383439383961653531633432383033343536)](https://www.codacy.com/gh/ctreminiom/go-atlassian/dashboard?utm_source=github.com&utm_medium=referral&utm_content=ctreminiom/go-atlassian&utm_campaign=Badge_Grade) [![](https://camo.githubusercontent.com/83d3746e5881c1867665223424263d8e604df233d0a11aae0813e0414d433943/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d626c75652e737667)](https://github.com/ctreminiom/go-atlassian/blob/master/LICENSE) [![](https://camo.githubusercontent.com/06c412f257676ff43d3934f1c1f1d455d6d7f8115734d97ef6b4379a414b33c4/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f776f726b666c6f772f7374617475732f637472656d696e696f6d2f676f2d61746c61737369616e2f54657374696e673f6c6162656c3d2546302539462541372541412532307465737473267374796c653d666c617426636f6c6f723d373543343642)](https://github.com/ctreminiom/go-atlassian/actions?query=workflow%3ATesting) [![](https://camo.githubusercontent.com/035a5d1b682c8149a27cea632d609ce279c81343a0f265e35f72341578b5565b/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f254630253946253932254131253230676f2d646f63756d656e746174696f6e2d3030414344372e7376673f7374796c653d666c6174)](https://docs.go-atlassian.io/)

go-atlassian is a Go module that enables the interaction with the Atlassian Cloud Services.

## ✨ Features

### 🛫 Jira Software Cloud

* CRUD Application Roles, Dashboards, Filters, Groups, Issues.
* Search Issues using the JQL query.
* Add attachments into JIRA issues, and transition issues.
* Creation of issue\(s\) using the instance custom\_fields.
* Edit issues using the operation's method **\(add,remove,replace\).**
* Assign issues and send custom mail notifications.
* CRUD the Issue Field Configuration, Issue Type Screen Scheme, and Permission Scheme.
* CRUD custom\_fields contexts and options.
* Link issues and create new issue link types.
* Get the issue priorities and resolutions.
* CRUD Screen Screens, Screens and Screen Tabs.
* CRUD issue votes and watchers.
* CRUD Jira Project\(s\), Project Categories, Project Components, Project Versions.
* CRUD Project Roles and add/remove project actors\(users\).
* Validate new Jira Project Name or Key.
* Add Jira Field into Screen.
* Get or Cancel Jira Async Task\(s\).
* CRUD Jira Users and Search users.

### 🛬 Jira Service Management Cloud

* CRUD JSM Customer\(s\).
* Search JSM Knowledgebase Articles.
* CRUD JSM Organizations and manipulates the Organization's Users.
* Get\(s\) and Answer Approval\(s\) and SLA\(s\).
* CRUD JSM Feedback\(s\) and Participant\(s\).
* Get\(s\) JSM Projects and Queues.
* CRUD JSM Request Types.

### 🛸 Atlassian Admin Cloud

* Get Organization\(s\), Verified Domain\(s\), Audit Log\(s\), and Event Actions.
* CRUD Organization Polities.
* Enable/Disable an Organization User.
* Get/Update Organization User.
* Get/Delete Organization User Token\(s\).
* Create/Deactivate/Get\(s\)/Update/Path SCIM User\(s\).
* Create/Get\(s\)/Update/Delete SCIM Group\(s\).
* Get SCIM Schema\(s\).

### 🛰️ Confluence Cloud

* In Development🔨

### 🚠 Jira Agile Cloud

* In Development🔨

## 🔰 Installation

Make sure you have Go installed \(download\). Version `1.13` or higher is required.

```text
## Jira Software Cloud / Service Management
$ go get -u -v github.com/ctreminiom/go-atlassian/jira/

## Atlassian Cloud Admin
$ go get -u -v github.com/ctreminiom/go-atlassian/admin/
```

## 📓 Documentation

Documentation is hosted live at [https://docs.go-atlassian.io/](https://docs.go-atlassian.io/)

## 📝 Usage

More examples in `jira/examples` `admin/examples` `jira/sm/examples` directories. Here's a short example of how to get a Jira Issue:

```text
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
			log.Println("Response HTTP Response", string(response.BodyAsBytes))
			log.Println(response.StatusCode)
		}
		log.Fatal(err)
	}

	log.Println("Response HTTP Code", response.StatusCode)
	log.Println("HTTP Endpoint Used", response.Endpoint)

	log.Println(issue.Key)
	log.Println(issue.Fields.Reporter.AccountID)

	for _, transition := range issue.Transitions {
		log.Println(transition.Name, transition.ID, transition.To.ID, transition.HasScreen)
	}

}
```

## ⭐️ Project assistance

If you want to say **thank you** or/and support active development of `go-atlassian`:

* Add a [GitHub Star](https://github.com/ctreminiom/go-atlassian) to the project.
* Write interesting articles about project on [Dev.to](https://dev.to/), [Medium](https://medium.com/) or personal blog.
* Support the project by donating a cup of coffee.
* Contributions, issues and feature requests are welcome!
* Feel free to check [issues page](https://github.com/ctreminiom/go-atlassian/issues).

[![Buy Me A Coffee](https://camo.githubusercontent.com/c3f856bacd5b09669157ed4774f80fb9d8622dd45ce8fdf2990d3552db99bd27/68747470733a2f2f7777772e6275796d6561636f666665652e636f6d2f6173736574732f696d672f637573746f6d5f696d616765732f6f72616e67655f696d672e706e67)](https://www.buymeacoffee.com/ctreminiom)

## 💡 Inspiration

The project was created with the purpose to provide a unique point to provide an interface for interacting with Atlassian products. This module is highly inspired by the Go library [https://github.com/andygrunwald/go-jira](https://github.com/andygrunwald/go-jira) but focused on Cloud solutions.

## 🧪 Run Test Cases

## 💳 Credits

In addition to all the contributors we would like to thank these vendors:

* **Atlassian** for developing such a powerful ecosystem.
* **Gitbook** for provided full features for open-source projects

## 📝 License

Copyright © 2021 [Carlos Treminio](https://github.com/ctreminiom). This project is [MIT](https://opensource.org/licenses/MIT) licensed.

[![FOSSA Status](https://camo.githubusercontent.com/225779948e4b6b38821405cda256693d1c12977647d0667a19851c34602103ee/68747470733a2f2f6170702e666f7373612e636f6d2f6170692f70726f6a656374732f6769742532426769746875622e636f6d253246637472656d696e696f6d253246676f2d61746c61737369616e2e7376673f747970653d6c61726765)](https://app.fossa.com/projects/git%2Bgithub.com%2Fctreminiom%2Fgo-atlassian?ref=badge_large)


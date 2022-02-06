# ⏱ Changelogs

This example will guide you with the creation of a simple CLI that extracts the issue changelogs using a JQL query and saves it into a CSV/JSON file.



The first step will be the creation of the new Jira Cloud instance in order to manipulate the endpoints services, this code will ask you the credentials on the prompt and it'll create a new Jira client instance.

```go
var questions = []*survey.Question{
	{
		Name:     "host",
		Prompt:   &survey.Input{Message: "Select an Atlassian instance", Default: os.Getenv("HOST")},
		Validate: survey.Required,
	},
	{
		Name:   "mail",
		Prompt: &survey.Input{Message: "Please provide an email address", Default: os.Getenv("MAIL")},
	},
	{
		Name:   "token",
		Prompt: &survey.Input{Message: "Please provide your Atlassian token API", Default: os.Getenv("TOKEN")},
	},
	{
		Name:   "jql",
		Prompt: &survey.Input{Message: "Please provide a valid JQL query"},
	},
}

var answers Answers
if err := survey.Ask(questions, &answers); err != nil {
	log.Fatal(err)
}

log.Infoln("Creating the Jira client...")
jiraCloud, err := v2.New(nil, answers.Host)
if err != nil {
	return
}

jiraCloud.Auth.SetBasicAuth(answers.Mail, answers.Token)
jiraCloud.Auth.SetUserAgent("curl/7.54.0")

log.Infoln("Getting the instance fields...")
```




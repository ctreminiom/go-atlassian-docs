# ⏱ Changelogs

This example will guide you with the creation of a simple CLI that extracts the issue changelogs using a JQL query and saves it into a CSV/JSON file.



The first step will be the creation of the new Jira Cloud instance in order to manipulate the endpoints services, this code will ask you the credentials on the prompt and it'll create a new Jira client instance.

![](<../.gitbook/assets/image (20).png>)

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

Then, the CLI will ask you what fields are required to be extracted, the following code implements that funcionality.

![](../.gitbook/assets/fields.gif)

```go
log.Infoln("Getting the instance fields...")

var fields []*models.IssueFieldScheme
var startAt int

for {

	page, response, err := jiraCloud.Issue.Field.Search(context.Background(), nil, startAt, 50)
	if err != nil {
		log.Error(response.Bytes.Bytes())
		log.Fatal(err)
	}

	if page.IsLast {
		break
	}

	fields = append(fields, page.Values...)
	startAt += 50
}

var fieldOptions []string
for _, field := range fields {
	fieldOptions = append(fieldOptions, fmt.Sprintf("%v || %v", field.Name, field.ID))
}

var fieldsToFilter []string
prompt := &survey.MultiSelect{Message: "What fields do you want to export?", Options: fieldOptions, PageSize: 20}

if err = survey.AskOne(prompt, &fieldsToFilter); err != nil {
	log.Fatal(err)
}

var fieldsToFilterAsIDs []string
for _, field := range fieldsToFilter {
	fieldsToFilterAsIDs = append(fieldsToFilterAsIDs, strings.Split(field, " || ")[1])
}
```

With the JQL query and the fields needed, we can use the Jira Search service and extract the issue metadata.

```go
log.Infoln("Searching the issue history...")

var issues []*models.IssueSchemeV2
for {

	page, response, err := jiraCloud.Issue.Search.Get(
		context.Background(),
		answers.JQL,
		nil,
		[]string{"changelog"},
		startAt,
		50,
		"")

	if err != nil {
		log.Error(response.Bytes.Bytes())
		log.Fatal(err)
	}

	if len(page.Issues) == 0 {
		break
	}

	startAt += 50
	issues = append(issues, page.Issues...)
}

log.Infof("%v issues found", len(issues))
```

&#x20;Finally, extract the issue history using the fields selected previously and save the report on a .csv file.

![](<../.gitbook/assets/image (19).png>)

```go
file, err := os.Create(fmt.Sprintf("changelogs-%v.csv", uuid.NewString()))
if err != nil {
	log.Fatal(err)
}

writer := csv.NewWriter(file)

writer.Write([]string{"key", "project", "summary", "field", "from", "to", "when", "who?"})

for _, issue := range issues {

	for _, history := range issue.Changelog.Histories {

		for _, item := range history.Items {

			if contains(fieldsToFilterAsIDs, item.Field) {

				var from string
				if item.From == "" {
					from = item.FromString
				} else {
					from = item.From
				}

				var to string
				if item.To == "" {
					to = item.ToString
				} else {
					to = item.To
				}

				writer.Write([]string{
					issue.Key,
					issue.Fields.Project.Name,
					issue.Fields.Summary,
					item.Field,
					from,
					to,
					history.Created,
					history.Author.EmailAddress,
				})
			}
		}
	}
}

writer.Flush()
log.Info("Report created!!")
```

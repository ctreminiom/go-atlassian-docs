---
cover: >-
  ../.gitbook/assets/1212_atlassian-employee-resource-groups_1120x545@2x-1-1560x760.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🧩 Extract customfields from issue(s)

## Introduction

In Jira Cloud, custom fields are managed in a way that allows for flexibility and dynamic creation. Each new custom field that you create within a Jira Cloud instance is assigned a unique ID. This ID is used to identify and manage the custom field's configuration and data within the Jira system.

In this particular case, the library contains multiples helpers method will help you to extract the customfield information by type using the response buffer.

## Extract from a single issue

The following methods can be used to extract the customfield information from **one** issue.

### **ParseMultiSelectCustomField**&#x20;

This method parses a multi-select custom field from the given buffer data associated with the specified custom field ID and returns a slice of pointers to `CustomFieldContextOptionSchema` structs.&#x20;

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

options, err := models.ParseMultiSelectCustomField(response.Bytes, "customfield_10046")
if err != nil {
	log.Fatal(err)
}

for _, option := range options {
	fmt.Println(option.ID, option.Value)
}
```
{% endcode %}

### ParseCascadingSelectCustomField

This method parses a cascading custom field from the given buffer data associated with the specified custom field ID and returns a **CascadingSelectScheme** struct pointer.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

cascading, err := models.ParseCascadingSelectCustomField(response.Bytes, "customfield_10045")
if err != nil {
	log.Fatal(err)
}

fmt.Println(cascading.Value, cascading.Child.Value)
```
{% endcode %}

### ParseMultiUserPickerCustomField

This method parses a group-picker custom field from the given buffer data associated with the specified custom field ID and returns a slice of pointers to **UserDetailScheme** structs.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

users, err := models.ParseMultiUserPickerCustomField(response.Bytes, "customfield_10055")
if err != nil {
	log.Fatal(err)
}

for _, user := range users {
	fmt.Println(user.EmailAddress, user.AccountID)
}
```
{% endcode %}

### ParseMultiGroupPickerCustomField

This method parses a group-picker custom field from the given buffer data associated with the specified custom field ID and returns a slice of pointers to **GroupDetailScheme** structs.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

groups, err := models.ParseMultiGroupPickerCustomField(response.Bytes, "customfield_10052")
if err != nil {
	log.Fatal(err)
}

for _, group := range groups {
	fmt.Println(group.Name)
}
```
{% endcode %}

### ParseFloatCustomField

ParseFloatCustomField parses a float custom field from the given buffer data associated with the specified custom field ID and returns string.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

number, err := models.ParseFloatCustomField(response.Bytes, "customfield_10043")
if err != nil {
	log.Fatal(err)
}

fmt.Println(number)
```
{% endcode %}

### ParseSprintCustomField

ParseSprintCustomField parses a sprints custom field from the given buffer data associated with the specified custom field ID and returns a slice of the **SprintDetailScheme** struct.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

sprints, err := models.ParseSprintCustomField(response.Bytes, "customfield_10020")
if err != nil {
	log.Fatal(err)
}

for _, sprint := range sprints {
	fmt.Println(sprint.ID)
}
```
{% endcode %}

### ParseLabelCustomField

ParseLabelCustomField parses a textfield slice custom field from the given buffer data associated with the specified custom field ID and returns string slice.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

labels, err := models.ParseLabelCustomField(response.Bytes, "customfield_10042")
if err != nil {
	log.Fatal(err)
}

for _, label := range labels {
	fmt.Println(label)
}
```
{% endcode %}

### ParseMultiVersionCustomField

ParseMultiVersionCustomField parses a version-picker custom field from the given buffer data associated with the specified custom field ID and returns a slice of pointers to **VersionDetailScheme** structs.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

versions, err := models.ParseMultiVersionCustomField(response.Bytes, "customfield_10067")
if err != nil {
	log.Fatal(err)
}

for _, version := range versions {
	fmt.Println(version.ID, version.Name, version.Description)
}
```
{% endcode %}

### ParseAssetCustomField

ParseAssetCustomField parses the Jira assets elements from the given buffer data associated with the specified custom field ID and returns a struct **CustomFieldAssetScheme** slice.

{% code overflow="wrap" %}
```go
issue, response, err := client.Issue.Get(context.Background(), "KP-23", nil, []string{"transitions"})
if err != nil {
	log.Fatal(err)
}

assets, err := models.ParseAssetCustomField(response.Bytes, "customfield_10068")
if err != nil {
	log.Fatal(err)
}

for _, asset := range assets {
	fmt.Println(asset.Id, asset.WorkspaceId, asset.ObjectId)
}
```
{% endcode %}

## Extract from multiple issues

If you want to extract the customfield values from a buffer of **issues**, you can use the following methods, it returns a map where the key is the issue key and the value is the customfield values parsed

### ParseMultiSelectCustomFields

ParseMultiSelectCustomFields extracts and parses multi-select custom field data from a given bytes.Buffer from multiple issues.&#x20;

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **CustomFieldContextOptionScheme** structs, representing the parsed multi-select custom field values.&#x20;

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.
* The function parses this structure to extract and organize the custom field values.
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10046"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

multiSelectOptions, err := models.ParseMultiSelectCustomFields(response.Bytes, "customfield_10046")
if err != nil {
	log.Fatal(err)
}

for issue, options := range multiSelectOptions {
	for _, option := range options {
		fmt.Println(issue, option.ID, option.Value)
	}
}
```
{% endcode %}

### ParseCascadingCustomFields&#x20;

ParseCascadingCustomFields extracts and parses a cascading custom field data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a **CascadingSelectScheme** struct pointer, representing the parsed cascading custom field value.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10045"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseCascadingCustomFields(response.Bytes, "customfield_10045")
if err != nil {
	log.Fatal(err)
}

for issue, value := range customfields {
	fmt.Println(issue, value.Value, value.Child.Value)
}
```
{% endcode %}

### ParseMultiUserPickerCustomFields&#x20;

ParseMultiUserPickerCustomFields extracts and parses a user picker custom field data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **UserDetailScheme** structs, representing the parsed multi-select custom field values.&#x20;

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10055"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseMultiUserPickerCustomFields(response.Bytes, "customfield_10055")
if err != nil {
	log.Fatal(err)
}

for issue, users := range customfields {

	for _, user := range users {
		fmt.Println(issue, user.AccountID, user.DisplayName)
	}
}
```
{% endcode %}

### ParseMultiGroupPickerCustomFields&#x20;

ParseMultiGroupPickerCustomFields extracts and parses a group picker custom field data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **GroupDetailScheme** structs, representing the parsed multi-group custom field values.&#x20;

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.&#x20;
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10052"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseMultiGroupPickerCustomFields(response.Bytes, "customfield_10052")
if err != nil {
	log.Fatal(err)
}

for issue, groups := range customfields {
	for _, group := range groups {
		fmt.Println(issue, group.Name, group.GroupID)
	}
}
```
{% endcode %}

### ParseFloatCustomFields&#x20;

ParseFloatCustomFields extracts and parses the float customfield information from multiple issues using a bytes.Buffer.&#x20;

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a string representing the parsed float64 customfield value.&#x20;

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.&#x20;
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10043"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseFloatCustomFields(response.Bytes, "customfield_10043")
if err != nil {
	log.Fatal(err)
}

for issue, number := range customfields {
	fmt.Println(issue, number)
}
```
{% endcode %}

### ParseSprintCustomFields&#x20;

ParseSprintCustomFields extracts and parses sprint custom field data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **SprintDetailScheme** structs, representing the parsed sprint custom field values.

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.&#x20;
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10020"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseSprintCustomFields(response.Bytes, "customfield_10020")
if err != nil {
	log.Fatal(err)
}

for issue, sprints := range customfields {

	for _, sprint := range sprints {
		fmt.Println(issue, sprint.Name, sprint.ID, sprint.BoardID, sprint.State)
	}
}
```
{% endcode %}

### ParseLabelCustomFields&#x20;

ParseLabelCustomFields extracts and parses the label customfield information from multiple issues using a bytes.Buffer.&#x20;

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a string slice representing the parsed label customfield value.

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.&#x20;
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10042"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseLabelCustomFields(response.Bytes, "customfield_10042")
if err != nil {
	log.Fatal(err)
}

for issue, labels := range customfields {
	fmt.Println(issue, labels)
}
```
{% endcode %}

### ParseMultiVersionCustomFields&#x20;

ParseMultiVersionCustomFields extracts and parses a version picker custom field data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **ParseMultiVersionCustomFields** structs, representing the parsed multi-version custom field values.

* The JSON data within the buffer is expected to have a specific structure where the custom field values are organized by issue keys and options are represented within a context.&#x20;
* The function parses this structure to extract and organize the custom field values.&#x20;
* If the custom field data cannot be parsed successfully, an error is returned.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10067"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseMultiVersionCustomFields(response.Bytes, "customfield_10067")
if err != nil {
	log.Fatal(err)
}

for issue, versions := range customfields {

	for _, version := range versions {
		fmt.Println(issue, version.Name, version.ID)
	}
}
```
{% endcode %}

### ParseAssetCustomFields&#x20;

ParseAssetCustomFields extracts and parses jira assets customfield data from a given bytes.Buffer from multiple issues.

This function takes the name of the custom field to parse and a bytes.Buffer containing JSON data representing the custom field values associated with different issues. It returns a map where the key is the issue key and the value is a slice of **CustomFieldAssetScheme** structs, representing the parsed assets associated with a Jira issues.

{% code overflow="wrap" %}
```go
_, response, err := client.Issue.Search.Post(context.Background(), "project = KP", []string{"customfield_10072"}, nil, 0, 50, "")
if err != nil {
	log.Fatal(err)
}

customfields, err := models.ParseAssetCustomFields(response.Bytes, "customfield_10072")
if err != nil {
	log.Fatal(err)
}

for issue, assets := range customfields {

	for _, asset := range assets {
		fmt.Println(issue, asset.Id, asset.WorkspaceId, asset.ObjectId)
	}
}
```
{% endcode %}
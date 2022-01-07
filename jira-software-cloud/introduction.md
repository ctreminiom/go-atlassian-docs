# 🗃 Introduction

The Jira REST API enables you to interact with Jira programmatically. Use this API to [build apps](https://developer.atlassian.com/cloud/jira/platform/integrating-with-jira-cloud/), script interactions with Jira, or develop any other type of integration. This page documents the REST resources available in Jira Cloud, including the HTTP response codes and example requests and responses.

This document contains examples of the Jira API v3 and v2. Both versions offer the same collection of operations. However, version 3 provides support for the [Atlassian Document Format](https://developer.atlassian.com/cloud/jira/platform/apis/document/structure/) (ADF) in:

* `body` in comments, including where comments are used in issue, issue link, and transition resources.
* `comment` in worklogs.
* `description` and `environment` fields in issues.
* `textarea` type custom fields (multi-line text fields) in issues. Single line custom fields (`textfield`) accept a string and don't handle Atlassian Document Format content.

The version separation is through packages, if you need to use version 3, use the **v3** package

![](<../.gitbook/assets/image (18).png>)

{% hint style="success" %}
Both versions contain the same methods!!
{% endhint %}

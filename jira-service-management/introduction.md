---
cover: >-
  ../.gitbook/assets/hero_1120x545_csd-5489-tier-1-illo-interpersonal-skills-1-9-in-series@2x-1560x760.png
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

# 🗺 Introduction

The _go-atlassian_ `sm` module provides a set of functions and types for interacting with the Jira Service Management REST API. It  provides a programmatic interface to interact with Jira Service Management, enabling developers to automate various aspects of service management.

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption><p>Request Sample</p></figcaption></figure>

Here are some key components and concepts related to the Jira Service Management REST API:

1. **Request Types**: Request types represent different types of service requests or issue types within Jira Service Management. Examples include "Incident," "Change Request," "Service Request," etc. Request types define the fields and workflows associated with each type of request.
2. **Queues**: Queues in Jira Service Management are used to organize and prioritize service requests. Requests are assigned to specific queues based on their attributes or criteria, such as request type, status, or assignee. Queues help teams manage and process incoming requests efficiently.
3. **Approvers**: Approvers are individuals or groups responsible for reviewing and approving certain actions or changes within Jira Service Management. For example, an approver might need to approve a change request before it can be implemented. The REST API allows you to retrieve and manage approvers associated with requests or specific workflows.
4. **Customers**: Customers in Jira Service Management represent the end-users or customers who raise service requests. These can be individuals or groups within an organization. The REST API enables you to create, update, and manage customer accounts, as well as retrieve information about customers and their associated requests.
5. **Workflows**: Workflows define the stages and transitions that a request goes through during its lifecycle in Jira Service Management. Workflows determine how requests move from one status to another, who can perform certain actions, and what actions are available at each stage. The REST API allows you to interact with workflows, including creating, updating, and transitioning requests between workflow states.
6. **Service Level Agreements (SLAs)**: SLAs define the expected response and resolution times for different request types. They help teams prioritize and meet service-level targets. The REST API allows you to manage SLAs associated with requests, retrieve SLA information, and track SLA performance.

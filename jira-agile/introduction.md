# 🏜 Introduction

The _go-atlassian_ `agile` module provides a set of functions and types for interacting with the Jira Agile REST API. It is designed to allows developers to interact with and automate various aspects of agile project management within Jira, such as boards, sprints, and issues.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Kanban Board Sample</p></figcaption></figure>

Here's a brief introduction to some key concepts in Jira Agile Rest API:

1. **Boards**: A board in Jira represents a visual representation of a project, typically in the form of a Kanban board or a Scrum board. It helps teams track and manage their work. The API provides endpoints to create, retrieve, update, and delete boards, as well as to manage the configuration and settings associated with them.
2. **Sprints**: Sprints are time-boxed iterations in agile development, typically used in Scrum methodology. They represent a fixed duration during which a team works on a set of prioritized issues. The API allows you to create, retrieve, update, and delete sprints, as well as manage the issues associated with them.
3. **Issues**: Issues in Jira represent tasks, bugs, user stories, or any other unit of work that needs to be tracked. The API provides endpoints to create, retrieve, update, and delete issues. You can also perform operations such as assigning issues to users, transitioning between workflow statuses, adding comments, and more.
4. **Agile Boards API**: This API allows you to manage the boards in Jira, including retrieving information about boards, creating new boards, updating board settings, and managing board filters.
5. **Agile Sprints API**: With the Agile Sprints API, you can interact with sprints, including creating new sprints, retrieving sprint details, updating sprint properties, and managing the issues associated with sprints.
6. **Agile Issues API**: This API allows you to perform operations on issues within the context of agile boards and sprints. You can retrieve issues assigned to a board or sprint, create new issues, update issue details, transition issues between workflow statuses, and more.

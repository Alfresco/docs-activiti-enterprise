---
Title: Alfresco Process Workspace
---

# Alfresco Process Workspace
The Alfresco Process Workspace is accessed by end-users to provide them a view of in progress [process instances](../workspace/workspace-processes.md) and [tasks](../workspace/workspace-tasks.md). Users are able to create new process instances and tasks and assign [forms](../modeling/modeling-forms/README.md) to standalone tasks. Users are also able to claim tasks that require human input and view and manage their workload in a dashboard.

To deploy an instance of the Process Workspace with an application, a [user interface definition](../modeling/modeling-interfaces.md) with the template `process` selected needs to be included in the project definition.

The URL to access it will be in the format: `{my-domain}/{application-name}/{ui-name}`. 

## About
The about page can be accessed via the UI or at the URL: `{my-domain}/{application-name}/{ui-name}/about` and shows the packages and package versions used in the application. 

## Settings
You can view the application configuration of the Process Workspace by visiting the URL:`{my-domain}/{application-name}/{ui-name}/app.config.json`.


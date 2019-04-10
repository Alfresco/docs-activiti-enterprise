---
Title: Alfresco Modeling Application
---

# Alfresco Modeling Application

The Alfresco Modeling Application is used to create and update the components that make up an Alfresco Activiti Enterprise project. These include: 

* [Projects](../modeling/modeling-projects.md) are the top level component of a business application.
* [Processes](../modeling/modeling-processes/README.md) use BPMN elements to model a business process. They incorporate the other components created in the Modeling Application.
* [Forms](../modeling/modeling-forms/README.md) are used to collect data from end-users and are attached to a start event or user task within a process.
* [Connectors](../modeling/modeling-connectors/README.md) are used to import data into a process from an external source or update an external source using a process. Connectors are attached to a service task within a process.
* [Decision tables]() are used to make business decisions as part of a process. 

## Modeling
All components of an application can be designed using a Graphical User Interface (GUI) or an XML or JSON editor. Users require the *APS_MODELER* role in order to create projects within the Modeling Application. 

The [files that comprise an application](../modeling/modeling-files.md) are stored in an instance of Alfresco Content Services (ACS). Process instances and tasks that are running can also be stored in a separate repository as nodes, including any content uploaded to them.

The Modeling Application is deployed into the default namespace so there will only be a single instance of it running. The URL will be in the format: `{my-domain}/alfresco-modeling-app`.

## About
The about page can be accessed via the UI or at the URL: `{my-domain}/alfresco-modeling-app/about` and shows the packages and package versions used in the application. 

## Settings
You can view the application configuration of the Modeling Application by visiting the URL:`{my-domain}/alfresco-modeling-app/app.config.json`.
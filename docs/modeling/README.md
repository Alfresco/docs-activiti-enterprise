---
Title: Alfresco Modeling Application
---

# Alfresco Modeling Application

The Alfresco Modeling Application is used to create and update the components that make up an Alfresco Activiti Enterprise project. These include: 

* [Projects](../modeling/modeling-projects.md) are the top level component of a business application.
* [Processes](../modeling/modeling-processes.md) are where BPMN elements are combined to model a business process.
* [Forms](../modeling/modeling-forms.md) are used to collect data from end-users. 
* [Connectors](../modeling/modeling-connectors/README.md) are used to import data into a process from an external source or update an external source using a process.
* [Decision tables]() are used to make business decisions as part of a process. 

All components of an application can be designed using a Graphical User Interface (GUI) or an XML or JSON editor. Users require the *APS_MODELER* role in order to create projects within the Modeling Application. 

The [files that comprise an application](../modeling/modeling-files.md) are stored in an instance of Alfresco Content Services (ACS). Process instances and tasks that are running are  can also be stored in ACS as nodes, including any content uploaded to them.

The Modleing Application is deployed into the default namespace so there will only be a single instance of it running. The URL will be in the format: `{my-domain}/alfresco-modeling-app`.

## About
The about page can be accessed via the UI or at the URL: `{my-domain}/alfresco-modeling-app/about` and shows the packages and package versions used in the application. 

## Settings
You can view the application configuration of the Modeling Application by visiting the URL:`{my-domain}/alfresco-modeling-app/app.config.json`.
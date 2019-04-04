---
Title: Alfresco Administrator Application
---

# Alfresco Administrator Application
The Alfresco Administrator Application is used to [deploy projects](administrator/admin-deploy.md) that have been designed in the Alfresco Modeling Application. 

**Note**: It is important to note that once a project has been deployed it is referred to as an application. 

Once the Administrator Application has deployed a project it can be used to monitor various aspects of the subsequent application:

* The status of the application deployment can be monitored in the [application releases](administrator/admin-applications.md) section.
* Process instances can be viewed and managed in the [process instances](administrator/admin-processes.md) section.
* Tasks can be monitored in the [tasks](administrator/admin-tasks.md) section. 

The Administrator Application can also be used to [upgrade to a new version of an application](administrator/admin-upgrade.md). 

The Administrator Application is deployed into the default namespace so there will only be a single instance of it running. The URL to access it will be in the format: `{my-domain}/alfresco-admin-app`. 

## About
The about page can be accessed via the UI or at the URL: `{my-domain}/alfresco-admin-app/about` and shows the packages and package versions used in the application. 

## Settings
You can view the application configuration of the Administrator Application by visiting the URL:`{my-domain}/alfresco-admin-app/app.config.json`. 


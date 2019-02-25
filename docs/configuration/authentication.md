# Authentication
You can log into the administration console for the Alfresco Identity Service by navigating to `https://activiti-keycloak/{my-domain}/auth` and using the username **admin** and password of **admin**.

**Note**: it is strongly recommended that you update the password for the administration console when you first log into it. 

A default realm of **alfresco** is already setup with the installation of Alfresco Activiti Enterprise. 

Additionally, the following roles are created on installation: 

|Role|Description|
|----|-----------|
|APS_PROCESS_ADMIN|Provides administrator access to all functions including access to admin APIs but can also act as a user of the applications and have tasks assigned to it, whereas APS_ADMIN cannot.| 
|APS_DEVOPS|Provides access to the DevOps functions in the Administrator Application (application definitions and application instances).|
|APS_ADMIN|Provides administrator access to all functions including access to admin APIs.|
|APS_MODELER|Provides access to the Modeling Application.|
|APS_USER|Provides access to fill in forms and initiate processes. Will not have access to any admin APIs.| 

There are several users provided with the deployment that have various roles attached to them. You can use these for testing purposes by updating their passwords using the Identity Service admin account or create your own. 

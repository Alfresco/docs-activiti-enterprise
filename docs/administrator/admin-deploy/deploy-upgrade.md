---
Title: Upgrading an application
---

# Upgrading an application   
Upgrading an application allows for a new version of a released project to be deployed to an existing application. Tasks and process instances that are in progress and based on a previous application version can still be completed, however any new ones started will use the new model definitions. 

Some considerations for upgrading applications to be aware of are that:

* Only the latest released project version can be used to upgrade to. 
* Applications can only be upgraded through the deployment service. This means that if an application was deployed via Helm it cannot be upgraded. 
* It is not possible to change an application name when upgrading an application. 
* It is not possible to use a different Docker image for any service when upgrading an application except for its connectors.
* It is not possible to edit or add any variables to services when upgrading an application. 

## Versioning logic
The version of an application is incremental and independent of the released project version as shown in the following table: 

| Released project version | Application version | 
| ------------------------ | ------------------- | 
| 1 | 1 | 
| 2 | 2 | 
| 4 | 3 | 
| 10 | 4 | 

## Upgrade steps 
Use the following steps to upgrade an application: 

1. Sign into the Administrator Application. 
2. Navigate to the **Application Instances** tab and select the **Upgrade** action against the application to upgrade. 
3. The application name can't be changed and the version to upgrade to can only be the latest released project version.
4. Update the users and/or groups that will have administrator access to the application. 
5. Update the users and/or groups that will have access to the application. 
6. Configure the details for any existing or new connectors
7. The application infrastructure can't be changed. 
8. Click **Upgrade** to upgrade the application. 
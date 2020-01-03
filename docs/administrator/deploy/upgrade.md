---
Title: Upgrading applications
---

# Upgrading applications
Upgrading an application allows for a new version of a released project to be deployed to an existing application. Tasks and process instances that are in progress and based on a previous application version can still be completed, however any new ones started will use the new model definitions. 

Some considerations for upgrading applications to be aware of are that:

* An application can only be upgraded to a released project version that is higher than the one currently deployed.  
* Applications can only be upgraded through the deployment service. This means that if an application was deployed via Helm it cannot be upgraded. 
* It is not possible to change an application name when upgrading an application. 
* It is not possible to use a different Docker image for any service when upgrading an application except for its connectors.
* It is not possible to edit or add any variables to services when upgrading an application, except for its connectors. 

## Versioning logic
The version of an application is incremental and independent of the released project version as shown in the following table: 

| Released project version | Application version | 
| ------------------------ | ------------------- | 
| 1 | 1 | 
| 2 | 2 | 
| 4 | 3 | 
| 10 | 4 | 

## Upgrade steps 
Use the following steps to upgrade an application in the Administrator Application: 

1. Sign into the Administrator Application. 
2. Select the **Upgrade** action against an application in the **Application Instances** tab to upgrade it. 
3. The application name can't be changed and the released project version to upgrade to must be greater than the one currently deployed.
4. Update the users and/or groups that will have administrator access to the application. 
5. Update the users and/or groups that will have access to the application. 

	**Note**: Users require the [`ACTIVITI_ADMIN` role](../identity/README.md#roles) in order to be assigned as an administrator to an application.
			
6. Configure the details for any existing or new connectors.

	* If you created the connector yourself, this will be the name of the connector and the URL of the image you published.
	* If this is an OOTB connector then the name and image are configured automatically. 

7. Set the connector variables in JSON format. For example, if using the [email connector](../../modeling/connectors/ootb/email.md) you can set the variables `EMAIL_HOST` and `EMAIL_PORT` with the following: 

	```json
	{"EMAIL_HOST":"https://mysmtp.com","EMAIL_PORT":"8050"}
	```
	
8. The application infrastructure can't be changed. 
9. Click **Upgrade** to upgrade the application. 
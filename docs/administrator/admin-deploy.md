---
Title: Deploying a project
--- 

# Deploying a project 
The **application releases** section of the Administrator Application contains a list of projects that are available to be deployed. This list of available projects is read directly from the repository that the Modeling Application writes to. Users require the *APS_DEVOPS* role to view the **application releases** section.

**Note**: It is important to note that once a project has been deployed it is referred to as an application. 

Projects will only appear in the **application releases** section if they have been [released](modeling/modeling-projects.md#releasing) in the modeling application. The only action that can be run against a project is to deploy it. This action uses the deployment service to deploy the project and its required services and user interfaces into a new namespace. There is a 1:1 relationship between applications and namespaces.

An admin user or group must be selected during the deployment process and an optional user-level set of individuals or group(s) of users. Once an application has had the deploy action used against it, the [status of the deployment can be monitored](/admin-applications.md).

## Deployment steps
1. Find the project in the list of potential releases in the Administrator Application and select to **Deploy** it. 
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application.
	2. Select the users and/or groups that will have administrator access to the application.
	3. Select the users and/or groups that will have access to the application. 
	4. Select the name of the connector and the location of the image to be used:

		* If you created the connector yourself, this will be the URL of the image you 		published.
		* If this is an OOTB connector then choose the correct connector(s) from the list. 

	5. Set the connector variables in JSON format.
3. **Deploy** the application. 

**Note**: Applications can also be [upgraded](administrator/admin-upgrade.md) if changes or updates are needed in the original project.



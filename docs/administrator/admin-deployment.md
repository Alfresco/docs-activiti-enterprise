---
Title: Application deployment
--- 

# Application releases
Application releases contains a list of projects that have been modeled and released in the Modeling Application and are waiting to be deployed. Once a project has been deployed it is referred to as an application. Users require the APS_DEVOPS role to view application releases. 

The list of available projects is read directly from the repository that the Modeling Application writes to. 

The only action that can be run against an application definition is to deploy it. Deploying an application uses the deployment service to deploy the application and required services and user interfaces into its own namespace. There is a 1:1 relationship between applications and namespaces.

An admin user or group must be selected before you can deploy and an optional user-level set of individuals or group(a) of users. Once an application has had the deploy action used against it, the status of the deployment can be monitored in the *Application instances* section.  

## Deploying a project
1. Find the project in the list of *Application Definitions*  in the Administrator Application and select to **Deploy** it. 
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application.
	2. Select the users and/or groups that will have administrator access to the application.
	3. Select the users and/or groups that will have access to the application. 
	4. Select the name of the connector and the location of the image to be used:

		* If you created the connector yourself, this will be the URL of the image you 		published.
		* If this is an OOTB connector then choose the correct connector(s) from the list. 

	5. Set the connector variables in JSON format.
3. **Deploy** the application. 

	**Note**: you can monitor the deployment status in *Application Instances*.

# Application instances
Application instances allows you to monitor the status of applications after they have been deployed in the application definitions section. Users require the APS_DEVOPS role to view application instances.

As soon as an application has been deployed, it can be monitored in application instances. The possible statuses are mirrored visually and are as follows:

Applications that have been successfully deployed can be removed using the **Undeploy** action. 

Applications in the process of being deployed or that have errored during deployment can be removed using the **Force Undeploy** action. 

An application that has been successfully deployed will use the application name for API queries. For example: 

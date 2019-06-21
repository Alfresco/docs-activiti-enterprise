---
Title: Deploying projects and monitoring applications
--- 

# Deploying projects and monitoring applications
The functions available in the **Devops** section of the Administrator Application are to: 

* [Deploy released projects](#deploying-a-released-project) in the **Application releases** section.
* [Monitor applications](#monitoring-applications) once they have been deployed in the **Application instances** section. 

To access these functions users require the *APS_DEVOPS* role. 

## Deploying a released project 
The **Application releases** section of the Administrator Application contains a list of released projects that are available to be deployed. This list of available released projects is read from the [released projects](../modeling/modeling-projects.md#storage) directory. 

**Note**: It is important to note that once a released project has been deployed it is referred to as an application. 

Projects will only appear in the **Application releases** section if they have been [released](../modeling/modeling-projects.md#releasing) in the modeling application. The only action that can be run against a released project is to deploy it. This action uses the deployment service to deploy the released project and its required services and user interfaces into a new namespace. There is a 1:1 relationship between applications and namespaces.

An admin user or group must be selected during the deployment process and an optional user-level set of individuals or group(s) of users. Once an application has had the deploy action used against it, the [status of the deployment can be monitored](#monitoring-applications).

### Deployment steps
To deploy a released project, sign into the Administrator Application with a user that has permission to deploy. 

**Important**: A project can only be deployed if it has been [released](../modeling/modeling-projects.md#releasing).

1. Find the released project in the list of potential releases in the Administrator Application and select to **Deploy** it. 
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application. 
	2. Select which version of the released project to deploy.
	2. Select the users and/or groups that will have administrator access to the application.
	3. Select the users and/or groups that will have access to the application. 
	4. Configure the details for any connectors defined in the process definitions:

		* If you created the connector yourself, this will be the name of the connector and the URL of the image you published..
		* If this is an OOTB connector then the name and image are configured automatically. 

	5. Set the connector variables in JSON format. For example, if using the [email connector](../modeling/modeling-connectors/connectors-ootb/connectors-email.md) you can set the variables `EMAIL_HOST` and `EMAIL_PORT` with the following: 

		```json
		{"EMAIL_HOST":"https://mysmtp.com","EMAIL_PORT":"8050"}
		```
	6. (Optional) [Customize any application infrastructure](../admin-deploy/deploy-infrastructure.md) on the **Infrastructure** tab before deploying.

		**Note**: This includes using custom Docker images and setting custom variables to set external PostgreSQL locations. 
	 
3. **Deploy** the application. 
4. [Monitor the deployment](#monitoring-applications) process in **Application instances**.
5. Once the deployment has completed the [APIs](../apis/README.md) can be accessed.

## Monitoring applications
The **Application instances** section is for monitoring the status of deployed applications. 

As soon as an application has been deployed, it can be monitored in application instances. The possible statuses are mirrored visually and are as follows:

| Status | Description | 
| ------ | ----------- | 
| CreateApp | The call to create an application was received and an entry was made in the database |
| ImageBuild | The process of building the images has started | 
| ImageBuildFailed | An error occurred whilst the images were being built |
| ImagePush | The process to push the images has started |
| ImagePushFailed | An error occurred whilst the images were being pushed |
| DeployStarted | The images have reached the registry and the creation of Kubernetes resources has started | 
| DeployStartedFailed | An error occurred whilst the Kubernetes resources were being created |
| Pending | The resources were created and are now pending startup | 
| Running | All Kubernetes resources are running | 
| Unknown | There are Kubernetes resources that were not able to enter a running state | 

Applications that have been successfully deployed can be removed using the **Undeploy** action. 

Applications in the process of being deployed or that have errored during deployment can be removed using the **Force Undeploy** action. 

Links are provided for the gateway of each application as well as the URL of its [Process Workspace](../workspace/README.md) and Content Application if the [UIs](../modeling/modeling-interfaces.md) were configured in the application. 

An application that has been successfully deployed will use the application name for [API queries](../apis/README.md). 

---
Title: Project deployment
--- 

# Project deployment
Deploying a project uses the [deployment service](../../architecture/arch-platform.md#deployment-service) to create the images for a project. These images and resources are known as a deployment descriptor. A released project can be deployed using the deployment service, or just have a deployment descriptor created for it. A deployment descriptor is created for every project, regardless of whether it was also deployed by the deployment service or not. 

**Important**: A project is only available to create a descriptor for, or to deploy, once it has been [released](../../modeling/modeling-projects.md#releasing).

**Note**: Project deployment options in the UI or by REST API calls require the *APS_DEVOPS* role. 

## Deployment descriptors
A deployment descriptor can be exported in Helm format so that an application can be deployed. The [deployment service](../../architecture/arch-platform.md#deployment-service) creates the image definitions for an application but does not deploy it. This allows for the application to be downloaded and deployed manually via Helm.

Exporting the deployment descriptor for an application will download a zip file that contains 
a `requirements.yaml` and a `values.yaml`. The `requirements.yaml` contains all the chart dependencies needed for the application deployment, including any custom images. The `values.yaml` contains the chart default overrides that are specific to each application. 

### Creating a deployment descriptor
To create a deployment descriptor in the UI:

1. Select **Create Deployment Descriptor** for a project in the **Project Releases** section.
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application. 
	2. Select which version of the released project to use.
	2. Select the users and/or groups that will have administrator access to the application.
	3. Select the users and/or groups that will have access to the application. 
	4. Configure the details for any connectors defined in the process definitions:

		* If you created the connector yourself, this will be the name of the connector and the URL of the image you published.
		* If this is an OOTB connector then the name and image are configured automatically. 

	5. Set the connector variables in JSON format. For example, if using the [email connector](../../modeling/modeling-connectors/connectors-ootb/connectors-email.md) you can set the variables `EMAIL_HOST` and `EMAIL_PORT` with the following: 

		```json
		{"EMAIL_HOST":"https://mysmtp.com","EMAIL_PORT":"8050"}
		```
		
	6. (Optional) [Customize any application infrastructure](../admin-deploy/deploy-infrastructure.md) on the **Infrastructure** tab before deploying.

		**Note**: This includes using custom Docker images and setting custom variables to set external PostgreSQL instances. 
	 
3. **Create** the descriptor. 

Once a deployment descriptor has been created it is available in the **Deployment Descriptors** section. A deployment descriptor can then be:

* Exported as a zip file using the **Export** option.
* Deployed by the deployment service using the **Deploy** option. 
* Deleted using the **Delete** option.

**Note**: A deployment descriptor cannot be deleted for a deployed application. The application must first be un-deployed. 

**Note**: A deployment descriptor can only be used to deploy a single application. To deploy multiple instances of the same released project different names must be chosen when creating a descriptor or deploying it. 

## Deploying a project
A project can deployed directly from a released project or from a [deployment descriptor](#deployment-descriptors). Using the deployment service to deploy a released project creates its services and user interfaces into a new namespace. There is a 1:1 relationship between applications and namespaces.

An admin user or group must be selected during the deployment process and an optional user-level set of individuals or group(s) of users. The permissions for an application can be updated once it has been deployed by selecting the **Manage Permissions** option against an application in the **Application Instances** section. 

### Deployment steps
To deploy a project from a released project in the UI: 

1. Select **Deploy** for a project in the **Project Releases** section. 
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application. 
	2. Select which version of the released project to deploy.
	2. Select the users and/or groups that will have administrator access to the application.
	3. Select the users and/or groups that will have access to the application. 
	4. Configure the details for any connectors defined in the process definitions:

		* If you created the connector yourself, this will be the name of the connector and the URL of the image you published.
		* If this is an OOTB connector then the name and image are configured automatically. 

	5. Set the connector variables in JSON format. For example, if using the [email connector](../../modeling/modeling-connectors/connectors-ootb/connectors-email.md) you can set the variables `EMAIL_HOST` and `EMAIL_PORT` with the following: 

		```json
		{"EMAIL_HOST":"https://mysmtp.com","EMAIL_PORT":"8050"}
		```
		
	6. (Optional) [Customize any application infrastructure](../admin-deploy/deploy-infrastructure.md) on the **Infrastructure** tab before deploying.

		**Note**: This includes using custom Docker images and setting custom variables to set external PostgreSQL instances. 
	 
3. **Deploy** the application. 

Once an application has been deployed, its status can be [monitored](#monitoring-applications) in the **Application Instances** section. After the deployment process has finished, the [application-level APIs](../../apis/README.md) can be accessed.

## Monitoring applications
The **Application Instances** section is for monitoring the status of deployed applications. 

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

Links are provided for the gateway of each application as well as the URL of its [Process Workspace](../../workspace/README.md) and Content Application if the [UIs](../../modeling/modeling-interfaces.md) were configured in the application. 

An application that has been successfully deployed will use the application name for [API queries](../../apis/README.md). 

 



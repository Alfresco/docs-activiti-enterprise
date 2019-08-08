---
Title: Project deployment
--- 

# Project deployment
Project deployment uses the [deployment service](../../architecture/arch-platform.md#deployment-service). There are two options for deploying a released project:

* [Create a deployment descriptor](#deployment-descriptors) for a released project that can be download as a Helm chart and then subsequently deployed via Helm.

* [Deploy a released project](#deploying-a-released-project) into the Activiti Enterprise cluster into its own namespace.

**Note**: Deploying a released project still creates a deployment descriptor for it and it is also possible to still deploy a deployment descriptor using the deployment service.

**Important**: A project is only available to create a descriptor for, or to deploy, once it has been [released](../../modeling/modeling-projects.md#releasing).

Project deployment options in the UI or by REST API call require the *APS_DEVOPS* role. 

## Deployment descriptors
Creating a deployment descriptor uses the API to create a descriptor for a released project that can be download as a Helm chart and then subsequently deployed via Helm. It is possible to:

* [Create a deployment descriptor](#creating-a-deployment-descriptor)
* [Export a deployment descriptor](#exporting-a-deployment-descriptor)

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

* Exported using the **Download as Helm** option.
* Deployed by the deployment service using the **Deploy** option. 
* Deleted using the **Delete** option.

**Note**: A deployment descriptor cannot be deleted for a deployed application. The application must first be un-deployed. 

**Note**: A deployment descriptor can only be used to deploy a single application. To deploy multiple instances of the same released project different names must be chosen when creating a descriptor or deploying it. 

### Exporting a deployment descriptor
Exporting a deployment descriptor will download a `helm.zip` file that contains the following:

```
/helm/
	/alfresco-process-application/
		Chart.yaml
		requirements.yaml
		/templates/
			_helpers.tpl
			NOTES.txt
		values.yaml
```

The `Chart.yaml` contains the location of the parent [Helm chart used](https://git.alfresco.com/process-services-public/alfresco-process-application-deployment).

The contents of `requirements.yaml` is dynamically updated with the necessary chart dependencies depending on the contents of the project. For example, if the Process Workspace is not defined as a user interface in the project, then the following will not appear in the file: 

```yaml
- name: alfresco-adf-app
  repository: https://kubernetes-charts.alfresco.com/incubator
  version: 2.1.3
  condition: alfresco-process-workspace-app.enabled
  alias: alfresco-process-workspace-app
``` 

The contents of `values.yaml` is also dynamically updated depending on the contents of the project and the configurations made during the deployment descriptor creation such as the  settings for external PostgreSQL instances. 

**Important**: Before deploying the Helm chart, a [client needs to be created](https://www.keycloak.org/docs/6.0/server_admin/#oidc-clients) in the Identity Service for the application using the same name as the deployment descriptor. The client level roles required are *APS_USER* and *APS_ADMIN* with the relevant users or groups assigned to each. 

## Deploying a released project
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

 



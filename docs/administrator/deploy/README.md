---
Title: Project deployment
--- 

# Project deployment
Project deployment can use the [deployment service](../../architecture/platform.md#deployment-service) or a Helm Chart. There are three options for deploying a released project:

* [Deploy exclusively using Helm](../deploy/helm.md).

* Create a [deployment descriptor](#deployment-descriptors) for a released project that can be download as a Helm chart and then subsequently deployed via Helm.

* [Deploy a released project](#deploying-a-released-project) into the Activiti Enterprise cluster into its own namespace.

**Note**: Deploying a released project still creates a deployment descriptor for the project and it is also possible to deploy a deployment descriptor into the Activiti Enterprise cluster using the deployment service.

**Important**: A project is only available to create a descriptor for, or to deploy, once it has been [released](../../modeling/projects.md#releasing).

Project deployment options in the UI or by REST API call require the `ACTIVITI_DEVOPS` role. 

## Naming  
Application and deployment descriptor names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. 

The following are examples of valid application and deployment descriptor names: 

```
hr-application-1
application3
application-alpha-omega
```

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
	3. Select the users and/or groups that will have [administrator access](../identity/README.md#permissions) to the application.

		**Note**: Users require the [`ACTIVITI_ADMIN` role](../identity/README.md#roles) in order to be assigned as an administrator to an application.
	
	4. Select the users and/or groups that will have [user access](../identity/README.md#permissions) to the application. 

		**Note**: Users require the [`ACTIVITI_USER` role](../identity/README.md#roles) in order to be assigned as a user to an application. 
		
		**Note**: Users and groups that are set as assignees or candidates of a [user task](../../modeling/processes/bpmn/user.md) are automatically added as users and cannot be removed.	
		
	5. Configure the details for any connectors defined in the process definitions:

		* If you created the connector yourself, this will be the name of the connector and the URL of the image you published.
		* If this is an OOTB connector then the name and image are configured automatically. 

	6. Configure any additional configuration parameters or override the existing ones for the connectors that are part of the application.		
	7. (Optional) [Customize any application infrastructure](../deploy/infrastructure.md) on the **Infrastructure** tab before deploying.

		**Note**: This includes using custom Docker images, setting custom variables to set external PostgreSQL instances and configuring an event bridge. 
	 
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

The `Chart.yaml` contains the location of the parent [Helm chart used](https://github.com/Alfresco/alfresco-process-application-deployment).

The contents of `requirements.yaml` is dynamically updated with the necessary chart dependencies depending on the contents of the project. For example, if the Process Workspace is not defined as a user interface in the project, then the following will not appear in the file: 

```yaml
- name: alfresco-adf-app
  repository: https://kubernetes-charts.alfresco.com/incubator
  version: 2.1.3
  condition: alfresco-process-workspace-app.enabled
  alias: alfresco-process-workspace-app
``` 

The contents of `values.yaml` is also dynamically updated depending on the contents of the project and the configurations made during the deployment descriptor creation such as the  settings for external PostgreSQL instances. 

### Deploying a deployment descriptor via Helm
To deploy a deployment descriptor using Helm an Identity Service client first needs to be created before deploying the Helm Chart. 

1. Create a file called `application.json` that contains the following lines and place it in the route of the Helm chart project: 

	```json
	{
    "name": "app_client_name",
    "security": [
    {
      "role": "APS_USER",
      "groups": [
        "insert_user_group"
      ],
      "users": [
        "insert_user"
      ]
    },
    {
      "role": "APS_ADMIN",
      "groups": [],
      "users": [
        "insert_admin"
      ]
    }
  	]
	}
	```

2. Update the `application.json` with the names of the users or groups that will have regular access and administrator to the application. 

3. Update the `application.json` with a relevant name for the client. This name will need to match the name of the Helm chart when it is deployed. 

4. Run the following command to create the Identity Service image replacing the `KEYCLOAK_AUTHSERVERURL` with that of the Identity Service URL of the Activiti Enterprise deployment:  

	```docker
	docker run -it --rm \ 	--env KEYCLOAK_AUTHSERVERURL=https://identity.***/auth \  	--env ACT_KEYCLOAK_CLIENT_APP=admin-cli \  	--env ACT_KEYCLOAK_CLIENT_USER=client \  	--env ACT_KEYCLOAK_CLIENT_PASSWORD=client \  	--volume "$PWD":/tmp/app \  	quay.io/alfresco/alfresco-deployment-cli:master /tmp/app/application.json
	```

5. Deploy using a command similar to the following: 

	```
	helm upgrade name ./helm/alfresco-process-application --install --set global.gateway.domain=domain.com --namespace=namespace
	```
	* `name` needs to match the name used in the `application.json`
	* `domain.com` is the domain name to use. 
	* `namespace` is the namespace to deploy to.

## Deploying a released project
A project can deployed directly from a released project or from a [deployment descriptor](#deployment-descriptors). Using the deployment service to deploy a released project creates its services and user interfaces into a new namespace. There is a 1:1 relationship between applications and namespaces.

An admin user or group must be selected during the deployment process and an optional user-level set of individuals or group(s) of users. The permissions for an application can be updated once it has been deployed by selecting the **Manage Permissions** option against an application in the **Application Instances** section. 

### Deployment steps
To deploy a project from a released project in the UI: 

1. Select **Deploy** for a project in the **Project Releases** section. 
2. The Administrator Application will then step through the deployment options:

	1. Choose a name for the application. 
	2. Select which version of the released project to deploy.
	3. Select the users and/or groups that will have [administrator access](../identity/README.md#permissions) to the application.
	
		**Note**: Users require the [`ACTIVITI_ADMIN` role](../identity/README.md#roles) in order to be assigned as an administrator to an application.
	
	4. Select the users and/or groups that will have [user access](../identity/README.md#permissions) to the application. 

		**Note**: Users require the [`ACTIVITI_USER` role](../identity/README.md#roles) in order to be assigned as a user to an application. 
		
		**Note**: Users and groups that are set as assignees or candidates of a [user task](../../modeling/processes/bpmn/user.md) are automatically added as users and cannot be removed.	
		
	5. Select the users and/or groups that will have access to the application. 
	6. Configure the details for any connectors defined in the process definitions:

		* If you created the connector yourself, this will be the name of the connector and the URL of the image you published.
		* If this is an OOTB connector then the name and image are configured automatically. 

	7. Configure any additional configuration parameters or override the existing ones for the connectors that are part of the application.
		
	8. (Optional) [Customize any application infrastructure](../deploy/infrastructure.md) on the **Infrastructure** tab before deploying.

		**Note**: This includes using custom Docker images, setting custom variables to set external PostgreSQL instances and configuring an event bridge.  
	 
3. **Deploy** the application. 

Once an application has been deployed, its status can be [monitored](#monitoring-applications) in the **Application Instances** section. After the deployment process has finished, the [application-level APIs](../../apis/README.md) can be accessed.

Applications can also be [upgraded](../deploy/upgrade.md) to new versions if the project definition has changed and a new project version has been released.

## Monitoring applications
The **Application Instances** section is for monitoring the status of deployed applications. 

As soon as an application has been deployed, it can be monitored in application instances. The possible statuses are mirrored visually and are as follows:

| Status | Description | 
| ------ | ----------- | 
| CreateApp | The call to create an application was received and an entry was made in the database |
| CreateDescriptor | The descriptor is being created |
| DescriptorCreated | The descriptor has been created |
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

Links are provided for the gateway of each application as well as the URL of its [Process Workspace](../../workspace/README.md) and Content Application if the [UIs](../../modeling/interfaces.md) were configured in the application. 

An application that has been successfully deployed will use the application name for [API queries](../../apis/README.md). 
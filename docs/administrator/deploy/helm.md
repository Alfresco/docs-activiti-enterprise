---
Title: Helm deployment
---

# Deploying with Helm
To deploy an application using Helm rather than the deployment service [use this Helm chart](https://github.com/Alfresco/alfresco-process-application-deployment).  

## Prerequisites

* The Activiti Enterprise infrastructure is deployed in Kubernetes.

## Create a namespace
An application requires a Kubernetes namespace that contains a Quay secret to pull images from Quay.io and a valid Activiti Enterprise license.

1. Create a Kubernetes namespace for the application. 

2. Install a Quay secret into the Docker registry of the namespace: 

	```
	kubectl create secret \
	docker-registry quay-registry-secret \
		--docker-server=quay.io \
		--docker-username="${DOCKER_REGISTRY_USER}" \
		--docker-password="${DOCKER_REGISTRY_PASSWORD}" \
		--docker-email="${DOCKER_REGISTRY_EMAIL}"
	```
 
3. Install a valid license secret into the Kubernetes namespace:

	```
	kubectl create secret \
		generic licenseaps --from-file activiti.lic
	```

## (Optional) Create custom image 
The [application runtime bundle](https://github.com/Alfresco/example-process-application/tree/master/example-application-project/project) can be replaced with a custom Docker image. The definition XML and JSON files for processes, forms, DMN and scripts need to be placed in the relevant folders.

1. Clone the example runtime bundle repository:

	```
	git clone https://github.com/Alfresco/example-process-application.git
	```

2. Clear out the example files in the directories under the `/project/` folder and insert the XML process definitions and JSON process extension files for the new application in their place. 

3. Edit the `Dockerfile` and set the location of where the XML and JSON files will be located in the created image. The default is `maven/processes`.

4. Update the value of `{DOCKER_REGISTRY}` in the `env.sh` file to point to the Docker registry of the Kubernetes namespace. 

5. Create and push the image using the following command: 

	```bash
	export DOCKER_IMAGE_TAG=<branch>
	./build.sh
	./push.sh
	```

## Set Helm chart values
The Helm chart values require updating to point to the correct custom image and Kubernetes namespace.

1. Clone the Helm chart: 

	```bash
	git clone https://github.com/Alfresco/alfresco-process-application-deployment
	```
	
2. Update the `values.yaml` file in `/helm/alfresco-process-application/` replacing the values with those relevant to the application deployment: 

	1. Update the gateway and identity hosts and the domain name of the Kubernetes cluster.

	2. If using a custom image then replace the location of the service with those pushed to the Docker registry. The following is an example of where to update the application runtime bundle location: 

		```yaml
    	runtime-bundle:
  		  enabled: true
  		...
  		image:
    	  repository: quay.io/alfresco/alfresco-process-runtime-bundle-service
    	  tag: "master"
  		...
		```

## Configure a client in the Identity Service 
A client needs to be created in the Identity Service as the application is deployed. Clients can also be updated or deleted by following the same process. 

1. Create a file called `application.json` that contains the following information and place it in the route of the Helm chart project: 

	* Update the users and groups for each role. At least one user or group needs to be assigned to each [role](../identity/README.md). 
	* Update the `name` of the client. The name specified in the `application.json` will need to match the name of the Helm chart when it is deployed.

	```json
	{
    "name": "{application-client-name}",
    "security": [
    {
      "role": "ACTIVITI_USER",
      "groups": [
        "{group-name}"
      ],
      "users": [
        "{user-name}"
      ]
    },
    {
      "role": "ACTIVITI_ADMIN",
      "groups": [],
      "users": [
        "{admin-name}"
      ]
    }
  	]
	}
	```

2. Run the following command to create, update or delete the image:

	* Update `{identity-service-url}` with the URL of the Identity Service in the Activiti Enterprise deployment.
	* Update `{action}` with the action to take on the client. The possible options are `create`, `update` and `delete`. The default is `create`. 

	```docker
	docker run -it --rm \
 	--env KEYCLOAK_AUTHSERVERURL={identity-service-url} \
  	--env ACT_KEYCLOAK_CLIENT_APP=admin-cli \
  	--env ACT_KEYCLOAK_CLIENT_USER=client \
  	--env ACT_KEYCLOAK_CLIENT_PASSWORD=client \
  	--env ACTIVITI_KEYCLOAK_CLI_COMMAND={action}
  	--volume "$PWD":/tmp/app \
  	quay.io/alfresco/alfresco-deployment-cli /tmp/app/application.json
	```
	
## Deploy using Helm
Once everything has been configured, the following command can be run to deploy the application with a few variables set:

* `name` needs to match the name used in the [`application.json`](#create-a-keycloak-client-in-the-identity-service)
* `domain.com` is the domain name to use. 
* `namespace` is the one [created for the application](#create-a-namespace).

```
helm upgrade name ./helm/alfresco-process-application --install --set global.gateway.domain=domain.com --namespace=namespace
```

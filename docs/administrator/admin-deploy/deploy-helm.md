---
Title: Helm deployment
---

# Helm deployment
To deploy an application using Helm rather than the deployment service [use this Helm chart](https://git.alfresco.com/process-services-public/alfresco-process-application-deployment/).  

## Prerequisites

* Activiti Enterprise is deployed in Kubernetes.

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

## (Optional) Create custom images 
Several services can be replaced with custom Docker images. Example projects are provided for the runtime bundle, form service and DMN service so that definition XML and JSON files can be placed in the images:

* [Runtime bundle](https://git.alfresco.com/process-services-public/alfresco-example-process-runtime-bundle-service/)
* [Form service](https://git.alfresco.com/process-services-public/alfresco-example-forms-service)
* [DMN service](https://git.alfresco.com/process-services-public/alfresco-example-dmn-service)

**Note**: The layout of each project is almost identical. The runtime bundle project will be used as an example.

1. Clone the example runtime bundle repository:

	```
	git clone https://git.alfresco.com/process-services-public/alfresco-example-process-runtime-bundle-service.git
	```

2. Clear out the example files in the `/processes/` folder and insert the XML process definitions and JSON process extension files for the new application in their place. 

3. Edit the `Dockerfile` and set the location of where the XML and JSON files will be located in the created image. The default is `maven/processes`.

4. Update the value of `{DOCKER_REGISTRY}` in the `env.sh` file to point to the Docker registry of the Kubernetes namespace. 

5. Create and push the runtime bundle image using the following command: 

	```bash
	export DOCKER_IMAGE_TAG=<branch>	./build.sh	./push.sh
	```

## Set Helm chart values
The Helm chart values require updating to point to the correct custom images and Kubernetes namespace.

1. Clone the Helm chart: 

	```bash
	git clone https://git.alfresco.com/process-services-public/alfresco-process-application-deployment/
	```
	
2. Update the `values.yaml` file in `/helm/alfresco-process-application/` replacing the values with those relevant to the application deployment: 

	1. Update the gateway and identity hosts and the domain name of the Kubernetes cluster.

	2. If using any custom images then replace the location of each with those pushed to the Docker. The following is an example of where to update the runtime bundle location: 

		```yaml
    	runtime-bundle:
  		  enabled: true
  		...
  		image:
    	  repository: activiti/example-runtime-bundle
    	  tag: "master"
  		...
		```
	
	3. Add the location of the XML and JSON files that were set when the custom images were created. The following is an example for the runtime bundle: 

		```yaml
		-  extraEnv: |    		- name: SPRING_ACTIVITI_PROCESSDEFINITIONLOCATIONPREFIX      	      value: "file:/process-definitions/"
		```

	**Note** The environment variables are different for each service:
	
	| Service | Environment variable | 
	| ------- | -------------------- |
	| Runtime bundle |  `SPRING_ACTIVITI_PROCESSDEFINITIONLOCATIONPREFIX` |
	| Form service | `FORMCONFIGURATION_DIRECTORYPATH` | 
	| DMN service | `ACTIVITI_CLOUD_DMN_DMNFILES` |

## Configure the Identity Service 
A client for the Identity Service needs to be created as the application is deployed.  

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

3. Run the following command to create the Identity Service image replacing the `KEYCLOAK_AUTHSERVERURL` with that of the Identity Service URL of the Activiti Enterprise deployment:  

	```docker
	docker run -it --rm \ 	--env KEYCLOAK_AUTHSERVERURL=https://identity.***/auth \  	--env ACT_KEYCLOAK_CLIENT_APP=admin-cli \  	--env ACT_KEYCLOAK_CLIENT_USER=client \  	--env ACT_KEYCLOAK_CLIENT_PASSWORD=client \  	--volume "$PWD":/tmp/app \  	quay.io/alfresco/alfresco-deployment-cli:master /tmp/app/application.json
	```

## Deploy using Helm
Once everything has been configured, the following command can be run to deploy the application with a few variables set:

* `name` needs to match the name used in the [`application.json`](#configure-the-identity-service)
* `domain.com` is the domain name to use. 
* `namespace` is the one [created for the application](#create-a-namespace).

```
helm upgrade name ./helm/alfresco-process-application --install --set global.gateway.domain=domain.com --namespace=namespace
```
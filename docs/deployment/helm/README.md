---
Title: Deploying with Helm
---

# Deploying with Helm 
Activiti Enterprise can be deployed using [Helm](https://helm.sh) charts. 

## Prerequisites 
A Helm deployment assumes that you have the following:

* [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
* A valid license for Activiti Enterprise.
* A Kubernetes cluster.
* A Docker registry for the [deployment service](../architecture/platform.md#deployment-service).
* Helm installed in the cluster. 
* An ingress bound to an external DNS address.  

## Deployment steps
Activiti Enterprise deployment can be split into four areas: 

1. Preparing the cluster:
	* [Set up Quay.io credentials](#set-up-quayio-credentials)
	* [Apply a license file to the cluster](#apply-a-license-to-the-cluster)
2. [Set variables for the deployment](#set-the-variables-for-the-deployment)
3. Configure any optional customizations:
	* [Include Alfresco Content Services (ACS)](#optional-include-alfresco-content-services-acs)
	* [Use external databases](#optional-use-external-databases)
	* [Use a custom realm file](#optional-use-a-custom-realm-file)
4. [Deploy to the cluster](#deploy-to-the-cluster) 

### Set up Quay.io credentials 
The resources for an Alfresco Activiti Enterprise deployment are stored in [Quay.io](https://quay.io/). These are password protected images and require a Kubernetes secret to be set up to access them:

**Note**: Quay.io credentials can be obtained by logging a ticket with [Alfresco support](https://support.alfresco.com).

1. Sign into Quay.io with your credentials using the following command: 
	
	```bash
	docker login quay.io
	```

2. Generate a base64 value for your dockercfg using one of the following commands:

	```bash
	# Linux
	cat ~/.docker/config.json | base64
	```
	
	```bash
	# Windows
	base64 -w 0 ~/.docker/config.json
	```

3. Create a file called `secrets.yaml` and add the following content to it, using your base64 string from the previous step as the `.dockerconfigjson` value: 

	```yaml
	apiVersion: v1
	kind: Secret
	metadata:
  	 name: quay-registry-secret
	type: kubernetes.io/dockerconfigjson
	data:
 	 .dockerconfigjson: <your-base64-string>
	```

4. Upload your `secrets.yaml` file to the namespace you are deploying into using the following command: 

	```bash
	kubectl create -f <file-location>/secrets.yaml --namespace=$DESIREDNAMESPACE
	```

### Apply a license to the cluster
A license file needs to be applied to use Alfresco Activiti Enterprise. It is created as a secret in Kubernetes and then referenced by the individual services. 

A Kubernetes secret can be created with your license file using command line commands or manually via the Kubernetes Dashboard. The secret must be called `licenseaps`.

For the namespace you are deploying to, run the following command to upload your license file:

```bash
kubectl create secret generic licenseaps --from-file=./activiti.lic
--namespace=$DESIREDNAMESPACE
```

**Note**: ensure that the Kubernetes secret is added to the correct namespace for your deployment.

### Set variables for the deployment 

1. Clone [this repository](https://github.com/Alfresco/alfresco-process-infrastructure-deployment) and make it your working directory.

2. Set the release name and the chart name:

	```bash
	RELEASE_NAME=infrastructure
	CHART_NAME=alfresco-process-infrastructure
	```

3. The `HELM_OPTS` variable is used throughout the deployment to include optional customizations to the chart before deploying it. 

	Declare `HELM_OPTS` and set the mandatory variables `{HTTP}` and `{DOMAIN}`: 
	* `{HTTP}` must be set as either `http` or `https` for the deployment.
	* `{DOMAIN}` must be the value of the domain to use for the deployment, for example: `aae.example.com`

	```bash
	export HELM_OPTS="--debug
  		--set global.gateway.http=${HTTP}
  		--set global.gateway.domain=${DOMAIN}"
	```
	
4. Configure the `secrets.yaml` so that the deployment service can access the Docker registry and Quay for images to create applications: 

	1. Edit `secrets.yaml` with the relevant credentials.

	2. Copy `secrets.yaml` to the route of the repository.

	3. Use the following command to add `secrets.yaml` to the deployment:

		```bash
		HELM_OPTS="${HELM_OPTS} -f secrets.yaml"
		```

### (Optional) Include Alfresco Content Services (ACS)
Alfresco Content Services (ACS) can be deployed with the infrastructure for the [process storage service](../architecture/platform.md#process-storage-service) to use for storing task and process data.

Use the following command to include ACS in the deployment: 

```bash
HELM_OPTS="${HELM_OPTS} -f values-alfresco-content-services.yaml"
```

**Note**: Enabling ACS also deploys the [NFS Provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs). 

### (Optional) Use external databases
External databases can be used for the [deployment service](../../architecture/platform.md#deployment-service) and [modeling service](../../architecture/platform.md#modeling-service) by setting custom values in `values-external-postgresql.yaml`.

Use the following command to include the external database configuration in the deployment:

```bash
HELM_OPTS="${HELM_OPTS} -f values-external-postgresql.yaml"
```

### (Optional) Use a custom realm file
A default realm file is included with the deployment, however a custom realm can be used instead. There are a number of [mandatory realm configurations](../helm/realm.md) that are required by Activiti Enterprise to include when creating a custom realm.

Edit the `values.yaml` to pull in a custom realm file, or edit the default file `alfresco-aps-realm.json` provided. 

Update the following section in the `values.yaml` to pull in a custom file: 

```yaml
keycloak:
 service:
 port: 80
  extraArgs: "-Dkeycloak.import=/realm/alfresco-aps-realm.json"
```

If the name of the default realm, client and resource are updated from `alfresco` and `activiti`, set the following in `HELM_OPTS`:

```bash
HELM_OPTS="${HELM_OPTS} 
--set global.keycloak.realm=${REALM} 
--set global.keycloak.client=${CLIENT}
--set global.keycloak.resource=${RESOURCE}"
```

### Deploy to the cluster
Use the following command from the root of repository to deploy into your cluster:

```bash
helm repo update
helm dependency update helm/${CHART_NAME}
helm upgrade --install \
${HELM_OPTS} ${RELEASE_NAME} helm/${CHART_NAME}
```
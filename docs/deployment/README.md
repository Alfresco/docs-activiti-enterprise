---
Title: Deployment
---

# Deployment

This page describes the options for deploying Alfresco Activiti Enterprise.

## Prerequisites

The following table details the technologies that are required and that you should be familiar with in order to successfully deploy Alfresco Activiti Enterprise on Amazon Web Services (AWS) Amazon Elastic Container Service for Kubernetes (EKS). 

**Note**: You can only install a single application with Alfresco Activiti Enterprise.

|Component|Getting Started Guide|
|---------|---------------------|
|Docker|https://docs.docker.com/|
|Helm|https://docs.helm.sh/using_helm/#quickstart-guide|
|Kubernetes|https://kubernetes.io/docs/tutorials/kubernetes-basics/|
|Kubectl|https://kubernetes.io/docs/tasks/tools/install-kubectl/|
|EKS|https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html|
|Kubernetes Dashboard|https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html|
|AWS CLI|https://github.com/aws/aws-cli#installation|

## Set up your cluster
Use an existing cluster to deploy Alfresco Activiti Enterprise into, or create a new one on AWS. If you are setting up a new cluster, the [Amazon EKS Getting Started guide](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html) takes you through the following initial steps:

* Creating an Amazon EKS Service Role
* Creating an Amazon EKS Cluster VPC
* Setting up kubectl for EKS authentication
* Creating an EKS Cluster
* Configuring kubectl to access the EKS Cluster
* Configuring and launching EKS worker nodes

## Kubernetes Dashboard
The Kubernetes Dashboard is a web-based user interface that allows you to manage your Kubernetes resources more easily. 

Follow the prerequisites and installation steps outlined in the AWS guide Deploy the Kubernetes Web UI.

## Helm
The server-side component of Helm called Tiller needs to be installed in the cluster in order to deploy any Helm charts into it. There are three options for granting Tiller permissions to the cluster. 

Running the following command will install Tiller into the cluster:

```bash
helm init 
```

The following command will update any linked repositories if you are running in an existing cluster:  

```bash
helm repo update
```

## Nginx Ingress
First of all decide which namespace to install Alfresco Activiti Enterprise into. If you do not specify a namespace using `--namespace=”my-namespace”` when running deployment commands then the default namespace will be used. Nginx Ingress then needs to be deployed to route traffic throughout the cluster.

Use the following commands to install version 1.1.5:  

```bash
export NGINX_INGRESS_RELEASE_NAME=nginx-ingress
```
```bash
helm install --name ${NGINX_INGRESS_RELEASE_NAME} stable/nginx-ingress --set-string controller.config.ssl-redirect=false --version 1.1.5
```
Note: using the RELEASE_NAME parameter avoids auto-generated release names such as frustrated-llama in your deployment.

Once Nginx Ingress has finished installing, run the following command to export the ELB address:

```bash
export ELB_ADDRESS=$(kubectl get services ${NGINX_INGRESS_RELEASE_NAME}-controller -o jsonpath={.status.loadBalancer.ingress[0].hostname})
```

## Route 53 configuration
Use AWS Route 53 to configure a Domain Name System (DNS) for your cluster. This will allow users of your deployment of Alfresco Activiti Enterprise to access it via DNS, for example: *my-software-company.com* rather than needing the IP address. 

1. Go to AWS Management Console and open the Route 53 console.
2. Click Hosted Zones in the left navigation panel, then Create Record Set.
3. In the Name field, enter your chosen DNS name prefixed with an asterisk to denote a wildcard entry. 
4. In the Alias Target, enter your ELB address from the output of the Nginx Ingress command ("$ELB_ADDRESS").
5. Click Create.

## HTTPS (optional)
Use the AWS certificate manager to configure an SSL certificate for your cluster if required. 

1. Set up a certificate in the Certificates section of AWS using the DNS entry configured in Route 53.
2. Go to the Load Balancer section on AWS and edit the entry for your cluster.
3. Amend the Instance Port for TCP 443 to be the Instance Port displayed for TCP 80.
4. Change the TCP Load Balancer Protocol for port 443 to HTTPS.
5. Assign the SSL Certificate to this entry. 
6. Remove all other entries so you are left with only a HTTPS entry.

## Quay.io credentials 
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

## Apply a license to the cluster
A license file needs to be applied to use Alfresco Activiti Enterprise. It is created as a secret in Kubernetes and then referenced by the individual services. 

A Kubernetes secret can be created with your license file using command line commands or manually via the Kubernetes Dashboard. The secret must be called licenseaps

For the namespace you are deploying to, run the following command to upload your license file:

```bash
kubectl create secret generic licenseaps --from-file=./activiti.lic
--namespace=$DESIREDNAMESPACE
```

**Note**: ensure that the Kubernetes secret is added to the correct namespace for your deployment.

To create the secret via the Kubernetes dashboard: 

1. Log into your Kubernetes dashboard
2. Ensure you are in the correct namespace
3. Select Secrets under Config and Storage
4. Upload your activiti.lic file and name your secret licenseaps

## Set variables for your deployment
First add the repositories that will be used to deploy Alfresco Activiti Enterprise using the following commands:

```bash
helm repo add activiti-cloud-charts https://activiti.github.io/activiti-cloud-charts
helm repo add alfresco https://kubernetes-charts.alfresco.com/stable
```

Secondly, clone the `alfresco-process-application-deployment` chart locally and make it your current working directory: 

```bash
git clone https://git.alfresco.com/process-services/alfresco-process-application-deployment.git
```

Next set the environment variables that will be used throughout the chart deployments, using the domain name and protocol configured in previous steps:

```bash
	export DOMAIN=”mydomain.com”
	export PROTOCOL=http
	if [[ "${PROTOCOL}" == "http" ]]; then export HTTP=true; else export HTTP=false; fi
```
```bash	
	export HELM_OPTS= --debug --dry-run
 	 --set global.gateway.http=${HTTP}
  	--set global.gateway.domain=${DOMAIN}
```

**Note**: the `--dry-run` argument doesn’t install a chart. It outputs what would have been installed to you so you may check it. This is an optional argument and will need to be removed from the `HELM_OPTS` variable to install any charts. 

```bash
export SSO_HOST=activiti-keycloak.${DOMAIN}
export SSO_URL=${PROTOCOL}://${SSO_HOST}/auth
export GATEWAY_HOST=activiti-cloud-gateway.${DOMAIN}
export GATEWAY_URL=${PROTOCOL}://${GATEWAY_HOST}
```

## Deploy Alfresco Activiti Enterprise infrastructure
The Alfresco Activiti Enterprise infrastructure needs to be deployed before the application itself. The infrastructure contains the Identity Service component used for authentication throughout the product.

Run the following commands to install the infrastructure: 

```bash
export CHART_REPO=alfresco
export CHART_NAME=alfresco-process-infrastructure
export RELEASE_NAME=infrastructure
```
```bash
helm upgrade --install ${HELM_OPTS} \
 --set alfresco-infrastructure.persistence.enabled=true \
 --set alfresco-content-services.enabled=false \
 ${RELEASE_NAME} ${CHART_REPO}/${CHART_NAME}
```
Once the pods are up and running, you can visit the Identity Service at:

`{PROTOCOL}://activiti-keycloak.{DOMAIN}/auth`

## Deploy Alfresco Activiti Modeling Application 
The modeling chart deploys all of the microservices required to run the Alfresco Modeling Application:

```bash
export CHART_REPO=activiti-cloud-charts
export CHART_NAME=activiti-cloud-modeling
export RELEASE_NAME=${CHART_NAME}
```
```bash
helm upgrade --install \
  ${HELM_OPTS} \
  -f values-global.yaml \
  -f values-${CHART_NAME}.yaml \
  ${RELEASE_NAME} ${CHART_REPO}/${CHART_NAME}
```
## Create an application and runtime bundle
Once you have the pods up and running for the modeling service, visit the following URL and use the Alfresco Modeling Application to model your application:

`{PROTOCOL}://activiti-cloud-gateway.{DOMAIN}/activiti-cloud-modeling`

The next step is to create a custom version of a runtime bundle that will include your newly modeled process(es) and application:

1. Download your XML process definitions from the Modeling Application.
2. Clone `https://git.alfresco.com/process-services-public/alfresco-process-application-deployment.git` 
3. Insert your process definitions into the processes folder.
4. Build your new Docker image and publish it to the repository of your choice.

## Deploy your application
Once you have modeled an application as well as created and published your own version of the runtime bundle then you need to amend the `values-activiti-to-aps.yaml` file in your local version of the `alfresco-process-application-deploy` repository.

For the runtime bundle, replace the repository location with your published runtime bundle:

```yaml
image:
    repository: quay.io/alfresco/alfresco-example-process-runtime-bundle-service
```

Export a name for your application:

```bash
export APP_NAME=”my-app-name”
```
Deploy your application: 

```bash
export RELEASE_NAME="${APP_NAME}"
export CHART_NAME="application"
export CHART_REPO="activiti-cloud-charts"
```
```bash
helm upgrade \
  --install \
  ${HELM_OPTS} \
  -f values-global.yaml \
  -f values-activiti-to-aps.yaml \
  ${RELEASE_NAME} ${CHART_REPO}/${CHART_NAME}
```

## Deploy the user interfaces
Once the application has been deployed, you can deploy the Alfresco Administrator Application and the Alfresco Process Workspace.

To deploy the Administrator Application run: 

```bash
export FRONTEND_APP_NAME="alfresco-admin-app"
```
```bash
 helm upgrade --install --wait \
  ${HELM_OPTS} \
  --set registryPullSecrets=quay-registry-secret \
  --set image.repository=quay.io/alfresco/${FRONTEND_APP_NAME} \
  --set image.tag=latest \
  --set image.pullPolicy=Always \
  --set ingress.hostName="${GATEWAY_HOST}" \
  --set ingress.path="/${FRONTEND_APP_NAME}" \
  --set env.APP_CONFIG_BPM_HOST="${GATEWAY_URL}" \
  --set env.API_URL="${GATEWAY_URL}" \
  --set env.APP_CONFIG_APPS_DEPLOYED="[{\"name\": \"${APP_NAME}\" }]" \
  --set env.APP_CONFIG_AUTH_TYPE="OAUTH" \
  --set env.APP_CONFIG_OAUTH2_HOST="${SSO_URL}/realms/${REALM}" \
  --set env.APP_CONFIG_IDENTITY_HOST="${SSO_URL}/admin/realms/${REALM}" \
  --set env.APP_CONFIG_OAUTH2_CLIENTID="activiti" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_SILENT_IFRAME_URI="${GATEWAY_URL}/${FRONTEND_APP_NAME}/assets/silent-refresh.html" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_LOGIN="/${FRONTEND_APP_NAME}/#" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_LOGOUT="/${FRONTEND_APP_NAME}/#" \
  ${FRONTEND_APP_NAME} alfresco-stable/alfresco-adf-app
```
To deploy the Process Workspace: 

```bash
export FRONTEND_APP_NAME="alfresco-process-workspace-app"
```
```bash
helm upgrade --install --wait \
  ${HELM_OPTS} \
  --set registryPullSecrets=quay-registry-secret \
  --set image.repository=quay.io/alfresco/${FRONTEND_APP_NAME} \
  --set image.tag=latest \
  --set image.pullPolicy=Always \
  --set ingress.hostName="${GATEWAY_HOST}" \
  --set ingress.path="/${FRONTEND_APP_NAME}" \
  --set env.APP_CONFIG_BPM_HOST="${GATEWAY_URL}" \
  --set env.API_URL="${GATEWAY_URL}" \
  --set env.APP_CONFIG_APPS_DEPLOYED="[{\"name\": \"${APP_NAME}\" }]" \
  --set env.APP_CONFIG_AUTH_TYPE="OAUTH" \
  --set env.APP_CONFIG_OAUTH2_HOST="${SSO_URL}/realms/${REALM}" \
  --set env.APP_CONFIG_IDENTITY_HOST="${SSO_URL}/admin/realms/${REALM}" \
  --set env.APP_CONFIG_OAUTH2_CLIENTID="activiti" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_SILENT_IFRAME_URI="${GATEWAY_URL}/${FRONTEND_APP_NAME}/assets/silent-refresh.html" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_LOGIN="/${FRONTEND_APP_NAME}/#" \
  --set env.APP_CONFIG_OAUTH2_REDIRECT_LOGOUT="/${FRONTEND_APP_NAME}/#" \
  ${FRONTEND_APP_NAME} alfresco-stable/alfresco-adf-app
```

## Post-deployment
You can check the status of your deployments at any time using: 

```bash
helm status {RELEASE_NAME}
```

After all pods have finished deploying successfully, you can view the interfaces at the following URLs:

* Process Workspace: `{PROTOCOL}://activiti-cloud-gateway.{DOMAIN}/alfresco-process-app`
* Admin: `{PROTOCOL}://activiti-cloud-gateway.{DOMAIN}/alfresco-admin-app`
* GraphIQL: `{PROTOCOL}://activiti-cloud-gateway.{DOMAIN}/graphiql`

For example: `https://activiti-cloud-gateway.alfresco.com/alfresco-admin-app`

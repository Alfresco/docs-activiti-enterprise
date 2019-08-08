---
Title: Deployment
---

# Deploying Alfresco Activiti Enterprise
The deployment for Alfresco Activiti Enterprise on a generic [Kubernetes](https://kubernetes.io/) cluster can be accomplished using [Terraform](https://www.terraform.io/). 

## Prerequisites
The following are prerequisites for deploying Activiti Enterprise: 

* [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
* A valid license for Activiti Enterprise. 
* Installed [Docker](https://docs.docker.com/get-started/).
* Installed [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Installed [Helm](https://helm.sh/docs/using_helm/#installing-helm).
* Installed [Terraform](https://learn.hashicorp.com/terraform/getting-started/install.html).

## Deployment
The following steps describe a deployment into an existing, generic Kubernetes cluster deployment. Examples are also available for:

* [Creating a full environment on Amazon Web Services (AWS)](https://git.alfresco.com/process-services-public/terraform-alfresco-process/tree/develop/examples/eks) with Activiti Enterprise deployed onto an EKS cluster. 
*  [Creating a Rancher 2 environment on Amazon Web Services (AWS)](https://git.alfresco.com/process-services-public/terraform-alfresco-process/tree/develop/examples/rancher-eks) with Activiti Enterprise deployed onto an EKS cluster. 

The generic Kubernetes cluster deployment assumes that you have the following already set up:

* A Kubernetes cluster.
* A load balancer with DNS entries and (if applicable) SSL certificates configured.
* A Docker registry.

### Deployment steps
1. Clone [this repository](https://git.alfresco.com/process-services-public/alfresco-process-terraform) and make it your working directory.  

2. Initialize Terraform using the following command if you have not already done so whilst verifying the Terraform plugin: 

	```bash
	terraform init
	```
	
3. Create a copy of the file `terraform_template.tfvars` and name it `terraform.tfvars`. This retains the original template and allows you to work on your own copy of it. 
	
4. Edit the `terraform.tfvars` file and replace the template variables with those relevant to your deployment: 

	| Variable | Description | Default | Required | 
	| -------- | ----------- | ------- | -------- | 
	| `acs_enabled` | Set whether Alfresco Content Services is installed as part of the infrastructure. This is a boolean value | `true` | No | 
	| `aps2_license` | The location of a license file for Activiti Enterprise || Yes |
	| `aws_efs_dns_name` | The EFS DNS name used for Alfresco Content Services storage. Applicable to AWS deployments only. Requires `acs_enabled` to be set to `true` || No |
	| `cluster_name` | The name for the cluster. If left blank it will be a concatenation of `project_name` and `project_environment` || No |
	| `gateway_host` | The gateway host name || Yes | 
	| `helm_service_account` | The service account to be used by Helm | `tiller` | No | 
	| `http` | Set whether to use http. This is a boolean value | `false` | No | 
	| `kubernetes_api_server` | The API server URL for Kubernetes. This value is set in *Step 5* | `https://kubernetes` | No | 	
	| `kubernetes_token` | The token for the `kubernetes_api_server`. This value is set in *Step 5* || No | 	
	| `project_environment` | The environment type of the deployment, for example `test` or `production` || Yes |
	| `project_name` | The name of the project to appear in the cluster || Yes | 	| `quay_url` | The URL of Quay: `quay.io` | `quay.io` | Yes |
	| `quay_user` | The username of the Quay account to pull images with || Yes |
	| `quay_password` | The password for the `quay_user` || Yes |
	| `registry_host` | The Docker registry host name to use || Yes |
	| `registry_user` | The username to use for the Docker registry | `registry` | No |
	| `registry_password` |	 The password for `registry_user` | `password` | No |
	| `zone_domain` | The zone domain name || Yes |
  
5. Use the following command to populate the variables `kubernetes_api_server` and `kubernetes_token` in the `terraform.tfvars` file :

	```bash
NAMESPACE=kube-system
 SERVICEACCOUNT=alfresco-deployment-service
 kubectl create serviceaccount -n kube-system ${SERVICEACCOUNT}
 kubectl create clusterrolebinding ${SERVICEACCOUNT}-admin-binding --clusterrole cluster-admin --serviceaccount=${NAMESPACE}:${SERVICEACCOUNT}
 echo "kubernetes_token = \"$(kubectl -n ${NAMESPACE} get secret $(kubectl -n ${NAMESPACE} get serviceaccount ${SERVICEACCOUNT} -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode)\"" >> terraform.tfvars

	```

8. Use the following command to complete the deployment:

	```bash
	terraform apply
	``` 
	
**Note**: For reference Terraform stores the state of a cluster deployment in the file `terraform.tfstate`. It is possible to [store this in a remote location](https://learn.hashicorp.com/terraform/getting-started/remote.html) such as an S3 bucket.
---
Title: Deploying with Terraform
---

# Deploying with Terraform
[Terraform](https://www.terraform.io/) is a tool for building infrastructure. Activiti Enterprise can be deployed using Terraform scripts with three different options provided. The options include examples of deploying to [Amazon Web Services (AWS)](https://aws.amazon.com/) and utilizing [Rancher](https://rancher.com/) as a tool to assist in managing Kubernetes resources.

There are three options for deploying using Terraform: 

* [Deployment with Rancher on Amazon EKS](#deployment-with-rancher-on-amazon-eks)
* [Deployment on Amazon EKS](#deployment-on-amazon-eks)
* [Deployment into an existing Kubernetes cluster](#deployment-into-an-existing-kubernetes-cluster)


## Deployment with Rancher on Amazon EKS 
The following steps describe the deployment of Activiti Enterprise on Amazon Elastic Kubernetes Service (Amazon EKS) with Terraform and utilizing Rancher for managing the Kubernetes resources. An Amazon EKS cluster is created as part of the build and the resources will be available to monitor in Rancher.

### Prerequisites
The Rancher on Amazon EKS deployment assumes that you have the following:

* [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
* A valid license for Activiti Enterprise. 
* Administrator access to an Amazon Web Services (AWS) account.
* Terraform *version 0.11*
* Rancher *version 2.0*

### Deployment steps

1. Clone [this repository](https://git.alfresco.com/process-services-public/alfresco-process-terraform) and make the `rancher-eks` folder your working directory.  

2. Initialize Terraform using the following command if you have not already done so whilst verifying the Terraform plugin: 

	```bash
	terraform init
	```

3. Create a copy of the file `terraform_template.tfvars` and name it `terraform.tfvars`. This retains the original template and allows you to work on your own copy of it. 

4. Use the following command to create an EKS cluster on Rancher: 

	```
	terraform apply -target=rancher2_cluster.aae-cluster
	```
	
5. Edit the `terraform.tfvars` file and replace the template variables with those relevant to your deployment: 

	| Variable | Description | Default | Required | 
	| -------- | ----------- | ------- | -------- | 
	| `aae_license` | The location of a license file for Activiti Enterprise || Yes |
	| `acs_enabled` | Set whether Alfresco Content Services is installed as part of the infrastructure. This is a boolean value | `true` | No | 
	| `aws_region` | The region of AWS to create the EKS cluster in || Yes | 
	| `aws_access_key_id` | The AWS access key ID for the AWS account used || Yes |
	| `aws_secret_access_key` | The AWS secret key for the AWS account used || Yes |
	| `cluster_name` | The name for the cluster. If left blank it will be a concatenation of `project_name` and `project_environment` || No |
	| `gateway_host` | The gateway host name || No | 
	| `identity_host` | The name of the identity server host || No |
	| `kubernetes_api_server` | The API server URL for Kubernetes. This value is set in *Step 7* | `https://kubernetes` | No | 	
	| `kubernetes_token` | The token for the `kubernetes_api_server`. This value is set in *Step 7* || No | 	
	| `my_ip_address` | The CIDR blocks that will have SSH access to the nodes in the cluster | `0.0.0.0/0` | No |
	| `project_environment` | The environment type of the deployment, for example `test` or `production` || Yes |
	| `project_name` | The name of the project to appear in the cluster || Yes | 	| `quay_url` | The URL of Quay: `quay.io` | `quay.io` | Yes |
	| `quay_user` | The username of the Quay account to pull images with || Yes |
	| `quay_password` | The password for the `quay_user` || Yes |
	| `rancher2_url` | The URL of the Rancher server || Yes |
	| `rancher2_access_key` | The access key for the Rancher instance used. This can be obtained in the **API & Keys** menu within the Rancher instance or using the `/apikeys` API || Yes |
	| `rancher2_secret_key` | The secret key for the Rancher instance used. This can be obtained in the **API & Keys** menu within the Rancher instance or using the `/apikeys` API
	| `registry_host` | The Docker registry host name to use || Yes |
	| `registry_user` | The username to use for the Docker registry | `registry` | No |
	| `registry_password` |	 The password for `registry_user` | `password` | No |
	| `ssh_username` |  | `aae` | No |
	| `ssh_public_key` | The public key for SSH authentication on the nodes || No |
	| `zone_domain` | The zone domain name || Yes |
	
6. Use the following command to create a `kubeconfig` file for the new EKS cluster:

	```bash
 	export KUBECONFIG=$PWD/.terraform/kubeconfig
 	echo "$(terraform output kube_config)" > $KUBECONFIG
 	kubectl cluster-info
	```

7. Use the following command to populate the variables `kubernetes_api_server` and `kubernetes_token` in the `terraform.tfvars` file:

	```bash
 echo "kubernetes_api_server = \"$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}')\"" >> terraform.tfvars
 NAMESPACE=kube-system
 SERVICEACCOUNT=default
 kubectl create serviceaccount -n kube-system ${SERVICEACCOUNT}
 kubectl create clusterrolebinding ${SERVICEACCOUNT}-admin-binding --clusterrole cluster-admin --serviceaccount=${NAMESPACE}:${SERVICEACCOUNT}
 echo "kubernetes_token = \"$(kubectl -n ${NAMESPACE} get secret $(kubectl -n ${NAMESPACE} get serviceaccount ${SERVICEACCOUNT} -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode)\"" >> terraform.tfvars
	```

8. Use the following command to populate the `my_ip_address` variable in the `terraform.tfvars` file and restrict SSH access to the Kubernetes nodes:

	```bash
	echo "my_ip_address = \"$(curl https://ipecho.net/plain)/32\"" >> terraform.tfvars
	```

9. Use the following command to complete the deployment:

	```bash
	terraform apply
	``` 
**Note**: For reference Terraform stores the state of a cluster deployment in the file `terraform.tfstate`. It is possible to [store this in a remote location](https://learn.hashicorp.com/terraform/getting-started/remote.html) such as an S3 bucket.

## Deployment on Amazon EKS
The following steps describe an example of deploying Activiti Enterprise on Amazon Elastic Kubernetes Service (Amazon EKS) with Terraform. An Amazon EKS cluster is created as part of the build.

### Prerequisites
The Amazon EKS deployment assumes that you have the following:

* [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
* A valid license for Activiti Enterprise. 
* Administrator access to an Amazon Web Services (AWS) account.
* Terraform *version 0.11*
* [eksctl](https://github.com/weaveworks/eksctl) installed. 

### Deployment steps

1. Clone [this repository](https://git.alfresco.com/process-services-public/alfresco-process-terraform) and make the `eks` folder your working directory.  

2. Initialize Terraform using the following command if you have not already done so whilst verifying the Terraform plugin: 

	```bash
	terraform init
	```
	
3. Create a copy of the file `terraform_template.tfvars` and name it `terraform.tfvars`. This retains the original template and allows you to work on your own copy of it. 
	
4. Edit the `terraform.tfvars` file and replace the template variables with those relevant to your deployment: 

	| Variable | Description | Default | Required | 
	| -------- | ----------- | ------- | -------- | 
	| `aae_license` | The location of a license file for Activiti Enterprise || Yes |
	| `acs_enabled` | Set whether Alfresco Content Services is installed as part of the infrastructure. This is a boolean value | `true` | No | 
	| `aws_region` | The region of AWS to create the EKS cluster in || Yes | 
	| `aws_access_key_id` | The AWS access key ID for the AWS account used || Yes |
	| `aws_secret_access_key` | The AWS secret key for the AWS account used || Yes |
	| `cluster_name` | The name for the cluster. If left blank it will be a concatenation of `project_name` and `project_environment` || No |
	| `gateway_host` | The gateway host name || No | 
	| `identity_host` | The name of the identity server host || No |
	| `kubernetes_api_server` | The API server URL for Kubernetes. This value is set in *Step 7* | `https://kubernetes` | No | 	
	| `kubernetes_token` | The token for the `kubernetes_api_server`. This value is set in *Step 7* || No | 	
	| `my_ip_address` | The CIDR blocks that will have SSH access to the nodes in the cluster | `0.0.0.0/0` | No |
	| `node_groupname` | The name to assign the group of worker nodes | `ng-1` | No | 
	| `project_environment` | The environment type of the deployment, for example `test` or `production` || Yes |
	| `project_name` | The name of the project to appear in the cluster || Yes | 	| `quay_url` | The URL of Quay: `quay.io` | `quay.io` | Yes |
	| `quay_user` | The username of the Quay account to pull images with || Yes |
	| `quay_password` | The password for the `quay_user` || Yes |
	| `registry_host` | The Docker registry host name to use || Yes |
	| `registry_user` | The username to use for the Docker registry | `registry` | No |
	| `registry_password` |	 The password for `registry_user` | `password` | No |
	| `zone_domain` | The zone domain name || Yes |
	
5. Edit the `cluster.yaml` file and ensure that at a minimum the following variables match with those set in `terraform.tfvars`:

	| `cluster.yaml` variable | `terraform.tfvars` variable |
	| ----------------------- | --------------------------- | 
	| `name` | A concatenation of `project_name` + `project_environment` |
	| `region` | `aws_region` | 
	| `nodeGroups.name` | `node_groupname` |

6. Create the EKS cluster using the following command:

	```
	eksctl create cluster -f cluster.yaml
	```

7. Use the following command to populate the variables `kubernetes_api_server` and `kubernetes_token` in the `terraform.tfvars` file:

	```bash
 echo "kubernetes_api_server = \"$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}')\"" >> terraform.tfvars
 NAMESPACE=kube-system
 SERVICEACCOUNT=default
 kubectl create serviceaccount -n kube-system ${SERVICEACCOUNT}
 kubectl create clusterrolebinding ${SERVICEACCOUNT}-admin-binding --clusterrole cluster-admin --serviceaccount=${NAMESPACE}:${SERVICEACCOUNT}
 echo "kubernetes_token = \"$(kubectl -n ${NAMESPACE} get secret $(kubectl -n ${NAMESPACE} get serviceaccount ${SERVICEACCOUNT} -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 --decode)\"" >> terraform.tfvars
	```
	
8. Use the following command to populate the `my_ip_address` variable in the `terraform.tfvars` file and restrict SSH access to the Kubernetes nodes:

	```bash
	echo "my_ip_address = \"$(curl https://ipecho.net/plain)/32\"" >> terraform.tfvars
	```
	
9. Use the following command to create a `kubeconfig` file for the new EKS cluster:

	```bash
	eksctl utils write-kubeconfig --name=<cluser_name> --kubeconfig=$PWD/kubeconfig --set-kubeconfig-context=true
    export KUBECONFIG=$PWD/kubeconfig
    kubectl cluster-info
	```

10. Use the following command to complete the deployment:

	```bash
	terraform apply
	``` 
	
**Note**: For reference Terraform stores the state of a cluster deployment in the file `terraform.tfstate`. It is possible to [store this in a remote location](https://learn.hashicorp.com/terraform/getting-started/remote.html) such as an S3 bucket.

## Deployment into an existing Kubernetes cluster
The following steps describe an example of deploying Activiti Enterprise into a generic Kubernetes cluster. An existing cluster must exist to deploy into when following these steps.

### Prerequisites
The generic Kubernetes cluster deployment assumes that you have the following:

* [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
* A valid license for Activiti Enterprise. 
* A load balancer with DNS entries and SSL certificates configured.
* A Docker registry.
* A Kubernetes cluster running *version 1.12*
* Terraform *version 0.11*

### Deployment steps
The following steps describe a deployment into an existing, generic Kubernetes cluster.

1. Clone [this repository](https://git.alfresco.com/process-services-public/alfresco-process-terraform) and make it your working directory.  

2. Initialize Terraform using the following command if you have not already done so whilst verifying the Terraform plugin: 

	```bash
	terraform init
	```
	
3. Create a copy of the file `terraform_template.tfvars` and name it `terraform.tfvars`. This retains the original template and allows you to work on your own copy of it. 
	
4. Edit the `terraform.tfvars` file and replace the template variables with those relevant to your deployment: 

	| Variable | Description | Default | Required | 
	| -------- | ----------- | ------- | -------- | 
	| `aae_license` | The location of a license file for Activiti Enterprise || Yes |
	| `acs_enabled` | Set whether Alfresco Content Services is installed as part of the infrastructure. This is a boolean value | `true` | No | 
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
  
5. Use the following command to populate the variables `kubernetes_api_server` and `kubernetes_token` in the `terraform.tfvars` file:

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
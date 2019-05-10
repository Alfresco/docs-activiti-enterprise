---
Title: Deployment
---

# Deploying Alfresco Activiti Enterprise
The deployment for Alfresco Activiti Enterprise on Amazon Web Services (AWS) uses [Rancher](https://rancher.com/) and [Terraform](https://www.terraform.io/). 

## Prerequisites
The following are prerequisites for deploying Activiti Enterprise: 

- [Quay.io](https://quay.io) credentials to access Activiti Enterprise images. 
- [AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) for authenticating against AWS during deployment.
- Installed [Docker](https://docs.docker.com/get-started/).
- Installed [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
- Installed [Helm](https://helm.sh/docs/using_helm/#installing-helm).
- Installed [Terraform](https://learn.hashicorp.com/terraform/getting-started/install.html).
- Deployed a [high availability installation of Rancher 2](https://rancher.com/docs/rancher/v2.x/en/installation/ha/). 
- Installed the Terraform plugin for Rancher: 
	- [Download the correct version](https://github.com/rancher/terraform-provider-rancher2/releases/tag/v0.2.0-rc5) of the Terraform plugin for Rancher 2 for your operating system. 
		- Unzip and move the downloaded files to: `$HOME/.terraform.d/plugins/terraform-provider-rancher2_v0.2.0-rc5`.
		- For iOS and Linux:
			- Use `chmod +x` to make the file exectuable. 
	- In Terminal or a command prompt run `terraform init` to verify the installation. 

## Deployment
Use the following set of instructions to deploy Activiti Enterprise into an AWS EKS cluster using Rancher and Terraform: 

1. Clone [this repository](https://git.alfresco.com/process-services-public/alfresco-process-terraform) and make it your working directory.  
2. Initialize Terraform using the following command if you have not already done so whilst verifying the Terraform plugin: 

	```bash
	terraform init
	```
	
3. Create a copy of the file `terraform_template.tfvars` and name it `terraform.tfvars`. This retains the original template and allows you to work on your own copy of it. 
	
4. Edit the `terraform.tfvars` file and replace the template variables with those relevant to your deployment: 

	| Variable | Description |
	| -------- | ----------- |
	| `rancher2_url` | The URL of your Rancher server |
	| `rancher2_access_key`  | The access key of a user that can create clusters in Rancher. This can be created using the **API & Keys** section in Rancher |
	| `rancher2_secret_key`  | The secret key of a user that can create clusters in Rancher. This can be created using the **API & Keys** section in Rancher | 
	| `aws_region` | The AWS region to deploy to | 
	| `aws_access_key_id` | The AWS access key ID for the AWS account to deploy with |
	| `aws_secret_access_key` | The AWS access key for the AWS account to deploy with |
	| `quay_email` | The email address of the Quay account to pull images with | 
	| `quay_url` | The URL of Quay: `quay.io` | 
	| `quay_user` | The username of the Quay account to pull images with |
	| `quay_password` | The password for the `quay_user` |
	| `project_name` | The name of the project to appear in Rancher | 
	| `project_environment` | The environment type of the deployment, for example `test` or `production` | 
	| `cluster_name` | The name for the cluster. If left blank it will be a concatenation of `project_name` and `project_environment` | 
	| `cluster_description` | An optional description for the cluster | 
	| `kubernetes_api_server` | *This value will be set during the deployment once an EKS instance is running* | 
	| `kubernetes_token` | *This value will be set during the deployment once an EKS instance is running* | 
	| `aws_zone_domain` | The AWS Route 53 domain name |
	| `aws_zone_id` | The AWS Route 53 DNS ID |
	| `aps2_license` | The location of a license file for Activiti Enterprise |

5. Use the following command to deploy an EKS cluster on Rancher:

	```bash
	terraform apply -target=rancher2_cluster.aps2-cluster
	```
	**Note**: This process will take approximately 20 minutes to complete whilst all the resources are created in AWS.

6. Use the following commands to create a [kubeconfig file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) for your new cluster: 

	```bash
	export KUBECONFIG=$PWD/.terraform/kubeconfig
 	echo "$(terraform output kube_config)" > $KUBECONFIG
 	kubectl cluster-info
 	```
  
7. Use the following command to populate the variables `kubernetes_api_server` and `kubernetes_token` in the `terraform.tfvars` file now that your cluster is running:

	```bash
	echo "kubernetes_api_server = \"$(kubectl config view --minify -o jsonpath='{.clusters[0].cluster.server}')\"" >> terraform.tfvars
 	echo "kubernetes_token = \"$(kubectl config view --minify -o jsonpath='{.users[0].user.token}')\"" >> terraform.tfvars
	```

8. Use the following command to complete the deployment of Activiti Enterprise:

	```bash
	terraform apply
	``` 
	
**Note**: For reference Terraform stores the state of a cluster deployment in the file `terraform.tfstate`. It is possible to [store this in a remote location](https://learn.hashicorp.com/terraform/getting-started/remote.html) such as an S3 bucket. 

## Post-deployment
Once you have deployed Activiti Enterprise, the following user interfaces and APIs are available:

- The [Alfresco Modeling Application](../modeling/README.md) to design project and process definitions, including forms, decision tables and connectors.
 
- The [Alfresco Administrator Application](../administrator/README.md) to deploy projects once they have been designed and released in the Modeling Application.

- The [Alfresco Process Workspace](../workspace/README.md) for end-users to claim and complete process instances and tasks that require human input. 

- APIs at a [platform](../apis/README.md#platform-endpoints) and [application](../apis/README.md#application-endpoints) level. 

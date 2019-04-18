---
Title: Monitoring applications
---

# Monitoring applications
The **Application instances** section is for monitoring the status of deployed applications. Users require the *APS_DEVOPS* role to view application instances.

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

An application that has been successfully deployed will use the application name for [API queries](../apis/README.md). 
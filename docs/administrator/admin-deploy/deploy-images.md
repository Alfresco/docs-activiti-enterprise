---
Title: Deploying with custom images
---

# Deploying with custom images
It is possible to replace the Activiti Enterprise images for certain services with custom images based on the [Activiti 7 starters](http://www.activiti.org/). The services that can be replaced are the following:

* [Audit service](https://github.com/Activiti/activiti-cloud-audit-service)
* [Query service](https://github.com/Activiti/activiti-cloud-query-service)
* [Runtime Bundle](https://github.com/Activiti/activiti-cloud-runtime-bundle-service)

## Deployment steps
Custom images need to be pushed to the Kubernetes registry before deploying an application that uses them:

```
docker push registry.$DNSNAME/<custom-docker-image-name>:master
```

Once a custom image is in the Kubernetes registry of your deployment, the location can be set for the service it is replacing and any custom variables configured during [application deployment](../admin-deploy/README.md#deployment-steps).

## Other services
Other services such as the DMN, Form, Preference and Process Storage services can have their images replaced with custom ones. These services do not have Activiti 7 starters available to them and will need to be created manually. 

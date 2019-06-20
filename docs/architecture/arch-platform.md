---
Title: Platform services
---

# Platform services
Platform services are those Services that are deployed into a single Kubernetes namespace and are used by Activiti Enterprise to manage applications and services across the entire deployment, irrespective of how many individual applications are deployed.

## Modeling service
The modeling service contains the backend functionality required for the [Alfresco Modeling Application](../modeling/README.md) to run. It requires an instance of [Alfresco Community Edition](#alfresco-community-edition) deployed with it for storing project and model definitions that the [deployment service](#deployment-service) uses to deploy projects. 

The REST APIs that the modeling service exposes deal with projects, models and releases. 

The following is a high level diagram of the modeling service:

![Modeling service diagram](../images/arch-modeling.png)

## Alfresco Community Edition 
An instance of [Alfresco Community Edition](https://docs.alfresco.com/community/concepts/welcome-infocenter_community.html) is deployed as part of Activiti Enterprise to store the project and model definitions created in the [Alfresco Modeling Application](../modeling/README.md) and used by the [Alfresco Administrator Application](../administrator/README.md) to deploy them.

## Deployment service
The deployment service uses the Docker API to build an image for the runtime bundle. It also creates images for the form service and DMN service depending on whether there are any forms or decision tables present in the released project. The deployment service then uses the Kubernetes API to deploy all the images that make up a released project. 

This service uses the project and model definitions created using the [Alfresco Modeling Application](../modeling/README.md) and stored in an instance of [Alfresco Community Edition](#alfresco-community-edition) to deploy the [application level services](../architecture/arch-application.md) into their own namespaces. The [Alfresco Administrator Application](../administrator/README.md) calls the deployment service when managing projects and applications.

By default, the data for the deployment service is stored in a Postgres database called **alfresco-deployment**.

The following is a high level diagram of the modeling service:

![Deployment service diagram](../images/arch-deploy.png)

The REST APIs that the deployment service exposes deal with applications.

## Alfresco Content Services
An instance of [Alfresco Content Services (ACS)](https://docs.alfresco.com/6.1/references/whats-new.html) is deployed as part of Alfresco Activiti Enterprise to store information about in progress tasks and processes in a content repository. ACS is not required to use Activiti Enterprise and the data for tasks and processes is still stored in databases.

## Alfresco Identity Service
Alfresco Activiti Enterprise uses the [Alfresco Identity Service](https://docs.alfresco.com/identity/concepts/identity-overview.html) for authentication and user and role management throughout the product. Authentication can be [configured](http://docs.alfresco.com/identity/concepts/identity-configure.html) to external identity provider instances such as LDAP and SAML. 

The [Alfresco Administrator Application](../administrator/admin-identity/README.md) allows administrators to manage common user-related functions without needing to access the Identity Service. 

---
Title: Platform services
---

# Platform services
Platform services are those that are deployed into a default namespace within Kubernetes and are used by Activiti Enterprise to manage applications and services across the entire deployment, irrespective of how many individual applications are deployed.

## Modeling service
The modeling service contains the backend functionality required for the [Alfresco Modeling Application](../modeling/README.md) to run. It requires an instance of [Alfresco Community Edition](#alfresco-community-edition) deployed with it for storing project and model definitions that the [deployment service](#deployment-service) uses to deploy projects. 

The REST APIs that the modeling service exposes deal with projects, models and releases. 

## Alfresco Community Edition 
An instance of [Alfresco Community Edition](https://docs.alfresco.com/community/concepts/welcome-infocenter_community.html) is deployed as part of Activiti Enterprise to store the project and model definitions created in the [Alfresco Modeling Application](../modeling/README.md) and used by the [Alfresco Administrator Application](../administrator/README.md) to deploy them.

## Deployment service
The deployment service uses the Kubernetes API to deploy application instances. This service uses the project and model definitions created using the [Alfresco Modeling Application](../modeling/README.md) and stored in an instance of [Alfresco Community Edition](#alfresco-community-edition) to deploy the [application level services](#application-level-services) into their own namespaces. The [Alfresco Administrator Application](../administrator/README.md) calls the deployment service when managing projects and applications.

By default, the data for the deployment service is stored in a Postgres database called **alfresco-deployment**.

The REST APIs that the deployment service exposes deal with applications.

## Alfresco Identity Service
Alfresco Activiti Enterprise uses the [Alfresco Identity Service](https://docs.alfresco.com/identity/concepts/identity-overview.html) for authentication and user and role management throughout the product. Authentication can be [configured](http://docs.alfresco.com/identity/concepts/identity-configure.html) to external identity provider instances such as LDAP and SAML. 

The [Alfresco Administrator Application](../administrator/admin-identity/README.md) allows administrators to manage common user-related functions without needing to access the Identity Service. 
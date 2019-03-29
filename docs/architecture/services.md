---
Title: Services
---

# Services
Alfresco Process Services is comprised of the following microservices and components. 

## Runtime bundle
A runtime bundle represents a stateless instance of the process engine that executes an immutable set of process definitions. It is important to note that you cannot deploy new process definitions to an existing runtime bundle, nor can you update a process definition within it. Instead, you create a new version of your runtime bundle that contains the updates. 

Runtime bundles expose a synchronous REST API and an asynchronous message-based API. Each runtime bundle contains a single application and each application is deployed to its own namespace. 

Runtime bundles emit and consume events that occur within processes via Spring Cloud Streams.

By default, the data from the runtime bundle is stored in a Postgres database that is shared within an application namespace between the audit service, query service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster. 

## Query service
The query service is used for querying process data without accessing a runtime bundle directly. It stores events routed via Spring Cloud Streams (through the Rabbit MQ binder) into its own data store for simple, read-only process and task querying. The query service performs some data transformation, whilst the audit service does not. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster. 

## Audit service
The audit service stores events routed via Spring Cloud Streams (through the Rabbit MQ binder) into its own data store, without any data manipulation. This allows for audit events at an application level to be queried (in a read-only fashion) without contacting a runtime bundle directly. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service and runtime bundle using separate schemas. This can be updated to write to a Postgres data store external to an application’s namespace or even external to the cluster.

## Notification Service
The notification service reads from the same data store as the query service, however it utilises [GraphQL](https://graphql.org/learn/) so that queries can be configured against specific events.

## Modeling service
The modeling service contains the backend functionality required for the [Alfresco Modeling Application](../user-interfaces/ui-modeling.md). 

## Form service
The form service contains the backend functionality required for [forms](../modeling/modeling-forms.md).

## Process storage service
The process storage service stores processes and tasks as nodes within an Alfresco Content Services (ACS) repository. 

The user that creates these nodes in ACS is **service-account-storage-service**.   

## Deployment service
The deployment service uses the Kubernetes API to deploy application instances. This service uses the project models created using the Alfresco Modeling Application to deploy the relevant services and components into their own namespaces. 

The deployment service itself resides in the default namespace in Kubernetes. By default, the data for the deployment service is stored in a Postgres database called **alfresco-deployment**.

## Cloud Connectors
Connectors allow for two-way integration between external systems and a runtime bundle. This is the default way to handle service tasks within a process. Connectors are created, stored and executed separately from process instances.

When a service task is executed as part of a process instance, the runtime bundle will emit an integration event that is picked up by a connector which performs the integration with a response being routed back to the process instance so the flow may continue.

## Rabbit MQ
[Rabbit MQ](https://www.rabbitmq.com/) is the default message broker deployed with Alfresco Process Services that routes the events emitted by the runtime bundle asynchronously to other relevant microservices such as the audit and query services. 

Rabbit MQ is the default implementation, but can be replaced by a different message broker.  

## Alfresco Identity Service
Alfresco Process Services uses the [Alfresco Identity Service](https://docs.alfresco.com/identity/concepts/identity-overview.html) for authentication and user and role management throughout the product. Authentication can be [configured](http://docs.alfresco.com/identity/concepts/identity-configure.html) to external identity provider instances such as LDAP and SAML. 
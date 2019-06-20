---
Title: Application services
---

# Application services
Application services are those that are deployed into a Kubernetes namespace as part of an application. There is a 1:1 relationship between Kubernetes namespaces and applications. Each application that is deployed will contain application level services specific to only that application in its own Kubernetes namespace. 

The application name therefore forms part of the REST API endpoints. 

## Runtime bundle
A runtime bundle represents a stateless instance of the process engine that executes an immutable set of process definitions. It is important to note that you cannot deploy new process definitions to an existing runtime bundle, nor can you update a process definition within it. Instead, you create a new version of your runtime bundle that contains the updates. 

Runtime bundles expose a synchronous REST API and an asynchronous message-based API. Each runtime bundle contains a single application and each application is deployed to its own namespace. 

Runtime bundles emit and consume events that occur within processes via Spring Cloud Streams.

By default, the data from the runtime bundle is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster. 

The REST APIs that the runtime bundle exposes deal with processes and tasks and are specific to each application. 

## Query service
The query service is used for querying process data without accessing a [runtime bundle](#runtime-bundle) directly. It stores events routed via Spring Cloud Streams (through the [Rabbit MQ](#rabbit-mq) binder) into its own data store for simple, read-only process and task querying. The query service performs some data transformation, whilst the [audit service](#audit-service) does not. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster. 

The REST APIs that the query service exposes deal with processes and tasks and are specific to each application. 

## Form service
The form service contains the backend functionality required for [forms](../modeling/modeling-forms/README.md) to function within an application. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster.

The REST APIs that the form service exposes deal with forms and are specific to each application.

The following is a high level diagram of the form service:

![Form service diagram](../images/arch-form.png)

## Process storage service
The process storage service stores process instances and tasks as nodes within an [Alfresco Content Services (ACS)](http://docs.alfresco.com/6.1/concepts/welcome.html) repository. 

The user that creates these nodes in ACS is **service-account-storage-service**.  

The REST APIs that the process storage service exposes deal with folders and files and are specific to each application.  

The following is a high level diagram of the process storage service:

![Process storage service diagram](../images/arch-storage.png)

## DMN service
The DMN service contains the backend functionality required for [decision tables](../modeling/modeling-decisions.md) to function within an application. 

## Rabbit MQ
[Rabbit MQ](https://www.rabbitmq.com/) is the default message broker deployed with Activiti Enterprise that routes the events emitted by the runtime bundle asynchronously to other relevant microservices such as the audit and query services. 

Rabbit MQ is the default implementation, but can be replaced by a different message broker. 

## Notification Service
The notification service reads from the same data store as the [query service](#query-service). It can be used to setup [GraphQL](https://graphql.org/learn/) to query specific events and can also use web sockets. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster.

## Audit service
The audit service stores events routed via Spring Cloud Streams (through the Rabbit MQ binder) into its own data store, without any data manipulation. This allows for audit events at an application level to be queried (in a read-only fashion) without contacting a [runtime bundle](#runtime-bundle) directly. 

By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster.

The REST APIs that the audit service exposes deal with audit events and are specific to each application. 

## Preference service
The preference service is a key value store that retains user-based preferences. By default, the data is stored in a Postgres database that is shared within an application namespace between the audit service, query service, notification service, preference service, form service and runtime bundle using separate schemas. This can be updated to point to a Postgres data store external to an application’s namespace or even external to the cluster.

## Connectors
Connectors are used to execute logic outside of processes and the [runtime bundle](#runtime-bundle). Connectors are attached to a [service task](../modeling/modeling-processes/processes-bpmn/bpmn-service.md) within a process definition. When the process flow reaches the service task, the values are sent from the process instance to a connector using Spring Cloud Streams via [Rabbit MQ](#rabbit-mq) to be used as part of the logic. The results are sent back to the process instance after the connector has finished and the process flow continues.
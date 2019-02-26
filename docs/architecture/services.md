# Services
Alfresco Activiti Enterprise is comprised of the following microservices and components. An [architectural diagram](../architecture/architecture.md) is also available. 

## Audit service
The audit service stores events routed via Spring Cloud Streams (through the Rabbit MQ binder) into its own data store, without any data manipulation. This allows for audit events at an application level to be queried (in a read-only fashion) without contacting a runtime bundle directly. 

By default, the data is stored in a Postgres database.

## Query service
The query service is used for querying process data without accessing a runtime bundle directly. It stores events routed via Spring Cloud Streams (through the Rabbit MQ binder) into its own data store for simple, read-only process and task querying. The query service performs some data transformation, whilst the audit service does not. 

By default, the data is stored in a Postgres database. 

## Notification Service
The notification service reads from the same data store as the query service, however it utilises [GraphQL](https://graphql.org/learn/) so that queries can be made against specific events using the API.  

## Runtime bundle
A runtime bundle represents a stateless instance of the process engine that executes an immutable set of process definitions. It is important to note that you cannot deploy new process definitions to an existing runtime bundle, nor can you update a process definition within it. Instead, you create a new version of your runtime bundle that contains the updates. 

Runtime bundles expose a synchronous REST API and an asynchronous message-based API.

Runtime bundles emit and consume events that occur within processes via Spring Cloud Streams. 

## Rabbit MQ
[Rabbit MQ](https://www.rabbitmq.com/) is the default message broker deployed with Alfresco Activiti Enterprise that routes the events emitted by the runtime bundle asynchronously to other relevant microservices such as the audit and query services. 

Rabbit MQ is the default implementation, but can be replaced by a different message broker.  

## Cloud Connectors
Connectors allow for two-way integration between external systems and a runtime bundle. This is the default way to handle service tasks within a process. Connectors are created, stored and executed separately from process instances.

When a service task is executed as part of a process instance, the runtime bundle will emit an integration event that is picked up by a connector which performs the integration with a response being routed back to the process instance so the flow may continue.

## Modeling service
The modeling service contains the backend functionality required for the [Alfresco Modeling Application](../user-interfaces/ui-modeling.md). 

## Alfresco Identity Service
Alfresco Activiti Enterprise uses the [Alfresco Identity Service](LINK REQUIRED) for authentication and user and role management throughout the product. Authentication can be [configured](LINK REQUIRED) to external identity provider instances such as LDAP and SAML. 
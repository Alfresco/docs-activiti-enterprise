---
Title: Cloud Connectors
---

# Cloud Connectors
Connectors are used to import data into your process from an external source or system, or to update an external source or system using data from your process. They can also be used to execute logic outside of a runtime bundle and pass the results back to the process.  

The process engine in a runtime bundle will emit an `IntegrationRequestEvent` when a service task is reached in the process flow and wait for an `IntegrationResultEvent` in an asynchronous fashion. The destination that the request is sent to is defined by the `implementation` property of a service task. The request and response to the external system is handled completely separate to the runtime bundle, increasing the resilience of the process engine itself. 

Cloud Connector components can be [extended further](https://www.alfresco.com/abn/adf/docs/process-services-cloud/) using the Application Development Framework. 

---
Title: Connectors
---

# Connectors 
Connectors are used to import data into your process from an external source or system, or to update an external source or system using data from your process. They are also used to execute logic outside of a runtime bundle and pass the result(s) back to the process. Connectors can be created or updated through the GUI or using the inbuilt JSON editor.

A connector must have a name and description followed by an unlimited number of actions that each contain input and output parameters of data type; boolean, date, integer or string.

Once a connector has been created, they are attached using the `implementation` value of a service task within a process. The connector input(s) and output(s) are mapped from an action on the service task to process variables defined within the process. This allows the response or variables to be re-used further on in a process.  

Examples of where connectors can be used include:

* automating emails using values from a process to populate the fields 
* interacting with nodes within Alfresco Content Services
* triggering a custom REST API and passing the response back to the process 

Connector components can also be [extended further](https://www.alfresco.com/abn/adf/docs/process-services-cloud/) using the Application Development Framework. 

## Connectors and the process engine
The process engine in a runtime bundle will emit an `IntegrationRequestEvent` when a service task is reached in the process flow and wait for an `IntegrationResultEvent` in an asynchronous fashion using Spring Cloud Streams routed by Rabbit MQ. 

The destination that the request is sent to is defined by the `implementation` property of a service task. The request and response to the external system is handled completely separate to the runtime bundle, increasing the resilience of the process engine itself.

## Using connectors
A set of [OOTB (out of the box) connectors](connectors-ootb.md) have been provided, or you can [create your own connectors](connectors-create.md).

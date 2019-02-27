---
Title: Alfresco Modeling Application
---

# Alfresco Modeling Application

The Alfresco Modeling Application is used to create and update applications, including processes and connectors.

All components of an application can be designed using a Graphical User Interface (GUI) and some also contain a JSON editor as well. Users require the APS_MODELER role in order to access the Modeling Application. 

The URL for the Modeling Application will be in the format: `{my-domain}/activiti-cloud-modeling`

## Projects
A project is the top level component of your business application. This is where your processes and forms are created, linked and stored. Once a project has been deployed (i.e. at runtime), it is referred to as an application. 

## Processes
Processes is the area where you create your business processes using BPMN elements. Components such as forms and connectors that you create in the Modeling Application can be linked to the relevant processes here. 

BPMN elements can be created or updated using the drag-and-drop GUI. It is also possible to upload an existing process definition using the upload function.  

The BPMN elements currently available in Alfresco Activiti Enterprise are: 

* [Start events](../bpmn/start-event.md)
* [End events](../bpmn/end-event.md)
* [User tasks](../bpmn/user-task.md)
* [Service tasks](../bpmn/service-task.md)
* [Exclusive gateways](../bpmn/exclusive-gateway.md)
* [Parallel gateways](../bpmn/parallel-gateway.md)

## Connectors
Connectors are used to import data into your process from an external source or system, or to update an external source or system using data from your process. They are how service tasks are implemented in a process. Connectors can be created or updated through the drag-and-drop GUI or using the JSON editor. 

A connector must have a name and description followed by an unlimited number of actions that each contain input and output parameters of data type; boolean, date, integer or string.
Connector actions are automatically assigned a UID when they have been linked to a service task in a process definition. 

[Further information](../configuration/connectors.md) is available regarding Cloud Connectors.

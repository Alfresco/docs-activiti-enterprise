---
Title: Processes
---

# Processes
Processes are the components that are built to represent a business processes. A process definition is the XML that can be thought of as a template for a process. A process instance is a specific, running instance of a process definition. There will be many process instances for each process definition. 

Each process definition represents a business process and is built using BPMN elements. These BPMN elements incorporate the other components that you can create in the Modeling Application  such as [forms](../modeling-forms/README.md), [connectors](../modeling-connectors/README.md) and [decision tables](../modeling-decisions.md). 

## Using processes
Once a project has been deployed, process instances can be started in several ways such as calling a start form from Process Workspace, manually starting a process instance or through an API.  

## Designing processes
Processes are designed using a drag-and-drop UI or using the in-built XML editor. It is also possible to upload an existing process definition using the upload function.

### BPMN elements
Processes are built using a combination of BPMN elements. Each process must contain a start event and an end event as well as conforming to certain rules particular to each element. As long as [each BPMN element](../modeling-processes/processes-bpmn/README.md) is used correctly there are numerous ways a business process can be modeled.

### Process variables


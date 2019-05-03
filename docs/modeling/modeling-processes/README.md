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

The processes area of the Modeling Application also has a **Process extensions** section in addition to the drag-and-drop UI and XML editor. This section displays the additional mappings outside of a process definition XML file for specific BPMN elements. Examples include process variables and the input and output parameter mapping for service tasks containing a connectors and decision tables. 

**Note**: The process extensions are exported in addition to a process definition XML. They are in the format: `<process-name>-extensions.json`.

### BPMN elements
Processes are built using a combination of BPMN elements. Each process must contain a start event and an end event as well as conforming to certain rules particular to each element. As long as [each BPMN element](../modeling-processes/processes-bpmn/README.md) is used correctly there are numerous ways a business process can be modeled.

### Process variables
Process variables are used to store values for use throughout a process. Default values can be set in a process definition and this value can be used or updated throughout the flow of a process instance. 

A process definition without any BPMN element selected will display an **Edit Process Variables** button in the properties panel. Process variables can be configured using the inbuilt GUI or the JSON editor provided with it.

Process variables are used for the following reasons: 

* To store [form values](../modeling-forms/README.md#form-values). When a form is submitted, all form values are created as process variables using the name of the form fields. If a process variable already exists with the same name, the value will be overwritten. 
* To pass values to forms by setting the `name` value of a process variable to match the `id` value of a form field.
* To pass input parameters to [connectors](../modeling-connectors/README.md) and receive output parameters back from a connector. 


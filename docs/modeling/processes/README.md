---
Title: Processes
---

# Processes
Processes are the components that are built to represent a business processes. A process definition is the XML that can be thought of as a template for a process. A process instance is a specific, running instance of a process definition. There will be many process instances for each process definition. 

Each process definition represents a business process and is built using BPMN elements. These BPMN elements incorporate the other components that you can create in the Modeling Application  such as [forms](../forms/README.md), [connectors](../connectors/README.md) and [decision tables](../decisions.md). 

## Naming  
Process names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. 

The following are examples of valid process names: 

```
process-beta
project10
one-hundred-and-thirty-two
```

## Using processes
Once a project has been deployed, process instances can be started in several ways such as calling a start form from Process Workspace, manually starting a process instance or through an API. 

## Designing processes
Processes are designed using a drag-and-drop UI or using the in-built XML editor. It is also possible to upload an existing process definition using the upload function.

The processes area of the Modeling Application also has a **Process extensions** section in addition to the drag-and-drop UI and XML editor. This section displays the additional mappings outside of a process definition XML file for specific BPMN elements. Examples include process variables and the input and output parameter mapping for service tasks containing a connectors and decision tables. 

**Note**: The process extensions are exported in addition to a process definition XML. They are in the format: `<process-name>-extensions.json`.

### BPMN elements
Processes are built using a combination of BPMN elements. Each process must contain a start event and an end event as well as conforming to certain rules particular to each element. As long as [each BPMN element](../processes/bpmn/README.md) is used correctly there are numerous ways a business process can be modeled.

### Process variables
[Process variables](../processes/variables.md) are used to store values and pass them between BPMN elements throughout a process instance.  
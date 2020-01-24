---
Title: Processes
---

# Processes
Processes are the collection of components that are built to represent business processes using [BPMN 2.0 format](https://www.omg.org/spec/BPMN/2.0/).

## Process definitions
A process definition can be thought of as a template for a modeled process. Process definitions are stored in a `.bpmn20.xml` file and have an associated JSON file containing extensions to the process definition. 

Process definitions are designed using [BPMN elements](../processes/bpmn/README.md) which can reference other modeled components such as [forms](../forms/README.md), [connectors](../connectors/README.md) and [decision tables](../decisions.md). 

The properties for a process definition are:

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- | 
| `ID` | The unique identifier for a process definition. Also referred to as a process definition ID and used to start a process instance by referencing the process definition to use. This is system generated and cannot be altered. | model-1bf32338-2bc2-4af2-9496-e9a031e22142 | Yes |
| `Name` | The name of the process definition. Process definition names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. | request-and-order-process | Yes |

The ID and name of a process definition are set as XML attributes of the `definitions` element, for example:

```xml
<bpmn2:definitions id="model-1bf32338-2bc2-4af2-9496-e9a031e22142" name="request-and-order-process">
```

## Processes
Each process has its own ID and name even if there is only a single process within the definition. Process definitions can contain multiple processes when using the BPMN element [pools](../processes/bpmn/pools.md). 

The properties for a process are: 

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- | 
| `ID` | The unique identifier for a process. This is system generated and cannot be altered. | Process_1w18m9x | Yes |
| `Process definition name` | The name of the process definition that the process is part of | request-and-order-process | Yes |
| `Process name` | The name of the process. Process definition names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically | request-process
| `Documentation` | A free text description of what the process does | A process to request and approve stock orders | No |
| `Executable` | If set as `false` then the process will be deployed at runtime but it will not be possible to instantiate any process instances for it. The default value is `true`. | false | Yes |

The ID, name and executable status of a process are set as XML attributes of the `process` element. Documentation is a sub-element of `process`, for example:

```xml
<bpmn2:process id="Process_1w18m9x" name="request-process" isExecutable="false">
```

## Process instances
Process instances are specific running instances of processes that are read from a process definition. Different processes instantiated from the same process definition will still will still receive unique process instance IDs. 

There can be any number of process instances running using the same process definition in an N:1 relationship.

Process instances can be instantiated in many ways such as through the [Process Workspace UI](../../workspace/processes.md#starting-a-process-instance), using the `POST /rb/v1/process-instances` API or using a [trigger](../triggers.md).

## Modeling processes
Process definitions are modeled with [BPMN elements](../processes/bpmn/README.md) and must contain a start event and an end event whilst conforming to the rules particular to each element.

There are three views of a process definition:

* The **Diagram Editor** GUI to drag-and-drop BPMN elements into a process diagram. BPMN elements can be freely moved and edited and sequence flows created between them.

* The **XML Editor** reflecting any changes made in the **Diagram Editor** and displaying the XML definition for a process. The contents of the **XML Editor** can be downloaded as `.bpmn20.xml` files.

* The **Extensions Editor** is a JSON editor that stores the extensions for process definitions. These are stored as `constants`, `mappings` and `properties` for each process in the definition. The **Extensions Editor** is automatically populated by actions undertaken in the **Diagram Editor** such as creating [process variables](../processes/variables.md). 

The descriptions of **Extension Editor** properties are:

| Property | Description |
| -------- | ----------- |
| `constants` | Constants store values that will not change for the duration of a process definition such as mapping connectors and decision tables to the correct service tasks | 
| `mappings` | Mappings store the mapping of variables between BPMN elements such process variables and user tasks  |
| `properties` | Properties store the process variables for a process definition |

An example **Extensions Editor** and `<process-definition-name>-extensions.json` file is:

```json
{
"process": "Process_1w18m9x": {
    "constants": {
        "Task_10kub5t": {
            "_activiti_dmn_table_": {
                "value": "dec"
            }
        }
    },
    "mappings": {
        "Task_19lk7bt": {
            "inputs": {
                "Text0mvlww": {
                    "type": "variable",
                    "value": "var_1"
                },
                "variables.task_var": {
                    "type": "variable",
                    "value": "var_2"
                }
            }
        }
    },
    "properties": {
        "383ffc09-ec7a-4765-a40c-bb84ad06b479": {
            "id": "383ffc09-ec7a-4765-a40c-bb84ad06b479",
            "name": "var_1",
            "type": "string",
            "required": false
        },
        "72c4a262-5db9-4283-8c76-710450ac5e1d": {
            "id": "72c4a262-5db9-4283-8c76-710450ac5e1d",
            "name": "var_2",
            "type": "string",
            "required": false,
            "value": "hello"
        }
    }
  }
}
```

The actions that can be run against a process definition are: 

| Action | Description | 
| ------ | ----------- | 
| `Download diagram` | Download the process diagram in `svg` format |
| `Download process` | Download the `<process-definition-name>.bpmn20.xml` and `<process-definition-name>-extensions.json` files for the process definition |
| `Validate` | Run validation against the process definition. Any errors will be returned in a pop-up window. | 
| `Save` | Save any changes made to the process definition | 
| `Delete` | Delete the process definition | 

## Messages
[Messages](../processes/bpmn/message.md) can be managed at the process definition level using the **Edit Messages** button. Messages are used to send payloads between throwing and catching message events and can be sent between different processes within the same process definition.  

## Process variables
[Process variables](../processes/variables.md) are used to store values and pass them between BPMN elements throughout a process instance. The scope of process variables is restricted to the process they are created in and not the process definition.  
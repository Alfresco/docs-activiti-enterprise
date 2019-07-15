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
Process variables are used to store values and pass them between the process and its BPMN elements. Default values can be set in a process definition and this value can be used or updated throughout the flow of a process instance.  

A process definition without any BPMN element selected will display an **Edit Process Variables** button in the properties panel. Process variables can be configured using the inbuilt GUI or the JSON editor provided with it.

Process variables are stored in the `properties` of the `<process-name>-extensions.json` file with unique IDs and can also be viewed through the UI in the **Extensions Editor**: 

```json
{
    "id": "process-5d0ecfa7-d262-4480-a00b-74734c857f4d",
    "properties": {
        "17aa41f7-9a0c-49c0-805b-045243f8a7e5": {
            "id": "17aa41f7-9a0c-49c0-805b-045243f8a7e5",
            "name": "firstName",
            "type": "string",
            "required": false,
        },
        "2147a4c2-aaf7-44df-a886-d85a7f186445": {
            "id": "2147a4c2-aaf7-44df-a886-d85a7f186445",
            "name": "age",
            "type": "integer",
            "required": true,
            "value": 25
        },
        "887fc645-66ec-4561-8490-e7cc3c8bf0ae": {
            "id": "887fc645-66ec-4561-8490-e7cc3c8bf0ae",
            "name": "accepted",
            "type": "boolean",
            "required": false
        }
    },
}
```

Process variables can also be passed between certain BPMN elements as inputs and outputs: 

* In [user tasks](../modeling-processes/processes-bpmn/bpmn-user.md) to pass values between process variables and [form variables](../modeling-forms/README.md#form-variables).
* As part of [call activities](../modeling-processes/process-bpmn/bpmn-call.md) to pass values between the originating and called process.
* For [decision tables](../modelding-decisions.md) to pass the input value(s) in and store the output(s) in the process.
* To pass input parameters to [connectors](../modeling-connectors/README.md) and receive an output back to the process. 

To pass process variable between elements requires the `inputs` and `outputs` sections of `mappings` to be defined in the `<process-name>-extensions.json`: 

```json
{
    "mappings": {
        "ServiceTask_1xsj0ao": {
            "inputs": {
                "nodeId": {
                    "type": "variable",
                    "value": "string1"
                }
            },
            "outputs": {
                "num1": {
                    "type": "variable",
                    "value": "alfrescoRequestResponseCode"
                }
            }
        }
    }
}
```


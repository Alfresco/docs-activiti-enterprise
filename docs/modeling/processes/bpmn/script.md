---
Title: Script task
---

# Script tasks
Script tasks are used to implement [scripts](../../scripts.md) into a process definition. 

Script tasks are displayed as a single thin, rounded rectangle with a script icon inside.

## XML
The XML representation of a script task is:

```xml
<bpmn2:serviceTask id="Task_0gpdh83" name="Order script" implementation="script.EXECUTE">
	<bpmn2:documentation>A script to loop and update the list of orders.</bpmn2:documentation>
</bpmn2:serviceTask>
```

## Properties 

### Basic properties
The basic properties for a script task are: 

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- | 
| `id` | The unique identifier for the script task. This is system generated and cannot be altered | Task_0gpdh83 | Yes |
| `name` | The name of the script task. This will display on the script task in the process diagram | Order script | No |
| `documentation` | A free text description of what the script task does | A script to loop and update the list of orders.  | No |

The ID and name of a script task are set as XML attributes of the `serviceTask`. Documentation is a sub-element of `serviceTask`, for example: 

```xml
<bpmn2:serviceTask id="Task_0ykbcv0" name="Order script" implementation="script.EXECUTE">
	<bpmn2:documentation>A script to loop and update the list of orders.</bpmn2:documentation>
</bpmn2:serviceTask>
```

### Multi-instance type
The type of [multi-instance](../bpmn/multi.md) execution for the script task. The default value is `None`. 

`Sequential` multi-instance executions only ever have a single instance of that script task executing at any one time. The next instance will only start after the previous one has been executed. 

`Parallel` multi-instances executions start multiple instances of a script task at once, meaning they are can all be executed at the same time. 

### Script name
The [script](../../scripts.md) to execute. The script must exist within the same project as the process definition to be selected. 

Script tasks are handled as [service tasks](../bpmn/service.md) by Activiti Enterprise and will always have the `implementation` value of `script.EXECUTE`. The mapping between a script and its script task is stored in the `constants` of the **Extensions Editor** or `<process-definition-name>-extensions.json` file using the script `name`, for example:

```json
    "constants": {
        "Task_0ykbcv0": {
            "_activiti_script_": {
                "value": "order-script"
            }
        }
    }
```

### Mapping type
Mapping type sets whether [process variables](../variables.md) are sent to a script task and whether the variables in the script are sent back to a process as process variables once the script has executed.

If variables are sent to and from a script task explicit mapping can be configured between process variables and the variables in the script using the **Input mapping** and **Output mapping** fields. If no explicit mapping is set, implicit mapping will be used by attempting to match `name` fields. Only exact matches will map. 

Mappings are stored in the `<process-definition-name>-extensions.json` file and can be viewed in the **Extensions Editor**. 

Mapping type is only visible if a script has been selected. The default value is `Send all variables`. 
---
Title: Script task
---

# Script tasks
Script tasks are used to implement [scripts](../../scripts.md) into a process definition. 

Script tasks are handled as [service tasks](../bpmn/service.md) by Activiti Enterprise and will always have the `implementation` value of `script.EXECUTE`. 

## Properties
The properties that can be set for a script task are the following: 

### ID
The unique identifier for the script task. This is system generated and cannot be altered.
	
| Property | Example | Required | 
| -------- | ------- | -------- | 
| ID | `Task_1tutq6p` | Yes |

### Name
The name of the script task. This will appear in the XML definition and be visible in the process diagram. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Name | `Update order list` | No |

### Documentation
A free text description of what the script task does.

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Documentation | `A script to update the list of orders`  | No |

### Multi-instance type
The type of [multi-instance](../bpmn/multi.md) execution for the script task. The default value is `None`. 

`Sequential` executions only ever have a single instance of that script task running at any one time. The next instance will only start after the previous one has been executed. 

`Parallel` executions start multiple instances of a script task at once, meaning they are all executed at the same time. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Multi-instance type | `None`  | No |

### Script name
The [script](../../scripts.md) to execute. The script that the script task executes is referenced by its `name` in the **Extensions Editor** and `<process-name>-extensions.json` file. The `name` is the value of the property `_activiti_script_` referenced under the script task `id` in the `constants` section. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Script name | `update-orders`  | No |

### Mapping type
Mapping type sets whether [process variables](../variables.md) are sent to a script task and whether the variables in the script are sent back to a process as process variables once the script has executed.

If variables are sent to and from a script task explicit mapping can be configured between process variables and the variables in the script using the **Input mapping** and **Output mapping** fields. If no explicit mapping is set, implicit mapping will be used by attempting to match `name` fields. Only exact matches will map. 

Mappings are stored in the `<process-name>-extensions.json` file and can be viewed in the **Extensions Editor**. 

Mapping type is only visible if a script has been selected. The default value is `Send all variables`. 

## Process diagram
Script tasks are graphically represented by a single thin, rounded rectangle with a script icon inside. 	

The XML representation of a script task is:

```xml
<bpmn2:serviceTask id="Task_1tutq6p" name="Update order list" implementation="script.EXECUTE">
```

The JSON extensions of a script task is:

```json
    "constants": {
        "Task_1tutq6p": {
            "_activiti_script_": {
                "value": "update-orders"
            }
```
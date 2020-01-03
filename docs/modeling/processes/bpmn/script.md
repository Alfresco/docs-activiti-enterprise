---
Title: Script tasks
---

# Script tasks 
Script tasks are used to implement [scripts](../../scripts.md) into a process definition. 

Script tasks are handled as [service tasks](../bpmn/service.md) by Activiti Enterprise and will always have the `implementation` value of `script.EXECUTE`. The `name` of the script that is associated to the script task is stored in the [`<process-name>-extensions.json` file](../../projects.md#files) as the `value` under `_activiti_script_`.

Script tasks are graphically represented by a single, thin rounded rectangle with a script icon inside.

The XML representation of a script task is: 

```xml
<bpmn2:serviceTask id="ServiceTask_2" implementation="script.EXECUTE" />
```

The extensions JSON of a process containing a script task is:


```json
"constants": {
	"ServiceTask_1l85uyw": {
		"_activiti_script_": {
			"value": "java-script-1"
            }
        }
    },
```
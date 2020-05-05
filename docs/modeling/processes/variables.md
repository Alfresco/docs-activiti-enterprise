---
Title: Process variables
---

# Process variables
Process variables are used to store values and pass them between BPMN elements throughout a process instance. The scope of process variables is restricted to a process and not a process definition which is important to consider when modeling with [pools](../processes/bpmn/pools.md) in a process definition.

## Creating process variables
A process definition without any BPMN element selected will display an **Edit Process Variables** button in the properties panel. If using [pools](../processes/bpmn/pools.md) then the **Edit Process Variables** button will only appear when a single process is selected. Process variables can be configured using the inbuilt GUI or the JSON editor provided with it. 

Each process variable has four properties:  

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `name` | A unique name that can contain alphanumeric characters and underscores but must begin with a letter | `var_3` |
| `type` | A data type selected from a dropdown. See the following table for a list of data types | `String` |
| `required` | Sets whether the process variable must contain a value when a process instance is started | `false` | 
| `value` | An optional default value for the process variable | `ice-cream` |

**Note**: There are five process variable names that are created automatically and should not be used as custom process variable names. `initiator` stores the ID of the process initiator and `nrOfInstances`, `nrOfActiveInstances`, `nrOfCompletedInstances` and `loopCounter` are used by [multi-instance elements](../bpmn/multi.md).

The following are the data types that process variables can be set as:

| Type | Description | Example | 
| ---- | ----------- | ------- |
| String | A sequence of characters | `#Mint-Ice-Cream-4!`
| Integer | A positive whole number | `642` |
| Boolean | A value of either `true` or `false` | `true` |
| Date | A specific date in the format `YYYY-MM-DD` | `2020-04-22` | 
| Datetime | A specific date and time in the format `YYYY-MM-DD HH:mm:ss` | `2020-09-10 22:30:00`
| File | A [file](../modeling/files.md) uploaded into a process definition or as part of a process instance or task | 
| JSON | A JSON object | `{"flavor" : "caramel"}` | 
| Folder | A folder object described as JSON | `"name": "mint-folder"` |
| Array | A comma separated list of entries | `mint, strawberry, vanilla` will format to `["mint","strawberry","vanilla"]` |

Process variables are stored in the `properties` of the [`<process--definition-name>-extensions.json`](../modeling/projects.md#files) file with unique IDs and can also be viewed through the UI in the **Extensions Editor**: 

The following is an example of an `<process-definition-name>-extensions.json` file or extensions editor:

```json
{
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

Process variables can also be declared when starting a process instance. 

The following is an example payload for `POST rb/v1/process-instances` in the runtime bundle:

```json
{
  "payloadType": "StartProcessPayload",
  "processDefinitionId": "Process_MAigMN6p:1:bbfdea14-4907-11ea-9908-deb52b497d16",
  "variables": {
    "area_code": 472,
    "district" : "North"
   }
}
```

## Mapping variables
Process variables in a process can be mapped to and from parameters in BPMN elements such as [task variables](../forms/README.md#form-variables), [script variables](../scripts.md#script-variables) and [decision table values](../decisions.md). Input mapping is used to set the process variable sent from the process to the BPMN element and output mapping is used to set the target process variable to receive the results from the BPMN element after it has been executed.

The mapping of variables is stored in the `<process-definition-name>-extensions.json` file and can also be viewed in the **Extensions Editor** in the `mappings` section.

Mapping requires two sets of configuration:

* [Variable mapping](#variable-mapping)
* [Mapping type](#mapping-type)

### Variable mapping
The variable mapping is used to configure the type of mapping used between a process and its BPMN elements. There are three types of mapping that can be used: 

* **Process Variables** are just regular process variables that must match the type of the source or target parameter. For example an input parameter of type `string` cannot map to a process variable of type `file`.

* **Expressions**: Expressions can be entered using a JSON editor to create more complex mappings such as mapping JSON process variables to input and output parameters. For example, using `${temperature.celsius}` will use the value for the object `celsius`. 

	In the following example this would result in the value *16*:

	```json
	{
  	"day": "Monday",
  	"temperature": {
    	"celsius": 16,
    	"fahrenheit": 66
  		}
	}
	```

* Static **Values** can be entered rather than using process variables. 

Static values and expressions are stored as values in the `mappings` section of the `<process-definition-name>-extensions.json` and process variables are stored as variables. For example: 

```json
"Task_1f1wpht": {
	"inputs": {
		"flavor": {
			"type": "variable",
			"value": "choice"
			},
		"price": {
			"type": "value",
			"value": "${lookUp.price}"
			},
		"Limit": {
			"type": "value",
			"value": 200
			}
		}
	}
},
```

### Mapping type
There are three options for passing and updating values to and from process variables throughout a process instance: 

* [Send all variables](#send-all-variables)
* [Map variables](#map-variables)
* [Send no variables](#send-no-variables)

**Note**: The default behaviour is to send all variables.

#### Send all variables
Sending all variables passes all variables to and from the BPMN element without explicitly mapping between the process variables and element variables. To send all variables, select the option **Send all variables** and do not set and input and output mappings. 

When sending all variables mapping will be attempted using the `name` of process variables and the variable names in the BPMN element. If the names are identical then the values will be updated between the variables. 

For example, sending the process variables `flavor` and `cost` to a user task will create task variables called `flavor` and `cost`. If a form field called `flavor` exists in the user task then it will be updated with the value of `flavor` from the process variable. 

If all variables are sent for a BPMN element then the `id` of that element will not appear in the `mappings` section of the `<process-definition-name>-extensions.json` or **Extensions Editor**. 

#### Map variables
Mapping variables is to explicitly map individual process variables to variables in the BPMN element. To map variables, select the option **Send all variables** and set the input and output mappings. 

If mapping variables the `id` of the BPMN element will appear in the `mappings` section of the `<process-definition-name>-extensions.json` or **Extensions Editor** with the `inputs` and `outputs` populated with the `name` or `id` of the BPMN element variable.

The following is an example of mapping variables for a service task: 

```json
{
    "mappings": {
        "ServiceTask_0dfu5w2": {
            "inputs": {
                "nodeId": {
                    "type": "value",
                    "value": "${content.nodeId}"
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

#### Send no variables
Sending no variables will not pass any variables between a process instance and its BPMN elements.

If no variables are sent for a BPMN element then the `id` of that element will be present in the `mappings` section of the `<process-definition-name>-extensions.json` or **Extensions Editor** with empty `inputs` and `outputs`.

The following is an example of sending no variables for a user task:

```json
{
    "mappings": {
        "UserTask_0xyu1k8": {
            "inputs": {},
            "outputs": {}
        }
    }
}
```
# Process variables
Process variables are used to store values and pass them between BPMN elements throughout a process instance.  

## Creating process variables
A process definition without any BPMN element selected will display an **Edit Process Variables** button in the properties panel. Process variables can be configured using the inbuilt GUI or the JSON editor provided with it. 

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

Process variables are stored in the `properties` of the [`<process-name>-extensions.json`](../modeling/projects.md#files) file with unique IDs and can also be viewed through the UI in the **Extensions Editor**: 

The following is an example of an `<process-name>-extensions.json` file or extensions editor:

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

The following is an example payload for `POST /v1/process-instances` in the runtime bundle:

```json
{
  "payloadType": "StartProcessPayload",
  "processDefinitionId": "3bce2a6b-20f3-11ea-9182-eafde8758223",
  "variables": {
  {	
  	"name": "area_code",
    "type": "integer",
    "value": 472,
    "required": true
   },
  {	
  	"name": "district",
    "type": "string",
    "value": "North",
    "required": false
   }
}
```

## Mapping process variables
Process variables can be mapped to and from variables in a BPMN element so that their values can be reused and updated throughout a process instance. 

The following are examples of passing process variables in a process:

* In [user tasks](../processes/bpmn/user.md) to pass values between process variables and [form fields](../forms/fields.md) or [form variables](../forms/README.md#form-variables).
* As part of [call activities](../processes/bpmn/call.md) to pass process variables between the originating and called process.
* To pass input parameters to [connectors](../connectors/README.md) and receive the output parameters back to a process.
* For [decision tables](../decisions.md) to pass the input values into the table and to store the output values in a process.
* To use a [file](../files.md) within a process. 

Process variables are mapped in a process definition to the BPMN element that is being passed them using the **Mapping type** field in the UI. Inputs and outputs are then set for each variable. The `inputs` and `outputs`  are stored in the `mappings` section of the `<process-name>-extensions.json` and can also be viewed in the **Extensions Editor** of a process definition. 

**Note**: Static values can be entered instead of process variables when mapping to BPMN element variables. 

There are three options for passing and updating values to and from process variables throughout a process instance: 

* [Send all variables](#send-all-variables)
* [Map variables](#map-variables)
* [Send no variables](#send-no-variables)

**Note**: The default behaviour is to send all variables.

### Send all variables
Sending all variables passes all variables to and from the BPMN element with explicitly mapping between the process variables and element variables. 

When sending all variables mapping will be attempted using the `name` of process variables and the variable names in the BPMN element. If the names are identical then the values will be updated between the variables. 

If all variables are sent for a BPMN element then the `id` of that element will not appear in the `mappings` section of the `<process-name>-extensions.json` or **Extensions Editor**. 

### Map variables
Mapping variables is to explicitly map individual process variables to variables in the BPMN element. 

If mapping variables the `id` of the BPMN element will appear in the `mappings` section of the `<process-name>-extensions.json` or **Extensions Editor** with the `inputs` and `outputs` populated with the `name` or `id` of the BPMN element variable.

The following is an example of mapping variables for a service task: 

```json
{
    "mappings": {
        "ServiceTask_0dfu5w2": {
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

### Send no variables
Sending no variables will not pass any variables between a process instance and its BPMN elements.

If no variables are sent for a BPMN element then the `id` of that element will be present in the `mappings` section of the `<process-name>-extensions.json` or **Extensions Editor** with blank `inputs` and `outputs`.

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
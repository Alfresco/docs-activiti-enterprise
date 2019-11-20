---
Title: Aspose connector
---

# Aspose connector
The Aspose connector is used to generate `docx` and `pdf` files using an Aspose template and metadata map. The generated document is saved to the Alfresco Content Services repository folder that corresponds to the process instance ID that called the connector in the following format:

```bash
<application-name> Site / Document Library / <process-instance-id> / <service-task-id> / <generated-document>
``` 

**Important**: The Aspose connector requires a licensed version of Alfresco Content Services (ACS) deployed to interact with.

The Aspose connector is graphically represented by the Aspose logo next to a document under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the Aspose connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1jas8cr" implementation="asposeConnector.GENERATE" />
```

## Template configuration
The Aspose connector uses a [process variable](../../processes/README.md#process-variables) of type JSON to insert values into a pre-configured template. The [input parameter](#input-parameters) that contains the information is called `metadata`.

An example of the `metadata` [input parameter](#input-parameters) is the following: 

```json
{
"iceCream": {
	"flavor":"Mint"
	},
"timeOfYear": {
	"season":"Summer"
	}
}
``` 

The Aspose template itself uses the following format to import values from the `metadata` JSON into a document:

```
Current season: {{timeOfYear.season}}
Flavor of the month: {{iceCream.flavor}}
```

## Input parameters 
The following are the parameters that can be passed to the Aspose connector as input parameters using the `GENERATE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `nodeId` | The node ID of the template to use from Alfresco Content Services | String | `*` |
| `uri` | The URI of the template to use | String | `*` |
| `files` | A [file](../../modeling/files.md) uploaded in a process and set as a process variable or uploaded as part of a form or another connector to use as the template | File | `*` |
| `metadata` | The metadata to use in the template. If no value for `metadata` is passed then the inbound variable map will be used in the template | JSON | No |
| `outputFormat` | Format to save the output document in. Values are `DOCX` or `PDF` | String | Yes |
| `outputFileName` | The name of the generated file | String | No |
| `parentFolder` | The node ID of the folder to store the generated document in. If this value is set, the generated document will be output here and not to the default process instance folder for the process instance | String | No |

`*` One of these parameters is required.   

## Output parameter
The following is the parameter that is returned to the process by the Aspose connector as an output parameter using the `GENERATE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `docgen.result` | The generated document | File | 
| `docgen.error` | A list of errors if any are caught by the connector | String |

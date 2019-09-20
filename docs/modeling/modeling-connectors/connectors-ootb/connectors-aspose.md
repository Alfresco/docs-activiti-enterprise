---
Title: Aspose connector
---

# Aspose connector
The Aspose connector is used to generate `docx` and `pdf` files using an Aspose template and metadata map. The Apose connector is graphically represented by 

**Important**: The Aspose connector requires a licensed version of Alfresco Content Services (ACS) deployed to interact with.

The `implementation` value of the Aspose connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1jas8cr" implementation="AsposeConnector.GENERATE" />
```

## Input parameters 
The following are the parameters that can be passed to the Aspose connector as input parameters using the `GENERATE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `nodes` | | | Yes |
| `metadata` | The metadata to use in the template | | No |
| `outputFormat` | Format to save the output document in. Values are `DOCX` or `PDF` | String | Yes |

## Output parameter
The following is the parameter that is returned to the process by the Aspose connector as an output parameter using the `GENERATE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `docgen.result` | | String | 
| `docgen.error` | A list of errors if any are caught by the connector | String |

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Aspose connector: 

| Variable | Description |
| -------- | ----------- |
|  | The filename of a valid Aspose license  |
| `ABS_PATH` | |
---
Title: Aspose connector
---

# Aspose connector
The Aspose connector is used to generate `docx` and `pdf` files using an Aspose template and metadata map. The document is saved to the Alfresco Content Services repository folder that corresponds to the process instance ID that called the connector. The Aspose connector is graphically represented by 

**Important**: The Aspose connector requires a licensed version of Alfresco Content Services (ACS) deployed to interact with.

The `implementation` value of the Aspose connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1jas8cr" implementation="asposeConnector.GENERATE" />
```

## Input parameters 
The following are the parameters that can be passed to the Aspose connector as input parameters using the `GENERATE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `nodeId` | The node ID of the template to use from Alfresco Content Services | String | `*` |
| `nodes` | The node array describing the template to use from Alfresco Content Services | Array | `*` |
| `file` | A JSON object describing a template file | JSON Object | `*` |
| `metadata` | The metadata to use in the template | | No |
| `outputFormat` | Format to save the output document in. Values are `DOCX` or `PDF` | String | Yes |
| `outputFileName` | The name of the generated file | String | No |

`*` One of these parameters is required. If more than one is used, the order of preference is `nodeId`, `nodes` and then `file`.  

## Output parameter
The following is the parameter that is returned to the process by the Aspose connector as an output parameter using the `GENERATE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `docgen.result` | The node ID of the generated document | String | 
| `docgen.error` | A list of errors if any are caught by the connector | String |
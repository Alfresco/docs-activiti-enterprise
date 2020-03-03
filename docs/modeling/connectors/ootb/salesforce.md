---
Title: Salesforce connector
---

# Salesforce connector
The Salesforce connector is used to integrate with an installation of [Salesforce](https://salesforce.com). This allows processes to operate on Salesforce objects in a number of ways. The Salesforce connector is graphically represented by the Salesforce logo under the OOTB Connectors menu whilst modeling a process. 

The following actions can be executed using the Salesforce connector: 

* [Create an object instance](#create-an-object-instance)
* [Get an object instance](#get-an-object-instance)
* [Update an object instance](#update-an-object-instance)
* [Delete an object instance](#delete-an-object-instance)
* [Query an object instance](#query-an-object-instance)
* [Submit a Salesforce object instance for approval](#submit-an-object-instance-for-approval)
* [Query the current approval processes in Salesforce](#query-approval-processes)
* [Approve a Salesforce object instance submitted for approval](#approve-an-object-instance-submitted-for-approval) 
* [Reject a Salesforce object instance submitted for approval](#reject-an-object-instance-submitted-for-approval)
* [Create a new custom object in Salesforce](#create-a-custom-object-definition) 

## Configuration parameters
The Salesforce connector requires a [developer account](https://developer.salesforce.com/signup) to interact with Activiti Enterprise. The following credentials need to be obtained and set as configuration parameters in the modeling application or during application deployment:

| Parameter | Description | Default | 
| --------- | ----------- | ------- |
| `SALESFORCE_CLIENT_ID` | The ID of your Salesforce account | |
| `SALESFORCE_CLIENT_SECRET` | The secret associated to your Salesforce account | |
| `SALESFORCE_USERNAME` | The user that the connector will use to interact with Salesforce | |
| `SALESFORCE_PASSWORD` | The password for the user will use to interact with Salesforce | |
| `SALESFORCE_SECURITY_TOKEN` | The security token for the user that will interact with Salesforce | |
| `SALESFORCE_URL_LOGIN` | The URL to login to Salesforce | `https://login.salesforce.com/services/oauth2/token` |
| `SALESFORCE_SOAP_URL_LOGIN` | The URL for SOAP requests | `https://login.salesforce.com/services/Soap/c/45.0` |
| `SALESFORCE_VERSION` | The version of Salesforce | `45.0` |

## Output parameters
The following are the output parameters that all actions in the Salesforce connector use: 

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `salesforceResult` | The result from the REST or SOAP call from Salesforce | String |
| `salesforceStatus` | The HTTP status code of the response | Integer | 
| `salesforceError` | A list of errors if any are caught by the connector | String |

**Note**: The execution of the Salesforce connector is always successful. Any errors will be returned in the `salesforceError` parameter. The parameter will be null if there were no errors.

## Create an object instance
The `CREATE` action is used to create an object instance in Salesforce.

The `implementation` value of the create action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.CREATE" />
```

The following are the parameters that can be passed to the Salesforce connector as input parameters using the `CREATE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `SObjectName` | The name of a valid Salesforce object to operate on, for example *Account* | String | Yes |
| `salesforcePayload`| The payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/api-explorer/sobject/Account) for valid fields | JSON object | Yes |

## Get an object instance
The `GET` action is used to retrieve an existing object instance from Salesforce.

The `implementation` value of the get action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.GET" />
```

The following are the parameters that can be passed to the Salesforce connector as input parameters using the `GET` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `SObjectName` | The name of a valid Salesforce object to operate on, for example *Account* | String | Yes |
| `SObjectId`| The ID of the object to retrieve | String | Yes |

## Update an object instance
The `UPDATE` action is used to update an existing object instance in Salesforce.

The `implementation` value of the update action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.UPDATE" />
```

The following are the parameters that can be passed to the Salesforce connector as input parameters using the `UPDATE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `SObjectName` | The name of a valid Salesforce object to operate on, for example *Account* | String | Yes |
| `SObjectId`| The ID of the object to update | String | Yes |
| `salesforcePayload`| The payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/api-explorer/sobject/Account) for valid fields | JSON object | Yes |

## Delete an object instance
The `DELETE` action is used to delete an existing object instance in Salesforce.

The `implementation` value of the delete action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.DELETE" />
```

The following are the parameters that can be passed to the Salesforce connector as input parameters using the `DELETE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `SObjectName` | The name of a valid Salesforce object to operate on, for example *Account* | String | Yes |
| `SObjectId`| The ID of the object to delete | String | Yes |

## Query an object instance
The `QUERY` action is used to query existing object instances in Salesforce.

The `implementation` value of the query action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.QUERY" />
```

The following is the parameter that can be passed to the Salesforce connector as an input parameter using the `QUERY` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `salesforceQuery`| A valid query against Salesforce | String | Yes |

## Submit an object instance for approval
The `approval_submit` action is used to submit an existing object instance for approval in Salesforce.

The `implementation` value of the submission action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.approval_submit" />
```

The following is the parameter that can be passed to the Salesforce connector as an input parameter using the `approval_submit` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `salesforcePayload`| The submission payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_process_approvals_submit.htm) for valid fields | JSON object | Yes |

## Query approval processes
The `approval_list` action is used to query the current approval processes in Salesforce.

The `implementation` value of the query action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.approval_list" />
```

No input parameters need to be passed with the `approval_list` action. 

## Approve an object instance submitted for approval
The `approval_approve` action is used to approve an existing object instance that has been submitted for approval in Salesforce.

The `implementation` value of the approval action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.approval_approve" />
```

The following is the parameter that can be passed to the Salesforce connector as an input parameter using the `approval_approve` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `salesforcePayload`| The approval payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_process_approvals_approve.htm) for valid fields | JSON object | Yes |

## Reject an object instance submitted for approval
The `approval_reject` action is used to reject an existing object instance that has been submitted for approval in Salesforce.

The `implementation` value of the rejection action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.approval_reject" />
```

The following is the parameter that can be passed to the Salesforce connector as an input parameter using the `approval_reject` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `salesforcePayload`| The rejection payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_process_approvals_reject.htm) for valid fields | JSON object | Yes |

## Create a custom object definition
The `custom_object_create` action is used to create a custom object definition using the Salesforce SOAP metadata API.

The `implementation` value of the custom object action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5lhj3ty" implementation="salesforceConnector.custom_object_create" />
```

The following is the parameter that can be passed to the Salesforce connector as an input parameter using the `custom_object_create` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `salesforcePayload`| The custom metadata creation payload to send to Salesforce as a JSON map. See [Salesforce API documentation](https://developer.salesforce.com/docs/atlas.en-us.api_meta.meta/api_meta/meta_createMetadata.htm) for valid fields | JSON object | Yes |
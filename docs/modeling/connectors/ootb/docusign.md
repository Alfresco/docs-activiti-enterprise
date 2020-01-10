---
Title: Docusign connector
---

# Docusign connector
The Docusign connector is used to send documents via email to be digitally signed. The process flow waits for a document to be signed before continuing with the process. The signed document
is saved to the Alfresco Content Services repository folder that corresponds to the process instance ID that called the connector in the following format:

```bash
<application-name> Site / Document Library / <process-instance-id> / <service-task-id> / <signed-document>
``` 

**Important**: The Docusign connector requires a licensed version of Alfresco Content Services (ACS) deployed to interact with. It also requires a [Docusign](https://www.docusign.com/) account to handle document signing. 

The Docusign connector is graphically represented by the Docusign logo under the OOTB Connectors menu whilst modeling a process.

The `implementation` value of the Docusign connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1jas8cr" implementation="docusignConnector.SIGNDOCUMENT" />
```

## Docusign configuration
The Docusign connector uses the Docusign REST API. An application needs to be set up and authorized to utilize this functionality in the connector. The following steps outline this process:

1. Sign into your Docusign account. 
2. [Configure an application for JWT authentication](https://developers.docusign.com/esign-rest-api/guides/authentication/oauth2-jsonwebtoken) including the prerequisites required to setup an RSA key. 
3. [Grant consent to the application](https://developers.docusign.com/esign-rest-api/guides/authentication/obtaining-consent). 

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Docusign connector: 

| Variable | Description | 
| -------- | ----------- | 
| `DOCUSIGN_APILOCATION` | The URL of the Docusign REST API to use | 
| `DOCUSIGN_ACCOUNT_ID` | The Docusign account that the application is registered to | 
| `DOCUSIGN_CLIENT_ID` | The application integration key | 
| `DOCUSIGN_IMPERSONATED_USER_ID` | The GUID of the Docusign user that the application impersonates  | 
| `DOCUSIGN_AUTH_SERVER` | The URL of the Docusign authorization server | 
| `DOCUSIGN_JWT_LIFETIME` | The expiration time of the JWT expressed in seconds | 
| `DOCUSIGN_RSA_KEY` | The private RSA key for the Docusign application | 
| `ALFRESCO_CONTENT_REPO_BASE_URL` | The base URL of the Content Services deployment |

## Input parameters 
The following are the parameters that can be passed to the Docusign connector as input parameters using the `SIGNDOCUMENT` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `nodeId` | The node ID of the file to sign from Alfresco Content Services | String | `*` |
| `uri` | The URI of the file to sign | String | `*` |
| `files` | A [file](../../files.md) uploaded in a process and set as a process variable or uploaded as part of a form or another connector to sign | File | `*` |
| `recipientEmail` | The email address to send the file to for signing | String | Yes |
| `recipientName` | The name of the email recipient | String | No |
| `emailSubject` | The subject line of the email | String | No | 
| `documentId` | A document ID for the Docusign API to use. The value must be positive integer | Integer | No | 
| `nodeFormat` | The document format for the Docusign API. The default value is `pdf` | String | No |
| `signHerePage` | The label for the `Sign Here` box in the document | String | No | 
| `signHereX` | The X position of the `Sign Here` box in the document | String | No |
| `signHereY` | The Y position of the `Sign Here` box in the document | String | No | 
| `timeout` | The time to wait for the document to be signed expressed in seconds | Integer | No | 
| `parentFolder` | The node ID of the folder to store the signed document in. If this value is set, the generated document will be output here and not to the default process instance folder for the process instance | String | No |
| `outputFileName` | The name of the signed document | String | No |
| `nodeType` | The node type assigned to the signed document in ACS. The default is `cm:content` | String | No |

`*` One of these parameters is required.   

## Output parameter
The following is the parameter that is returned to the process by the Docusign connector as an output parameter using the `SIGNDOCUMENT` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `docusign.result` | The node ID of the signed document | String | 
| `docusign.error` | A list of errors if any are caught by the connector | String |
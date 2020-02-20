---
Title: DBP connector
---

# DBP connector
The Digital Business Platform (DBP) Connector links an instance of Alfresco Activiti Enterprise and [Alfresco Content Services (ACS)](http://docs.alfresco.com/6.1/concepts/welcome.html). This enables process instances in Activiti Enterprise to create, update and delete content in ACS. The DBP connector is graphically represented by the Alfresco logo under the OOTB connectors menu whilst modeling a process. 

**Important**: The DBP connector requires a licensed version of Alfresco Content Services (ACS) deployed to interact with.

The following actions can be executed using the DBP connector: 

* [Update the properties of a node in ACS](#update-node-properties)
* [Retrieve the properties of a node from ACS](#retrieve-node-properties)
* [Move a node to a different location within ACS](#move-a-node)
* [Delete a node from ACS](#delete-a-node)
* [Create a folder in ACS](#create-a-folder)
* [Delete a folder from ACS](#delete-a-folder)

As an example, the `implementation` value of the update node properties action in a service task would be similar to the following:

```xml
 <bpmn2:serviceTask id="ServiceTask_00b2568" implementation="dbp-connector.updateMetadataTask" />
```

## Update node properties
The `updateMetadataTask` action is used to update the metadata of a node in ACS. Metadata includes properties such as the title of a document.

### Input parameters
The following are the parameters that can be passed to the DBP connector as input parameters using the `updateMetadataTask` action: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeId` |  The ID of the node to update | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |
| `properties` | The values of the properties to update | JSON object | `{"cm:title" : "New titled document"}` |

### Output parameter
The following is the parameter that is returned to the process by the DBP connector as an output parameter using the `updateMetadataTask` action: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |

## Retrieve node properties
The `retrieveMetadataTask` action is used to retrieve the metadata of a node in ACS. Metadata includes properties such as title, lock status and node type. 

### Input parameter
The following are is the parameter that can be passed to the DBP connector as an input parameter using the `retrieveMetadataTask` action: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeId` | The ID of the node to retrieve properties from | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |

### Output parameters
The following are the parameters that are returned to the process by the DBP connector as output parameters using the `retrieveMetadataTask` action: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |
| `alfrescoRetrievedMetadataResponse` | The properties returned by the request | JSON object | *See the following for an example* |

The following is an example of `alfrescoRetrievedMetadataResponse`:

```json
"alfrescoRetrievedMetadataResponse" : "class Node {
                                                id: d606c499-0c8a-4ae9-b297-de50e2eed350
                                                name: test_28e5620b-e1f1-4d7d-b4ba-4ca19f86dd36
                                                nodeType: cm:content
                                                isFolder: false
                                                isFile: true
                                                isLocked: false
                                                modifiedAt: 2019-03-05T11:42:06.891Z
                                                modifiedByUser: class UserInfo {
                                                    displayName: Administrator
                                                    id: admin
                                                }
                                                createdAt: 2019-03-05T11:42:06.891Z
                                                createdByUser: class UserInfo {
                                                    displayName: Administrator
                                                    id: admin
                                                }
                                                parentId: 366379d7-7334-4fdf-a61c-414fa9114c7e
                                                isLink: null
                                                isFavorite: null
                                                content: class ContentInfo {
                                                    mimeType: application/octet-stream
                                                    mimeTypeName: Binary File (Octet Stream)
                                                    sizeInBytes: 0
                                                    encoding: UTF-8
                                                }
                                                aspectNames: [cm:auditable]
                                                properties: null
                                                allowableOperations: null
                                                path: null
                                                permissions: null
                                            }"
```

## Move a node
The `moveNode` action is used to move a node to a different location in ACS.

### Input parameters
The following are the parameters that can be passed to the DBP connector as input parameters using the `moveNode` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeId` | The ID of the node to move | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |
| `targetParentId` | The ID of the new parent node | String | `d606c499-0c8a-4ae9-b297-de50e2eed350` |

### Output parameter
The following is the parameter that is returned to the process by the DBP connector as an output parameter using the `moveNode` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |

## Delete a node
The `deleteNode` action is used to delete a node from ACS. The node type must be `cm:content`.

**Note**: The node is moved to the trashcan.

### Input parameter
The following is the parameter that can be passed to the DBP connector as an input parameter using the `deleteNode` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeId` | The ID of the node to delete | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |

### Output parameter
The following is the parameter that is returned to the process by the DBP connector as an output parameter using the `deleteNode` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |

## Create a folder
The `createFolder` action is used to create a folder in ACS.

### Input parameters
The following are the parameters that can be passed to the DBP connector as input parameters using the `createFolder` action:

| Parameter | Description | Type | Notes | Example |
| --------- | ----------- | ---- | ----- | ------- |
| `folderName` |  The name of the folder to be created | String | | `myNewFolder` |
| `parentId` |  The node ID of the parent folder | String | Can be specified as a node ID (including already known aliases such as **-my-**) or a site ID (which must contain a document library that will become the parent of the new folder) | `d606c499-0c8a-4ae9-b297-de50e2eed350` |
| `relativePath` |  The path of the new folder relative to its parent | String | Parameter is optional. If it is not specified then the new folder will be created as a direct child of the parent folder | `/newStructure/levelOne/levelTwo` |

### Output parameters
The following are the parameters that are returned to the process by the DBP connector as output parameters using the `createFolder` action: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |
| `alfrescoCreatedNodeId` | The node ID of the newly created folder | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |
| `alfrescoCreatedNodeResponse` | The properties of the newly created folder | JSON object | *See the following for an example* |

The following is an example of `alfrescoCreatedNodeResponse`:

```json
 "alfrescoCreatedNodeResponse": "class Node {
                                              id: a0d48ea5-6fe7-4c07-b4d2-41a8e73fa93e
                                              name: testFolder_abd2407c-d059-4451-8482-eb8561cc893e
                                              nodeType: cm:folder
                                              isFolder: true
                                              isFile: false
                                              isLocked: false
                                              modifiedAt: 2019-03-08T10:17:09.146Z
                                              modifiedByUser: class UserInfo {
                                                  displayName: Administrator
                                                  id: admin
                                              }
                                              createdAt: 2019-03-08T10:17:09.146Z
                                              createdByUser: class UserInfo {
                                                  displayName: Administrator
                                                  id: admin
                                              }
                                              parentId: 499e9761-ca7a-4228-825d-35899b5b9984
                                              isLink: null
                                              isFavorite: null
                                              content: null
                                              aspectNames: [cm:auditable]
                                              properties: null
                                              allowableOperations: null
                                              path: null
                                              permissions: null"
                                          }
```



## Delete a folder
The `deleteFolder` action is used to delete a folder in ACS. The node type must be `cm:folder`.

**Note**: The node will be moved to the trashcan.

### Input parameter
The following is the parameter that can be passed to the DBP connector as an input parameter using the `deleteFolder` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeId` | The ID of the node to delete | String | `fdcac4a4-1f51-4e02-820d-c2b669647671` |

### Output parameter
The following is the parameter that is returned to the process by the DBP connector as an output parameter using the `deleteFolder` action:

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `alfrescoRequestResponseCode` |  The HTTP response status code for the request | Integer | `200` |

## Configuration parameters
Values for configuration parameters that are specific to a connector instance can be set in the modeling application or during application deployment.

The following are the configuration parameters that need to be set for the DBP connector: 

**Note**: The value `$DNSNAME` needs to be updated for your deployment. All other values are static.  

**Note**: A service user that is available to the Identity Service and ACS repository should be created and used in place of the default admin for the `ALFRESCO_IDENTITY_SERVICE_USERNAME`. It should be given the appropriate permissions for the actions it will need to perform against ACS following the principal of least privilege.

| Variable | Description | Value | 
| -------- | ----------- | ------- | 
| `ACT_RABBITMQ_HOST` | The host of Rabbit MQ | `rabbitmq` |
| `ACT_RABBITMQ_PORT` | The port number Rabbit MQ is running on | `5672` |
| `MESSAGING_ACTIVEMQ_HOST` | The host of Active MQ | `aps2-infra-activemq-broker.default` |
| `MESSAGING_ACTIVEMQ_PORT` | The port number of Active MQ | `5672` |
| `MESSAGING_ACTIVEMQ_USERNAME` | The username Active MQ will use | `admin` |
| `MESSAGING_ACTIVEMQ_PASSWORD` | The password for the Active MQ user | `admin` |
| `ALFRESCO_CONTENT_REPO_BASE_URL` | The base URL of the ACS repository to link with | `https://gateway.$DNSNAME/alfresco` |
| `ALFRESCO_IDENTITY_SERVICE_TOKENURL` | The URL for obtaining identity tokens | `https://identity.$DNSNAME/auth/realms/alfresco/protocol/openid-connect/token` |
| `ACTIVITI_SERVICE_RUNTIME_URL` |The base URL for runtime bundles | `https://gateway.$DNSNAME` |
| `ACTIVITI_SERVICE_QUERY_URL` | The base URL for the query service | `https://gateway.$DNSNAME` |
| `ACTIVITI_SERVICE_AUDIT_URL` | The base URL for the audit service | `https://gateway.$DNSNAME` |
| `ACTIVITI_SERVICE_MODELING_URL` | The base URL for the modeling service | `https://gateway.$DNSNAME` |
| `ACTIVITI_SERVICE_FORM_URL` | The base URL for the form service | `https://gateway.$DNSNAME` |
| `ALFRESCO_EVENT_GATEWAY_BASE_URL` | The base URL for the gateway | `https://gateway.$DNSNAME` |
| `ALFRESCO_CONTENT_REPO_DISCOVERY_PATH` | The base URL for ACS APIs | `/api` |
| `ALFRESCO_CONTENT_REPO_CORE_PATH` | The base URL for the ACS core API | `/api/-default-/public/alfresco/versions/1` |
| `ALFRESCO_CONTENT_REPO_AUTHENTICATION_PATH` | The base URL for the ACS authentication API | `/api/-default-/public/authentication/versions/1` |
| `ALFRESCO_CONTENT_REPO_WORKFLOW_PATH` | The base URL for the ACS workflow API | `/api/-default-/public/workflow/versions/1` |
| `ALFRESCO_CONTENT_REPO_SEARCH_PATH` | The base URL for the ACS content search API | `/api/-default-/public/search/versions/1` |

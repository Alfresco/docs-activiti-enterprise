---
Title: Content connector
---

# Content connector
The content connector can be used to execute actions against content in [Alfresco Content Services (ACS)](http://docs.alfresco.com/latest/concepts/welcome.html) as part of a process. The content connector is graphically represented by the Alfresco logo whilst modeling a process.

**Important**: The content connector requires a licensed version of Alfresco Content Services (ACS) to interact with.

The content connector does not require any connector variables to be set. It connects to the deployed ACS instance automatically. 


The following actions can be executed using the content connector: 

* CREATE_NODE
* UPDATE_CONTENT
* MOVE_CONTENT
* COPY_CONTENT
* DELETE_CONTENT
* LOCK_NODE
* UNLOCK_NODE 
* SET_PERMISSIONS
* CREATE_FOLDER
* SELECT_FOLDER
* SELECT_FILE
* SELECT_METADATA
* UPDATE_METADATA
* UPDATE_TAG

As an example, the `implementation` value of the copy content action in a service task would be similar to the following:

```xml
 <bpmn2:serviceTask id="ServiceTask_00b2568" implementation="content-connector.COPY_CONTENT" />
``` 
 
## Common input parameters
The following input parameters are common between content connector actions: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- | 
| `nodeId` | *Required.\** The node ID of the node to operate on in Alfresco Content Services. | String | dioos2a1-2a22-4f42-820d-c2b628751121 |
| `files` | *Required.\** A [file](../../files.md) uploaded in a process and set as a process variable or uploaded as part of a form or another connector to operate on. | File | |
| `folders` | *Required.\** A [process variable](../../processes/variables.md) of type folder. | Folder | |
| `locationPath` | *Required.\** The URI of a node to operate on. | String | |
| `searchQuery` | *Required.\** The query to use to find the nodes to operate on. | String | |
| `search_skipCount` | *Optional.* The number of items to exclude before generating a result set from the `searchQuery`. | Integer | 5 |
| `search_maxItems` | *Optional.* The maximum number of items to include in the result set from the `searchQuery`. | Integer | 1 |
| `search_rootNodeId` | *Optional.* The node ID to begin the `searchQuery` from. | String | fdcac4a4-1f51-4e02-820d-c2b669647671 |
| `search_include` | *Optional.* Additional information to return about the node from the `searchQuery`. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions |
| `search_orderBy` | *Optional.* Orders the list of properties returned by the `searchQuery`. | Array | |
| `search_fields` | *Optional.* Set the list of properties to be returned about the node in the response of the `searchQuery` | Array | id, name |

`*` One of these parameters is required to identify the content to operate on. 


--- Is searchQuery setting the language automatically? E.g. CMIS? 

## Common output parameters 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- | 
| `nodes` | *Optional.* A list of nodes operated on by the action. | Array | | 
| `responses` | *Required.* The response from the action. For example the metadata of a node. | JSON | |
| `content.error` | *Optional.* Any errors returned by the connector in performing the operation. | String | | 


## Create content
The `CREATE_NODE` action is used to upload files to the Alfresco Content Services repository. 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `name` | *Required.* A name for the file. | String | January Orders | 
| `autorename` | *Optional.* If set to `true` then a suffix will be added to the file name if a file with that name already exists.| Boolean | True | 
| `nodeType` | *Optional.* The node type for the new content. The default value is `cm:content` | String | cm:content | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |


## Update content
The `UPDATE_CONTENT` action is used to upload new versions of files to the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `name` | *Optional.* A name for the file. | String | January Orders | 
| `textSource` | *Optional.* The source for the update to the file in text format. | String | | 
| `fileSource` | *Optional.* The source for the update to the file in [file](../../files.md) variable format. | File | | 
| `majorVersion` | *Optional.* If set to `true` then a major version will be created for the file update. If set to `false` a minor version will be created. | Boolean | False | 
| `comment` | *Optional.* The version comment to appear in the versioning history. | String | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |


## Move content
The `MOVE_CONTENT` action is used to move files or folders to a new location in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeIdTarget` | *Required.* The node ID of the location to move the content to. | String | |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

## Copy content
The `COPY_CONTENT` action is used to make a copy of files or folders and move the copy to a new location in the Alfresco Content Services repository.

* Folder and children? 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeIdTarget` | *Required.* The node ID of the location to copy the content to. | String | |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

## Select a file
The `SELECT_FILE` action is used to select a file from the Alfresco Content Services repository to be used in the process.

No additional input parameters are required to select a file. The following output parameter is included that contains the selected file: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `files` | *Required.* The files returned as a [file](../../files.md) variable. | File | | 

## Delete content
The `DELETE_CONTENT` action is used to delete files and folders from the Alfresco Content Services repository.

* What happens to children if deleting a folder? 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `permanent` | *Optional.* If set to `true` the content will be permanently deleted instead of being moved to the trash can. | Boolean | False |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 

## Lock content
The `LOCK_NODE` action is used to lock a node in the Alfresco Content Services repository so that it cannot be edited.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeBodyLock` | *Required.* | JSON | |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 

## Unlock content
The `UNLOCK_NODE` action is used to unlock a node in the Alfresco Content Services repository so that it can be edited again.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 

## Set permissions on content
The `SET_PERMISSIONS` action is used to update the permissions for files or folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `users` | *Required.* A list of user IDs to update permissions for. | Array | | 
| `role` | *Required.* The roles to assign to the list of `users`. | String | |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

## Create a folder
The `CREATE_FOLDER` action is used to create new folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `folderName` | *Required.* A name for the folder. | String | | 
| `autorename` | *Optional.* If set to `true` then a suffix will be added to the folder name if a folder with that name already exists.| Boolean | True | 
| `nodeType` | *Optional.* The node type for the new content. The default value is `cm:file` | String | cm:file | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

## Select a folder
The `SELECT_FOLDER` action is used to select a folder from the Alfresco Content Services repository to be used in the process.

No additional input parameters are required to select a folder. The following output parameter is included that contains the selected folder: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `folders` | *Required.* The folder returned as a [process variable](../../processes/variables.md) of type folder. | Folder | | 


## Select metadata

SELECT_METADATA

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 

## Update metadata
The `UPDATE_METADATA` action is used to update the metadata for files and folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `metadata` | *Required,* The metadata to assign to the node. | | | 
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

## Update tags
The `UPDATE_TAG` action is used to update tags for files and folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `tag` | *Required.* A list of tags to update for the node. | Array | |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |


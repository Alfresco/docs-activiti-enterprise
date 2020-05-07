---
Title: Content connector
---

# Content connector
The content connector can be used to execute actions against content in [Alfresco Content Services (ACS)](http://docs.alfresco.com/6.2/concepts/welcome.html) as part of a process. The content connector is graphically represented by the Alfresco logo whilst modeling a process.

**Important**: The content connector requires a licensed version of Alfresco Content Services (ACS) to interact with.

The content connector does not require any connector variables to be set. It connects to the deployed ACS instance automatically. 

## Actions 
The following actions can be executed using the content connector: 

* [Create a file](#create-a-file)
* [Create a folder](#create-a-folder)
* [Update content](#update-content)
* [Move content](#move-content)
* [Copy content](#copy-content)
* [Delete content](#delete-content)
* [Update content permissions](#update-content-permissions)
* [Update content tags](#update-content-tags)
* [Update content metadata](#update-content-metadata)
* [Lock content](#lock-content)
* [Unlock content](#unlock-content)
* [Select a file](#select-a-file)
* [Select a folder](#select-a-folder)
* [Select content metadata](#select-content-metadata)

As an example, the `implementation` value of the copy content action in a service task would be similar to the following:

```xml
 <bpmn2:serviceTask id="ServiceTask_00b2568" implementation="content-connector.COPY_CONTENT" />
``` 
 
### Common input parameters
The following input parameters are common between content connector actions: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- | 
| `nodeId` | *Required.\** The node ID of the node to operate on in Alfresco Content Services. | String | dioos2a1-2a22-4f42-820d-c2b628751121 |
| `files` | *Required.\** A [file](../../files.md) uploaded in a project and set as a process variable or uploaded as part of a form or another connector to operate on. | File | |
| `folders` | *Required.\** A [process variable](../../processes/variables.md) of type folder. | Folder | |
| `locationPath` | *Required.\** The URI of a node to operate on. | String | alfresco:/bc67b437-f9e6-439c-b88b-6d49efa0f734 |
| `searchQuery` | *Required.\** The query to use to find the nodes to operate on. | String | |
| `search_skipCount` | *Optional.* The number of items to exclude before generating a result set from the `searchQuery`. | Integer | 5 |
| `search_maxItems` | *Optional.* The maximum number of items to include in the result set from the `searchQuery`. | Integer | 1 |
| `search_rootNodeId` | *Optional.* The node ID to begin the `searchQuery` from. | String | fdcac4a4-1f51-4e02-820d-c2b669647671 |
| `search_include` | *Optional.* Additional information to return about the node from the `searchQuery`. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions |
| `search_orderBy` | *Optional.* Orders the list of properties returned by the `searchQuery`. | Array | |
| `search_fields` | *Optional.* Set the list of properties to be returned about the node in the response of the `searchQuery` | Array | id, name |

`*` One of these parameters is required to identify the content to operate on. 

### Common output parameters 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- | 
| `nodes` | *Optional.* A list of nodes operated on by the action. | Array | | 
| `responses` | *Required.* An list of HTTP responses from the action. | JSON | |
| `content.error` | *Optional.* Any errors returned by the connector in performing the operation. | String | | 


### Create a file
The `CREATE_NODE` action is used to upload files to the Alfresco Content Services repository. 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `name` | *Required.* A name for the file including an extension. | String | January Orders.docx | 
| `autorename` | *Optional.* If set to `true` then a suffix will be added to the file name if a file with that name already exists.| Boolean | True | 
| `nodeType` | *Optional.* The node type for the new content. The default value is `cm:content` | String | cm:content | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Create a folder
The `CREATE_FOLDER` action is used to create new folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `folderName` | *Required.* A name for the folder. | String | 2020-shipments | 
| `autorename` | *Optional.* If set to `true` then a suffix will be added to the folder name if a folder with that name already exists.| Boolean | True | 
| `nodeType` | *Optional.* The node type for the new content. The default value is `cm:folder` | String | cm:file | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Update content
The `UPDATE_CONTENT` action is used to upload new versions of files to the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `name` | *Optional.* A name for the file including an extension. | String | January Orders.docx | 
| `textSource` | *Optional.* The source for the update to the file in text format. | String | | 
| `fileSource` | *Optional.* The source for the update to the file in [file](../../files.md) variable format. | File | orders-source | 
| `majorVersion` | *Optional.* If set to `true` then a major version will be created for the file update. If set to `false` a minor version will be created. | Boolean | False | 
| `comment` | *Optional.* The version comment to appear in the versioning history. | String | Major update to amend order. | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Move content
The `MOVE_CONTENT` action is used to move files or folders to a new location in the Alfresco Content Services repository. If a folder is moved then its children will also move to the new location. 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeIdTarget` | *Required.* The node ID of the location to move the content to. | String | 164e0082-5ac1-4afa-a09b-619a8c79f66g |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | False| 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Copy content
The `COPY_CONTENT` action is used to make a copy of files or folders and move the copy to a new location in the Alfresco Content Services repository. If a folder is copied then its children will also be replicated in the new location. 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeIdTarget` | *Required.* The node ID of the location to copy the content to. | String | 164e0082-5ac1-4afa-a09b-619a8c79f66g |
| `useFiles` | *Optional.* Set to `true` to include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to include folders in the search query. | Boolean | False | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Delete content
The `DELETE_CONTENT` action is used to delete files and folders from the Alfresco Content Services repository. If deleting a folder then its children will also be deleted.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `permanent` | *Optional.* If set to `true` the content will be permanently deleted instead of being moved to the trash can. | Boolean | False |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 

### Update content permissions
The `SET_PERMISSIONS` action is used to update the permissions for files or folders in the Alfresco Content Services repository.

**Note**: Only one role can be assigned at a time. 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `users` | *Required.* A list of users to update permissions for. | Array | hruser, hradmin | 
| `role` | *Required.* The role to assign to the list of `users`. | String | Contributor |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Update content tags
The `UPDATE_TAG` action is used to update tags for files and folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `tags` | *Required.* A list of tags to update for the node. | Array | 2020, orders, online |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

### Update content metadata
The `UPDATE_METADATA` action is used to update the metadata for files and folders in the Alfresco Content Services repository.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `metadata` | *Required,* The metadata to assign to the node. | *content-metadata** | | 
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 
| `include` | *Optional.* Additional information to return about the node. Values include: `properties`, `aspectNames`, `path`, `isLink`, `allowableOperations`, `association`, `isLocked`, `permissions`. | Array | properties, permissions | 
| `fields` | *Optional.* Set the list of properties to be returned about the node. | Array | id, name |

`*` Updating metadata allows for variables or values to be mapped to custom types and custom aspects from a defined [content model](../../content.md). 

### Lock content
The `LOCK_NODE` action is used to lock a node in the Alfresco Content Services repository so that it cannot be edited.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `nodeBodyLock` | *Required.* | JSON | *Example below.* |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 

The following is an example of the `nodeBodyLock` syntax:

```json
{
	"nodeId":"d57a6dd9-a11d-4c9d-b115-cea708efceda",
	"nodeBodyLock": {
		"type": "FULL",
		"lifetime": "PERSISTENT"
          }
  }
```

### Unlock content
The `UNLOCK_NODE` action is used to unlock a node in the Alfresco Content Services repository so that it can be edited again.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 

### Select a file
The `SELECT_FILE` action is used to select a file from the Alfresco Content Services repository to be used in the process.

No additional input parameters are required to select a file. The following output parameter is included that contains the selected file: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `files` | *Required.* The files returned as a [file](../../files.md) variable. | File | orders-file | 

### Select a folder
The `SELECT_FOLDER` action is used to select a folder from the Alfresco Content Services repository to be used in the process.

No additional input parameters are required to select a folder. The following output parameter is included that contains the selected folder: 

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `folders` | *Required.* The folder returned as a [process variable](../../processes/variables.md) of type folder. | Folder | 2020-shipments | 

### Select content metadata
The `SELECT_METADATA` action is used to select the metadata from a file or folder from Alfresco Content Services.

| Parameter | Description | Type | Example |
| --------- | ----------- | ---- | ------- |
| `useFiles` | *Optional.* Set to `true` to only include files in the search query.  | Boolean | True |
| `useFolders`| *Optional.* Set to `true` to only include folders in the search query. | Boolean | False | 

The following is an example response of the selected metadata:

```json
{
	"164e0082-5ac1-4afa-a09b-619a8c79f66f": {
		"cm:categories": [],
		"cm:lastThumbnailModification": [
			"imgpreview:1588671895595"
    ]
  }
}
```

## Events
The following events can be configured as event [triggers](../../triggers.md) using the content connector:

* [Content created](#content-created)
* [Content updated](#content-updated)
* [Content moved](#content-moved)
* [Content deleted](#content-deleted)

### Common output parameters
All events share a common set of output parameters:

| Parameter | Description | Type | Example |
| --------  | ----------- | ---- | ------- |
| `type` | The Java class of the resource. | String | |
| `id` | The node ID of the node that is created. | String | dioos2a1-2a22-4f42-820d-c2b628751121 | 
| `nodeType` | The content type that is created. | String | cm:file | 
| `isFile` | A flag for whether the node is a file. | Boolean | True | 
| `isFolder` | A flag for whether the node is a folder. | Boolean | False | 
| `primaryHierarchy` | An array of parent nodes | Array | ioos2a1-2a22-4f42-820d-c2b628751121, 164e0082-5ac1-4afa-a09b-619a8c79f66f | 
| `properties` | The properties of the created node. | JSON | |
| `affectedPropertiesBefore` | The properties of the node before the event. | JSON | | 
| `affectedPropertiesAfter` | The properties of the node after the event. | JSON | | 
| `aspects` | The list of aspects of the created node that are prefixed. | Array | cm:versionable | 
| `aspectNamesBefore` | A list of the prefixed aspect names before the event. | Array | cm:versionable | 
| `aspectNamesAfter` | A list of the prefixed aspect names after the event. | Array | cm:versionable, cm:reviewed | 

### Content created
This event is fired when a file or folder is created in Alfresco Content Services that matches the input parameters defined in the trigger event.

For a file the event is called `FILE_CREATED`. 

For a folder the event is called `FOLDER_CREATED`.

| Parameter | Description | Type | Example |
| --------  | ----------- | ---- | ------- |
| `nodeName` | The name of the node that is created. | String | 2020-orders | 
| `parentFolder` | The node ID of the parent folder. | String | dioos2a1-2a22-4f42-820d-c2b628751121 | 
| `nodeType` | The content type of the node that is created. | String | cm:file | 

### Content updated
This event is fired when the properties of a file or folder in Alfresco Content Services are updated to match the input parameters defined in the trigger event.

For a file the event is called `FILE_UPDATED`. 

For a folder the event is called `FOLDER_UPDATED`.

| Parameter | Description | Type | Example |
| --------  | ----------- | ---- | ------- |
| `properties` | The properties that must be matched for the event to be fired. | JSON | | 
| `parentFolder` | The node ID of the parent folder. | String | dioos2a1-2a22-4f42-820d-c2b628751121 | 
| `nodeType` | The content type of the node that is updated. | String | cm:file | 

### Content moved
This event is fired when a file or folder is moved to a new location in Alfresco Content Services that matches the input parameters defined in the trigger event.

For a file the event is called `FILE_MOVED`. 

For a folder the event is called `FOLDER_MOVED`.

| Parameter | Description | Type | Example |
| --------  | ----------- | ---- | ------- |
| `targetFolder` | The node ID of the folder the node is moved to for an event to fire. | String | 164e0082-5ac1-4afa-a09b-619a8c79f66f | 
| `nodeType` | The content type of the node that is moved. | String | cm:file | 

### Content deleted
This event is fired when a file or folder is deleted from Alfresco Content Services that matches the input parameters defined in the trigger event.

For a file the event is called `FILE_DELETED`. 

For a folder the event is called `FOLDER_DELETED`.

| Parameter | Description | Type | Example |
| --------  | ----------- | ---- | ------- |
| `parentFolder` | The node ID of the folder the node is deleted in. | String | dioos2a1-2a22-4f42-820d-c2b628751121 | 
| `nodeType` | The content type of the node that is deleted. | String | cm:file | 


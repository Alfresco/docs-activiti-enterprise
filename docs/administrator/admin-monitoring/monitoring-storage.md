---
Title: Application storage
---

# Application storage
Once an application is deployed, in progress tasks and processes are stored as nodes in an instance of Alfresco Content Services (ACS). Standalone tasks are not stored in ACS. This is an enterprise version of ACS and requires a separate license. 

**Note**: Information relating to tasks and processes is also stored in the relevant databases. The ACS repository is an optional additional storage option. 

The ACS instance that stores the task and process information can be accessed at the following URL: `gateway.{domain}.com/share`. 

## File uploads 
File uploads made using the [attach file field](../../modeling/modeling-forms/forms-fields.md#attach-file-fields) are stored in ACS. The uploads are stored as nodes in under the task ID which is stored under the process instance ID. 

## Application folder structure
The following is an example of a site that describes how tasks and processes are stored by referencing their IDs:

```
/{application-name}/
	/{process-instance-id}/
		/{task-id}/
			form-{task-id}.json
	/{process-instance-id}/
	/{process-instance-id}/
	/{process-instance-id}/
		/{task-id}/
			form-{task-id}.json
			{form-image-upload}.png
			{form-spreadsheet-upload}.xlsx
...
```

**Note**: The `form-{task-id}.json` file contains the values entered into a form when it was submitted. 

## Permissions
The permissions for task folders are assigned when a user [claims a task](../../workspace/workspace-tasks.md#claiming-a-task). 
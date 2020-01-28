---
Title: Projects
--- 

# Projects
A project is the top level component of your business process that you are modeling. They group multiple process definitions, forms and connectors into a logical project that represents this business process. A project needs to be released for version control and once a project has been released it can then be deployed. After a released project has been deployed (i.e. at runtime), it is referred to as an application. 

## Naming  
Project names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. 

The following are examples of valid project names: 

```
project-a
project4
two-hundred-and-twenty-one
```

## Collaborators
By default users can only view the projects they have created. The **Collaborators** option allows user access to be managed for individual projects. 

To add a collaborator to a project:

1. Check that the user has the [`ACTIVITI_MODELER` role](../administrator/identity/README.md).
2. Select the **Collaborators** option against the project.
3. Search for the user and click **Add**.

## Versioning and releasing
Projects are version controlled through a release function. The version will not be incremented until it has been [released](#releasing-a-project). A new project will begin at version 0 and increments by one every time it is released. 

**Note**: A project cannot be [deployed](../administrator/deploy/README.md) until it has been released to at least version 1. 

### Releasing a project
Use the following steps to release a project: 

1. Save any changes to the project components.
2. Navigate to the project list page of the Modeling Application. 
3. Use the ellipsis in the **Options** column of your project and select **Release**. 

After a project has been released the **Version** column will increment by one for that project.

## Storage
The [modeling service](../architecture/platform.md#modeling-service) uses an instance of PostgreSQL to store project and model definitions and maintain versions for released projects.
 
Released projects are the only projects that can be deployed and the only ones that will be visible in the [Administrator Application](../administrator/README.md).

### Folder structure
A folder is created for each project in the Modeling Application that contains a set structure for each element type. When you download a project, the extracted zip file will be in the same structure for every project.  

The following is an example of an exploded zip file of a project called *holiday* that contains a connector, a decision table, a UI, a form and two process definitions:

```
/holiday/
	/connectors/
		emailConnector.json
	/decision-tables/
		auto-approve-extensions.json
		auto-approve.xml
	/files/
		approval-policy.bin
		approval-policy-extensions.json	
	/forms/
		approval-form.json
	/ui/
		process.json
	/processes/
		approve-extensions.json
		approve.bpmn.xml
		request-extensions.json
		request.bpmn20.xml
	holiday.json
```

### Files
File definitions are created and stored for each element of a project:

* `<process-definition-name>.bpmn20.xml` is the format that process definitions are stored in.
* `<connector-name>.json` is the format that connector definitions are stored in. 
* `<form-name>.json` is the format that form definitions are stored in. 
* `<ui-name>.json` is the format that UI definitions are stored in for content or process. 
* `<decision-table-name>.xml` is the format that decision table definitions are stored in.
* `<decision-table-name>-extensions.json` is the format that decision table UIDs are stored in. 
* `<process-definition-name>-extensions.json` is the format that stores the links between process elements. For example it maps the `implementation` value of service tasks with the relevant connector actions and equivalent process variables. 
* `<file-name>.bin` is the binary format that uploaded files are stored as.
* `<file-name>-extensions.json` is the format that stores the metadata for the associated uploaded file. 
* `<project-name>.json` is the project manifest that stores the name and version of a project.

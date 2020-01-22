---
Title: User tasks
---

# User tasks
User tasks represent a stage in the process where human action is required.

Human action is handled by a task being assigned to specific users or groups. The task that is assigned is modeled using a [form](../../forms/README.md). Once a task is completed, the process flow continues on to the next element in the process. 


## Properties
The properties that can be set for a user task are the following: 

#### ID
The unique identifier for the user task. This is system generated and cannot be altered.

| Property | Example | Required | 
| -------- | ------- | -------- | 
| ID | `UserTask_0gpdh83` | Yes |

#### Name
The name of the user task. This will appear in the XML definition and be visible in the process diagram. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Name | `Flavor order` | No |

#### Documentation
A free text description of what the user task does.

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Documentation | `A form to choose the flavor of ice cream.`  | No |

#### Assignment
The users or groups that are able to complete the task. A single user can be set as the `activiti:assignee` or candidates can be used. Candidates are a list of users or groups that may [claim a task](../../../workspace/tasks.md#claiming-a-task). Candidate users are stored in the `activiti:candidateUsers` property and candidate groups in `activiti:candidateGroups`.

Users and groups can be set from three different sources: 

* **Static** values are a free text field that has no validation against users in the [Identity Service](../../../administrator/identity/service.md). The text entered will require an exact match to a `username` in the Identity Service to be assigned.   

* **Identity** allows for users and groups to be searched in the [Identity Service](../../../administrator/identity/service.md) and selected for the assignment.

* **Expression** allows for an expression using variables to be used to select users and groups for the assignment. Expressions can be a simple process variable such as `${userToAssign}` or an expression such as `${userDetails.username}` that uses a process variable of type JSON. A JSON editor is provided for creating expressions for assignment.

**Note**: Users and groups that are selected as assignees or candidates in a user task are automatically added as [users](../../../administrator/identity/README.md#permissions) when deploying an application if they are set using the static or identity options. Setting an assignee or candidate using the expression source will require the potential users or groups to be manually assigned users when deploying an application. 

#### Due date 
An optional due date and time for a user task in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format. A date picker can be used to choose the time and date.

Checking the **Use process variable** box for due date allows for a [process variable](../variables.md) to be passed to generate the date. The process variable must be of type `datetime`. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Due date | `2020-01-01T10:00:00`  | No |

#### Multi-instance type
The type of [multi-instance](../bpmn/multi.md) execution for the user task. The default value is `None`. 

`Sequential` executions only ever have a single instance of that user task running at any one time. The next instance will only start after the previous one has been completed. 

`Parallel` executions start multiple instances of a user task at once, meaning they are all active and can all be worked on at the same time. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Multi-instance type | `None`  | No |

#### Priority
A comparative priority for the user task between zero and four. The priority property is to aid end-users in their task management.  

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Priority | `2`  | No |

#### Form selector
The [form](../../forms/README.md) to present to the user completing the task. The form must exist within the same project as the process definition to be selected. 

A new form can be created using the **Create Form** symbol and a previously selected form can be edited using the **Open Form** symbol. 

Adding a form to a process will add the property `activiti:formKey` to the user task XML. The value of the `formKey` is the `id` of the selected form. 

| Property | Example | Required | 
| -------- | ------- | -------- | 
| Form selector | `flavor-choice`  | No |

#### Mapping type
Mapping type sets whether [process variables](../variables.md) are sent to a user task as task variables and whether the form fields and task variables from a user task are sent back to a process as process variables once a task has been completed. 

If variables are sent to and from a user task explicit mapping can be configured between process variables and task variables and form fields using the **Input mapping** and **Output mapping** fields. If no explicit mapping is set, implicit mapping will be used by attempting to match `name` fields. Only exact matches will map. 

Mappings are stored in the `<process-name>-extensions.json` file and can be viewed in the **Extensions Editor**. 

Mapping type is only visible if a form has been selected. The default value is `Send all variables`. 

## Process diagram
User tasks are graphically represented by a single thin, rounded rectangle with a user icon inside. 

The XML representation of a user task is: 

```xml
<bpmn2:userTask id="UserTask_0gpdh83" name="Order" activiti:formKey="form-38098a3e-bff1-46cb-ba0f-0c94fdb287ed" activiti:assignee="hruser" activiti:dueDate="2020-01-01T01:00:00+aa" activiti:priority="2">
	<bpmn2:documentation>A form to choose the flavor of ice cream.</bpmn2:documentation>
	<bpmn2:incoming>SequenceFlow_02eaofe</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_14ma5mo</bpmn2:outgoing>
</bpmn2:userTask>
```
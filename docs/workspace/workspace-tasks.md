---
Title: Workspace tasks
---

# Workspace tasks
The tasks section displays information about all tasks within the application that the currently signed in user has access to. The default views are for tasks assigned to the the current user under **My Tasks** and all completed tasks under **Completed Tasks**. 

There are two types of tasks: 

* Tasks that are part of a process instance are created automatically when a process flow reaches a [user task](../modeling/modeling-processes/processes-bpmn/bpmn-user.md). Task details including assignation and the attached form are set within the process definition as part of the user task. The Process Workspace is used only to (optionally) claim the form and complete it.

* Standalone tasks are created outside of a process definition and have no impact on process instances. The Process Workspace can be used to create standalone tasks, assign them and (optionally) attach a form that has been defined in the application to them. 

The possible statuses for tasks are:

| Status | Description |
| ------ | ----------- |
| CREATED | The task has not yet been assigned |
| ASSIGNED | The task is assigned but has not been completed |
| SUSPENDED | The task has been suspended and needs to be reactivated to continue |
| COMPLETED | The task has been completed |
| CANCELLED | The task has been cancelled and cannot be completed |

## Creating a standalone task
Use the following steps to start a new task:

1. Click **Create** and select **New Task**.
2. Enter the details for the task:

 	| Field | Description | Required? |
 	| ----- | ----------- | --------- |
 	| Name | The name of the task | Yes |
 	| Description | A description of the task | No |
 	| Priority | The relative priority of the task as an `integer` | No |
 	| Due Date | The date the task needs to be completed by | No |
 	| User | The user the task will be assigned to | Yes |
 	| Candidate Group | | |

3. Click **Start** to start the task. 

### Attaching a form to a standalone task
To attach a form to a standalone task, the task must first have been created. The form to attach must also have been defined in the same application that the instance of Process Workspace is running in.

Use the following steps to attach a form to a standalone task:

1. Find the task you want to attach a form to in the **My Tasks** list and click it.
2. Select the **Attach Form** option.
3. Choose the form to attach from the dropdown list and then click **Attach Form**.

## Assigning a task

## Task details
Selecting a specific task opens up an information panel which displays basic properties about the task. 

| Property | Description | Editable | 
| -------- | ----------- | -------- | 
| Assignee | The user the task is assigned to | No |
| Status | The current status of the task | No | 
| Priority | The relative priority of the task as an `integer` | Yes | 
| Due date | The date the task needs to be completed by | Yes |
| Category | | No |
| Created | The date the task was first created on | No |
| Parent name | | No |
| Parent task id | | No |
| End date | | No |
| Id | The task ID | No |
| Description | The description of the task | No |

## Filtering tasks
It is possible to update the two default views for tasks by changing the filters, or create a new custom view by clicking the **Customize your filter** section on any view. 

The options for customizing a filter are as follows: 

| Option | Description | 
| ------ | ----------- |
| Status | The status of process instance to display in the filter |
| Assignee | The user the task is assigned to |
| Task name | The name of the task to filter on | 
| Sort by | The column to sort by: `Id`, `Name`, `CreatedDate` or `Priority` | 
| Order by | Whether the ordering is ascending or descending |

Once you have customized your filter, there are two options: 

* **Save filter**: Selecting this will save the filter over the current view.
* **Save filter as**: Selecting this will give you the option to provide a name for a new view for your filter and add it under the **Tasks** section. 

You can use the **Delete filter** option at any time to remove a view. 

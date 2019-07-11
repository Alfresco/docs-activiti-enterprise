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
 	| User | The user the task will be assigned to | Yes, if *Candidate Group* is blank |
 	| Candidate Group | The group of users the task will provisionally be assigned to. The task must then be [claimed](#claiming-a-task) by a single user | Yes, if *User* is blank |
 	| Form | A dropdown list of all forms available to link to a standalone task | No | 

	**Note**: Only [forms that have been set to attach to standalone tasks](../modeling/modeling-forms/README.md#forms-in-standalone-tasks) will be visible. 

3. Click **Start** to start the task. 

## Claiming a task
A task can be claimed if it has no user assigned. Tasks with no candidate group assigned are available to be claimed by any user, whilst those with a candidate group assigned can only be claimed by a member of that group. Completed and cancelled tasks cannot be claimed. 

Use the following steps to claim a task: 

1. Search for tasks with a **Status** of **CREATED**. 

	**Note**: Make sure the **Assignee** column is blank when filtering **CREATED** tasks.

2. Click on the task to claim to view its properties.
3. Click the **Claim** button. The task **Status** will update to **ASSIGNED**.  

## Unclaiming a task
A task can be unclaimed if the user it is assigned to decides to no longer work on it. Completed and cancelled tasks cannot be unclaimed.  

Use the following steps to unclaim a task:

1. Search for tasks with a **Status** of **ASSIGNED**.

	**Note**: This is the default filter for **My Tasks** 

2. Click on the task to unclaim to view its properties.
3. Click the **Unclaim** button. The task **Status** will update to **CREATED**. 

## Task details
Selecting a specific task opens up an information panel which displays basic properties about the task. 

| Property | Description | Editable | 
| -------- | ----------- | -------- | 
| Assignee | The user the task is assigned to | No |
| Status | The current status of the task | No | 
| Priority | The relative priority of the task as an `integer` | Yes | 
| Due date | The date the task needs to be completed by | Yes |
| Category | The categories assigned to a task | No |
| Created | The date the task was first created on | No |
| Parent name | The name of the parent task if the selected task is a subtask | No |
| Parent task id | The task ID of the parent task if the selected task is a subtask | No |
| End date | The date the task was completed| No |
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

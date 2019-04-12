---
Title: Workspace tasks
---

# Workspace tasks
The tasks section displays information about all tasks within the application that the currently signed in user has access to. The default views are for tasks assigned to the the current user under **My Tasks** and all completed tasks under **Completed Tasks**. 

There are two types of tasks: 

* Tasks that are part of a process instance are created automatically when a process flow reaches a [user task](). Task details including assignation and the attached form are set within the process definition as part of the user task. The Process Workspace is used only to (optionally) claim the form and complete it.

* Standalone tasks are created outside of a process definition and have no impact on process instances. The Process Workspace can be used to create standalone tasks, assign them and (optionally) attach a form that has been defined in the application to them. 

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

## Filtering tasks






There is a set of preset filters applied to process and task lists that can be customized or saved into new lists. 

Each view has a customizable filter associated with it, that can be used to change the task status on display, assigned user displayed and options on the sort field and order. Editing a filter gives the option to save it in the current view or as a new view. 


The possible statuses for tasks are:

| Status | Description |
| ------ | ----------- |
| CREATED | The task has not yet been assigned |
| ASSIGNED | The task is assigned but has not been completed |
| SUSPENDED | The task has been suspended and needs to be reactivated to continue |
| COMPLETED | The task has been completed |
| CANCELLED | The task has been cancelled and cannot be completed |

## Task details
Selecting a specific task opens up an information panel which displays basic properties about the task. 



---
Title: Application monitoring
---

# Process instances
Process instances allows you to monitor all active and suspended processes that are running on an application-by-application and process definition basis. Users require the APS_ADMIN role to view process instances.

Filtering the default display allows you to select to view process instances from a specific application model, and to further filter by the process definitions within that application. 

As soon as a process has been started, it can be monitored in process instances. The possible statuses of each instance are as follows:

| Status | Description |
| ------ | ----------- |
| RUNNING | Process instance is currently running |
| COMPLETED | Process instance is complete |
| SUSPENDED | Process instance is currently suspended and cannot continue until it is reactivated |
| CANCELLED | Process instance has been cancelled and cannot be completed |

Selecting a specific process instance opens up an information panel which displays basic properties about the instance. 

A process instance can be deleted by selecting the **Delete** action from the ellipsis dropdown menu next to the instance you want to remove. You can also view the **Variables** in the process from the same place. 

The option to **Suspend** or view the **Diagram** for process instances is only available to those in progress. Similarly, you can only **Activate** a suspended process instance. 

Ticking the checkbox at top of the panel allows you to select multiple process instances and perform a bulk action against them; **Delete**, **Suspend**, or **Activate**. 

# Tasks
Tasks allow you to monitor tasks of every status on an application-by-application basis and search for tasks by name or specific status. Users require the APS_ADMIN role to view tasks. 

Using the filter option allows you to select to view tasks from a specific application, name and status.

The possible statuses for tasks are:
	
| Status | Description |
| ------ | ----------- |
| CREATED | Task has not yet been assigned |
| ASSIGNED | Task is assigned but has not been completed |
| SUSPENDED | Task has been suspended and needs to be reactivated to continue |
| CANCELLED | Task has been cancelled and cannot be completed |
| COMPLETED | Task has been completed |

Selecting a specific task opens up an information panel which displays basic properties about a task. The Due Date, Assignee, Priority and Description of a task can be updated using this panel. 

Ticking the checkbox at top of the panel allows you to select multiple tasks and perform a bulk Delete, Suspend or Activate action against your selection. 


---
Title: Monitoring tasks and processes
---

# Monitoring tasks and processes
The functions available in the **Process Admin** section of the Administrator Application are to: 

* [Monitor processes](#monitoring-processes) in the **Process Instances** section.
* [Monitor tasks](#monitoring-tasks) in the **Tasks** section. 
* View [audit events](#audit) in the **Audit** section.

To access these functions users require the *APS_ADMIN* role. 

**Note**: In progress tasks and processes can be [stored as nodes in Alfresco Content Services](../admin-monitoring/monitoring-storage.md).

## Monitoring processes
The **Process Instances** section is for monitoring all active, completed and suspended processes that are running on an application-by-application and process definition basis.

Filtering the default display allows you to select to view process instances from a specific application model, and to further filter by the process definitions within that application. 

As soon as a process has been started, it can be monitored in process instances. The possible statuses of each instance are as follows:

| Status | Description |
| ------ | ----------- |
| RUNNING | The process instance is currently running |
| COMPLETED | The process instance is complete |
| SUSPENDED | The process instance is currently suspended and cannot continue until it is reactivated |
| CANCELLED | The process instance has been cancelled and cannot be completed |

Selecting a specific process instance opens up an information panel which displays basic properties about the instance. 

A process instance can be deleted by selecting the **Delete** action from the ellipsis dropdown menu next to the instance you want to remove. You can also view the **Variables** in the process from the same place. 

The option to **Suspend** or view the **Diagram** for process instances is only available to those in progress. Similarly, you can only **Activate** a suspended process instance. 

Ticking the checkbox at the top of the panel allows you to select multiple process instances and perform a bulk action against them; **Delete**, **Suspend**, or **Activate**. 

## Monitoring tasks
The **Tasks** section is for monitoring tasks of every status on an application-by-application basis and search for tasks by a specific name or status. 

Using the filter option allows you to select to view tasks from a specific application, by name and/or status.

The possible statuses for tasks are:
	
| Status | Description |
| ------ | ----------- |
| CREATED | The task has not yet been assigned |
| ASSIGNED | The task is assigned but has not been completed |
| SUSPENDED | The task has been suspended and needs to be reactivated to continue |
| CANCELLED | The task has been cancelled and cannot be completed |
| COMPLETED | The task has been completed |

Selecting a specific task opens up an information panel which displays basic properties about a task. The **Due Date**, **Assignee**, **Priority** and **Description** of a task can be updated using this panel. 

Ticking the checkbox at top of the panel allows you to select multiple tasks and perform a bulk **Delete**, **Suspend** or **Activate** action against your selection. 

## Audit
The **Audit** section of the Administrator Application provides an overview of events that have occurred in tasks and processes.  

The information about each event contains the following properties:

| Property | Description | 
| -------- | ----------- | 
| Event Type | The type of the event that occurred for example when a task was created or assigned |
| Event Time | The time that the event happened | 
| Entity ID | The ID of the entity described | 
| Process Instance ID | The process instance ID for the event |
| Entity | The payload of the event | 

Using the filter option allows for events to be filtered by application, event type, entity ID and process ID. 
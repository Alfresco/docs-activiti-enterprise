---
Title: Monitoring processes
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




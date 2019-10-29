---
Title: Workspace process instances
---

# Workspace process instances
The processes section displays information about all process instances in an application. The default views are for currently running process instances under **Running Processes** and all completed process instances under **Completed Processes**. There is a further view that shows  process instances of all status types called **All Processes**. If you select a process instance it will display the [tasks](../workspace/tasks.md) associated to it. 

The possible statuses of a process instance are as follows:

| Status | Description |
| ------ | ----------- |
| RUNNING | The process instance is currently running |
| SUSPENDED | The process instance is currently suspended and cannot continue until it is reactivated |
| COMPLETED | The process instance is complete |
| CANCELLED | The process instance has been cancelled and cannot be completed |

## Starting a process instance
Use the following steps to start a new process instance:

1. Click **Create** and select **New Process**.
2. Enter a name for the process instance and select the process definition to use from the dropdown list.
3. Click **Start** to start the process instance. 

## Filtering process instances
It is possible to update the three default views for process instances by changing the filters, or create a new custom view by clicking the **Customize your filter** section on any view. 

The options for customizing a filter are as follows: 

| Option | Description | 
| ------ | ----------- |
| Status | The status of process instance to display in the filter |
| Sort by | The column to sort by: `Id`, `Name`, `Status` or `StartDate` | 
| Order by | Whether the ordering is ascending or descending |
| Last modified from | The date the process instance was last modified from | 
| Last modified to | The date the process instance was last modified to |

Once you have customized your filter, there are two options: 

* **Save filter**: Selecting this will save the filter over the current view.
* **Save filter as**: Selecting this will give you the option to provide a name for a new view for your filter and add it under the **Processes** section. 

You can use the **Delete filter** option at any time to remove a view. 








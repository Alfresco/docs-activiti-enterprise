---
Title: User tasks
---

# User tasks
User tasks represent a stage in the process where a human action is required.

Human action is handled by a task being assigned to specific users or groups. The task that is assigned is usually modeled using a form. Once the form is completed, the process flow continues on to the next element. 

[Process variables](../README.md#process-variables) can be mapped from the originating process to [form fields](../../modeling-forms/forms-fields.md) using form field `id` in a user task as inputs. Similarly, form field data from the completed user task can be transferred back to process variables in the process as outputs. The mapping between process variables and form variables is stored in the `<process-name>-extensions.json` file for the process definition under the `mappings` section. 

The possible assignments for a user task are:

* Assignee: a specific user is defined.
* Candidate user: a list of users that may claim the task. 
* Candidate group: a group(s) of users that can claim the task. 

User tasks are graphically represented by a single thin, rounded rectangle with a user icon inside. 

The XML representation of a user task is: 

```xml
<bpmn2:userTask id="UserTask_1" activiti:assignee="testuser" activiti:dueDate="2019-02-23T19:08:00" activiti:priority="medium" />
```
---
Title: Forms
---

# Forms
Forms are used to capture data into specially designed field types such as text, date, file uploads and multiple choice radio buttons. They are where human intervention is required within a process and this intervention is handled by filling in a form that is displayed in a user’s task list within the [Alfresco Process Workspace](../../workspace/workspace-tasks.md). 

## Using forms
Forms can be assigned to [user tasks](../modeling-processes/processes-bpmn/bpmn-user.md) or [start events](../modeling-processes/processes-bpmn/bpmn-start.md) as part of a process. The only difference to how a form functions between the two elements is that it is not possible to save a form assigned to a start event. 

## Designing forms
Forms are created in a similar fashion to processes, by using a drag-and-drop GUI. There is also a JSON editor for forms that updates as new elements are added and removed from the form. 

Forms can be split into multiple tabs and each tab individually named to logically split up form field sections and questions. 

Each row of form fields added allows that row to be split into a number of columns; 1, 2, 3, 4, 6, 12. Column count between rows does not have to match on the same tab or form.

### Form fields
All form fields display a field editor when they are created in the GUI. Each has a tab for general properties that are common amongst the majority of form fields, with some also having a tab for advanced properties that are specific to that field type. 

The [general properties](../modeling-forms/forms-fields.md) that are shared amongst all form fields are described with any exceptions noted in the description of individual field types.

### Form values
Form values are the values that a user enters into a form when they are filling it in.

Form values can receive a default value from a [process variable](../modeling-processes/README.md#process-variables). To pass this default value to a form field, set the process variable `name` as the form field `id`. 

When a form that is part of a [user task](../modeling-processes/processes-bpmn/bpmn-user.md) is submitted, all of the values that have been entered into the form fields are created as [process variables](../modeling-processes/README.md#process-variables) within the parent process. These process variables are created using the name of the form fields. If a process variable already exists with the name of a form field then the value will be overwritten. 

### Form variables
Form variables are used to display the value of a variable using a [display value](../modeling-forms/forms-fields.md#display-value-fields) field.

A form definition without any fields selected will display an **Edit Form Variables** button in the properties panel. Form variables can be configured using the inbuilt GUI or the JSON editor provided with it.

### Forms in standalone tasks
Forms can be attached to standalone tasks that are not associated with a process. When designing a form in the UI there is a checkbox called **Visible in standalone task** for deciding whether a form can be used as part of a standalone task. This sets the property `standAlone` to `true` in the form definition if it can be used in a standalone task. 
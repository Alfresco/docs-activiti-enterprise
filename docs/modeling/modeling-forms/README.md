---
Title: Forms
---

# Forms
Forms are used to capture data into specially designed field types such as text, date, file uploads and multiple choice radio buttons. They are where human intervention is required within a process and this intervention is handled by filling in a form that is displayed in a user’s task list within the [Alfresco Process Workspace](../workspace/workspace-tasks). 

## Using forms
Forms can be assigned to [user tasks](../bpmn/user-tasks.md) or [start events](../bpmn/start-events.md) as part of a process. The only difference to how a form functions between the two elements is that it is not possible to save a form assigned to a start event. 

## Designing forms
Forms are created in a similar fashion to processes, by using a drag-and-drop GUI. There is also a JSON editor for forms that updates as new elements are added and removed from the form. 

Forms can be split into multiple tabs and each tab individually named to logically split up form field sections and questions. 

Each row of form fields added allows that row to be split into a number of columns; 1, 2, 3, 4, 6, 12. Column count between rows does not have to match on the same tab or form.

### Form fields
All form fields display a field editor when they are created in the GUI. Each has a tab for general properties that are common amongst the majority of form fields, with some also having a tab for advanced properties that are specific to that field type. 

The [general properties](../modeling-forms/forms-fields.md) that are shared amongst all form fields are described with any exceptions noted in the description of those fields.

### Form variables


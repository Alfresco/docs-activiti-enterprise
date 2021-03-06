---
Title: Forms
---

# Forms
Forms are used to capture data into specially designed field types such as text, date, file uploads and multiple choice radio buttons. They are where human intervention is required within a process and this intervention is handled by filling in a form that is displayed in a user’s task list within the [Alfresco Process Workspace](../../workspace/tasks.md). 

## Naming  
Form names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. 

The following are examples of valid form names: 

```
gamma-form
hr-form-request
one-hundred-and-thirty-one
```

## Using forms
Forms can be assigned to [user tasks](../processes/bpmn/user.md) or [start events](../processes/bpmn/start.md) as part of a process. The only difference to how a form functions between the two elements is that it is not possible to save a form assigned to a start event.

## Designing forms
Forms are created in a similar fashion to processes, by using a drag-and-drop GUI. There is also a JSON editor for forms that updates as new elements are added and removed from the form. 

Forms can be split into multiple tabs and each tab individually named to logically split up form field sections and questions. 

Each row of form fields added allows that row to be split into a number of columns; 1, 2, 3, 4, 6, 12. Column count between rows does not have to match on the same tab or form.

### Form fields
All form fields display a field editor when they are created in the GUI. Each has a tab for general properties that are common amongst the majority of form fields, with some also having a tab for advanced properties that are specific to that field type. 

The [general properties](../forms/fields.md) that are shared amongst all form fields are described with any exceptions noted in the description of individual field types.

### Form values
Form values are the values that a user enters into a form when they are filling it in.

Form values can receive a default value from a [process variable](../processes/variables.md). To pass this default value to a form field, map the process variable to the form field `id`. 

The default behaviour when a form that is part of a [user task](../processes/bpmn/user.md) is submitted, is to create all of the values that have been entered into the form fields as process variables within the parent process. This behaviour is defined by [the type of mapping defined in the process](../processes/variables.md). If all fields are passed without defining any mapping then process variables are created using the `id` of the form fields. If a process variable already exists with the same `name` as the form field `id` then the value of the original process variable will be overwritten. 

### Form variables
Form variables can be used to pass and receive values from [process variables](../processes/variables.md). They can be used to set the [visibility conditions](../forms/fields.md#visibility-conditions) of form fields and to [display values](../forms/fields.md#display-value-fields) to form fillers.

A form without any BPMN element selected will display an **Edit Form Variables** button in the properties panel. Form variables can be configured using the inbuilt GUI or the JSON editor provided with it.

Each form variable has four properties:  

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `name` | A unique name that can contain alphanumeric characters and underscores but must begin with a letter | `var_3` |
| `type` | A data type selected from a dropdown. See the following table for a list of data types | `String` |
| `required` | Sets whether the form variable must contain a value when a task is started | `false` | 
| `value` | An optional default value for the form variable | `ice-cream` |

The following are the data types that form variables can be set as:

| Type | Description | Example | 
| ---- | ----------- | ------- |
| String | A sequence of characters | `#Mint-Ice-Cream-4!`
| Integer | A positive whole number | `642` |
| Boolean | A value of either `true` or `false` | `true` |
| Date | A specific date in the format `YYYY-MM-DD` | `2020-04-22` | 
| Datetime | A specific date and time in the format `YYYY-MM-DD HH:mm:ss` | `2020-09-10 22:30:00`
| File | A [file](../modeling/files.md) uploaded into a process definition or as part of a process instance or task | 
| JSON | A JSON object | `{"flavor" : "caramel"}` | 

### Forms in standalone tasks
Forms can be attached to standalone tasks that are not associated with a process. When designing a form in the UI there is a checkbox called **Visible in standalone task** for deciding whether a form can be used as part of a standalone task. This sets the property `standAlone` to `true` in the form definition if it can be used in a standalone task. The default value is `true` and this will be used if the property is not set in the form definition.  

 
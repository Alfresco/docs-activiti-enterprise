---
Title: Form fields
---

# Form fields
The majority of form fields share the same generic properties and all share the same options for visibility conditions. 

## General properties
The general properties that are shared between all types of form fields are the following: 

| Property | Description |
| -------- | ----------- |
| `Label` | The name of the field that will appear on the rendered form | 
| `ID` | The unique ID of the field | 
| `Required?` | Checking this box makes a field mandatory |
| `Read-only` | Sets whether the field can be filled in by a form filler or not | 
| `Colspan` | The number of columns a field spans | 
| `Placeholder` | The default value of the field |

**Note**: Any exceptions to the following general properties are stated in the section specific to that field type. For example, the header field type does not have the `Required?` property.

## Visibility conditions
Visibility conditions allow for form fields to be hidden or displayed depending upon the values of other fields or variables within that form. The visibility conditions button is displayed on the general properties tab of every field. 

| Step | Description | Options | 
| ---- | ----------- | ------- |
| Depends on | The field or variable that will be evaluated | A field or variable within the form |
| If it's | The comparison operator between *Depends on* and *Value* | `equal to`, `not equal to`|
| Value | The value, field or variable that the *Depends on* step is evaluating against | A field or variable within the form or a value |
| Next condition operator | The operator for evaluating against further conditions | `and`, `and not`, `or`, `or not`|

**Note**: Conditions are evaluated in the order they are defined in.

## Form field types
The form field types available to use in Activiti Enterprise are the following: 

| Form field | Description | 
| ---------- | ----------- | 
| [Amount](#amount-fields) | Amount fields are for entering a value depicting currency |
| [Attach file](#attach-file-fields) | Attach file fields are used so that form fillers can upload files with their submission |
| [Checkbox](#checkbox-fields) | Checkbox fields are `boolean` values |
| [Date](#date-fields) | Date fields are for `date` type data |
| [Display text](#display-text-fields) | Display text fields allow the form designer to display a line of fixed text to the form filler |
| [Display value](#display-value-fields) | Display value fields allow the form designer to display a value previously entered in the form |
| [Dropdown](#dropdown-fields) | Dropdown fields allow the form designer to define a set of options a form filler must choose from a a dropdown menu|
| [Group](#group-fields) | Group fields allow the form filler to select groups that have access to the application | 
| [Header](#headers) | Header fields are subtitle fields that can be used as section containers |
| [Hyperlink](#hyperlink-fields) | Hyperlink fields allow the form designer to expose a link that form fillers can click on |
| [Multiline text](#multiline-text-fields) | Multiline text fields are for entering `string` data across multiple lines |
| [Number](#number-fields) | Number fields are for entering `integer` data |
| [Outcome](#outcomes) | Outcomes can be added as options at the bottom of a form that a form filler needs to click on to complete the form |
| [People](#people-fields) | People fields allow the form filler to select users that have access to the application | 
| [Radio buttons](#radio-buttons) | Radio button fields allow the form designer to define a set of options a form filler must choose from a set of radio buttons |
| [Text](#text-fields) | Text fields are for entering `string` data in a single line |

### Amount fields
Amount fields are for entering a value depicting currency.

The advanced properties for an amount field are the following:

| Property | Description |
| -------- | ----------- |
| `Min value` | Sets the minimum value that can be entered into the field | 
| `Max value` | Sets the maximum value that can be entered into the field | 
| `Currency` | Sets the currency symbol for the field. For example: `$`, `£` or `€` |
| `Enable decimals` | Checking this box will allow for decimal places to be entered into the field |

### Attach file fields
Attach file fields are used so that form fillers can upload files with their submission.

**Note**: Attaching files to a form requires an [Alfresco Content Services](../../architecture/platform.md#alfresco-content-services) license so that the [uploads can be stored](../../administrator/monitoring/storage.md).

The advanced properties for an attach file field are the following:

| Property | Description |
| -------- | ----------- |
| `Allow multiple files to be attached` | Checking this box will allow for more than one file to be uploaded | 
| `Just link to files, do not copy files to Alfresco Activiti Enterprise` | Checking this box means that the form submission only contains the path to the upload(s) rather than uploading the actual file(s) | 
| `File source` | Sets the location for where files can be uploaded from. `Alfresco Content` is from an ACS instance, whilst `Local file` is local to the form filler. |

### Checkbox fields
Checkbox fields are `boolean` values. They are either checked or unchecked. 

Checkbox fields do not have the `Placeholder` property, nor do they have an advanced properties tab. 

### Date fields
Date fields are for `date` type data. 

The advanced properties for a date field are the following:

| Property | Description |
| -------- | ----------- |
| `Min date` | Sets the earliest date that can be entered into the field | 
| `Max date` | Sets the latest date that can be entered into the field | 
| `Date format` | Sets the format of how a date is entered into the field. For example: `YYYY-MM-DD` would display as *2001-10-01* for 1st October, 2001. |

### Display text fields
Display text fields allow the form designer to display a line of fixed text to the form filler. This text is uneditable by the filler themselves. The `Text to display` property is used to enter the text.

Display text fields not have the `Read-only`, `Placeholder` and `Required?` fields, nor do they have an advanced properties tab.

### Display value fields
Display value fields allow the form designer to display a value previously entered in the form. The `variables` property is used to select a [form variable](../forms/README.md#form-variables) to display.

Display value fields not have the `Read-only`, `Placeholder` and `Required?` fields, nor do they have an advanced properties tab. 

### Dropdown fields
Dropdown fields allow the form designer to define a set of options a form filler must choose from a list. This list can be a manually entered set of options or it can read from a REST service. 

The advanced properties for a manual dropdown field allow for a set of options to be entered with a `name` and `id` for each option set. Selecting the radio next to an option will set it as the *empty value*. An *empty value* is taken to mean the field is empty if this option is selected when the form is filled in. 

The advanced properties for a REST dropdown field are the following: 

| Property | Description |
| -------- | ----------- |
| `REST URL` | The URL of the REST service |
| `Path to array in JSON response` | The path to the JSON response. Enter `.` to use the full path |
| `ID property` | The ID of the REST service |
| `Label property` | The name of the REST service |

### Group fields
Group fields allow form fillers to select a single or multiple groups from the list of groups available in the application. 

Group fields do not have a `Placeholder` property. 

The advanced properties for a group field are the following: 

| Property | Description |
| -------- | ----------- |
| `Mode` | Sets whether only a single, or multiple groups can be selected | 
 
### Headers
Header fields are subtitle fields that can be used as section containers on a tab. They cannot be filled in by a form filler as they only display a subtitle. 

Header fields have a `Number of columns` property rather than the `Colspan` property and they do not have the `Read-only`, `Placeholder` and `Required?` properties.

The advanced properties for a header field are the following:

| Property | Description |
| -------- | ----------- |
| `Allow collapse` | Checking this box allows the header container to be collapsed with a `+` or `-` when the form is filled in | 
| `Collapse by default` | Checking this box will load the form with the header section already collapsed. This property is only available if the `Allow collapse` property is checked. |

### Hyperlink fields
Hyperlink fields allow the form designer to expose a link that form fillers can click on whilst they are filling out a form.

Hyperlink fields do not have the `Read-only` and `Placeholder` properties.

The advanced properties for a hyperlink field are the following:

| Property | Description |
| -------- | ----------- |
| `Hyperlink URL` | The URL that the field will launch when clicked |
| `Display text` | The text that is displayed for the URL. For example: *Click here to view the expenses policy* |

### Multiline text fields
Multiline text fields are for entering `string` data across multiple lines.

The advanced properties for a multiline text field are the following:

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |

### Number fields
Number fields are for entering `integer` data. 

The advanced properties for a number field are the following:

| Property | Description |
| -------- | ----------- |
| `Min value` | Sets the minimum `integer` value that can be entered into the field | 
| `Max value` | Sets the maximum `integer` value that can be entered into the field | 

### Outcomes
Outcomes can be added as options at the bottom of a form that a form filler needs to click on to complete the form, such as *Agree* or *Disagree*. 

### People fields
People fields allow form fillers to select a single or multiple users from the list of users that have access to the application. 

People fields do not have a `Placeholder` property. 

The advanced properties for a people field are the following: 

| Property | Description |
| -------- | ----------- |
| `Mode` | Sets whether only a single, or multiple users can be selected | 

### Radio buttons
Radio button fields allow the form designer to define a set of options a form filler must choose from. This list can be a manually entered set of options or it can read from a REST service.

The advanced properties for a manual radio button field are allow for a set of options to be entered with a `name` and `id` for each option set. Selecting the radio next to an option will set it as the default value.

The advanced properties for a REST radio button field are the following: 

| Property | Description |
| -------- | ----------- |
| `REST URL` | The URL of the REST service |
| `Path to array in JSON response` | The path to the JSON response. Enter `.` to use the full path |
| `ID property` | The ID of the REST service |
| `Label property` | The name of the REST service |

### Text fields
Text fields are for entering `string` data in a single line. 

The advanced properties for a text field are the following: 

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |
| `Input mask` | Set the format for how data may be entered into the field. For example `(00) 0000-0000` for a mandatory 8-digit phone number and 2-digit area code will not allow for letters to be entered at all |
| `Reversed` | This reverses the entry for an `Input mask` and reads the text from right to left instead | 
| `Input mask placeholder` | The placeholder to demonstrate the format of an `Input mask`. For example `(__) ____-____` in the phone number example  | 
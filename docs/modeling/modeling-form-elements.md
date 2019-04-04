---
Title: Form elements
---

# 

## Form fields
All form fields display a field editor when they are created in the GUI. Each has a tab for general properties that are common amongst the majority of form fields, and some elements also have a tab for advanced properties that are specific to that element type. 

The general properties for form fields are the following: 

**Note**: Any exceptions to the following general properties are stated in the section specific to that field type. For example, the header element does not have a required flag. 

| Property | Description |
| -------- | ----------- |
| `Label` | The name of the field that will appear on the rendered form | 
| `ID` | The unique ID of the field | 
| `Override ID` | Checking this box will allow the default ID to become editable | 
| `Required?` | Checking this box makes a field mandatory |
| `Colspan` | The number of columns a field spans | 
| `Placeholder` | The default value of the field |

## Text
Text fields are for entering `string` data in a single line. 

The advanced properties for a text field are the following: 

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field |
| `Max length` | Sets the maximum character count for text that can be entered into the field |
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |
| `Reversed` | |
| `Input mask placeholder` | |

## Multiline text
Multiline text fields are for entering `string` data across multiple lines.

The advanced properties for a multiline text field are the following:

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field |
| `Max length` | Sets the maximum character count for text that can be entered into the field |
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |

## Number
Number fields are for entering `integer` data only. 

Number fields do not have an advanced properties tab.

## Checkbox
Checkbox fields are `boolean` values. They are either checked or unchecked. 

Checkbox fields do not have the `placeholder` property, nor do they have an advanced properties tab.  

## Date
Date fields are for `date` type data only. 

The advanced properties for a date field are the following:

| Property | Description |
| -------- | ----------- |
| `Min date` | Sets the earliest date that can be entered into the field | 
| `Max date` | Sets the latest date that can be entered into the field | 
| `Date format` | Sets the format of how a date is entered into the field. For example: `YYYY-MM-DD` would display as *2001-10-01* for 1st October, 2001. |

## Dropdown
Dropdown fields allow the modeler to define a set of options a form filler must choose from a list. This list can be a manually entered set of options or can read from a REST service. 

##Amount
Amount fields are for entering a value depicting currency.

The advanced properties for an amount field are the following:

| Property | Description |
| -------- | ----------- |
| `Min value` | Sets the minimum value that can be entered into the field | 
| `Max value` | Sets the maximum value that can be entered into the field | 
| `Currency` | Sets the currency symbol for the field. For example: `$`, `£` or `€` |
| `Enable decimals` | Checking this box will allow for decimal places to be entered into the field |

## Radio button 
Radio button fields allow the modeler to define a set of options a form filler must choose from. This list can be a manually entered set of options or can read from a REST service.

## Hyperlink
Hyperlink fields allow the modeler to expose a link that form fillers can click on whilst they are filling out a form.

The advanced properties for a hyperlink field are the following:

| Property | Description |
| -------- | ----------- |
| `Hyperlink URL` | The URL that the field will launch when clicked |
| `Display text` | The text that is displayed for the URL. For example: *Click here to view the expenses policy* |

## Header 
Header fields are subtitle fields that can be used as section containers on a tab. They cannot be filled in by a form filler as they only display a subtitle. 

Header fields have a `Number of columns` property rather than the `colspan` property and they do not have the `placeholder` and `required` properties.

The advanced properties for a header field are the following:

| Property | Description |
| -------- | ----------- |
| `Allow collapse` | Checking this box allows the header container to be collapsed with a `+` or `-` when the form is filled in | 
| `Collapse by default` | Checking this box will load the form with the header section already collapsed. This property is only available if the `Allow collapse` property is checked. |

## Attach file
Attach file fields are used so that form fillers can upload files with their submission.

The advanced properties for an attach file field are the following:

| Property | Description |
| -------- | ----------- |
| `Allow multiple files to be attached` | Checking this box will allow for more than one file to be uploaded | 
| `Just link to files, do not copy files to Alfresco Activiti Enterprise` | Checking this box means that the form submission only contains the path to the upload(s) rather than uploading the actual file(s) | 
| `File source` | Sets the location for where files can be uploaded from. `All file sources` has no restrictions on the source location, whilst `Local file` |

## Display value
Display value fields allow the modeler to display a value previously entered in the form. The `label` property is used to enter the value to display.

Display value fields not have the `placeholder` and `required` fields, nor do they have an advanced properties tab. 

## Display text
Display text fields allow the modeler to display free text to the form filler. This text is uneditable by the filler themselves. The `Text to display` property is used to enter the text.

Display text fields not have the `placeholder` and `required` fields, nor do they have an advanced properties tab.

## Outcome
Outcome

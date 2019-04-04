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
| `Override ID` | Checking this box will allow the default ID to become editable | 
| `Required?` | Checking this box makes a field mandatory |
| `Colspan` | The number of columns a field spans | 
| `Placeholder` | The default value of the field |

**Note**: Any exceptions to the following general properties are stated in the section specific to that field type. For example, the header field type does not have a required flag.

## Visibility conditions
Visibility conditions allow for form fields to be hidden or shown depending upon the values of other fields or variables within that form.

## Form field types
The form field types available to use in Activiti Enterprise are the following: 

| Form field | Description | 
| ---------- | ----------- | 
| Amount | Amount fields are for entering a value depicting currency |
| Attach file | Attach file fields are used so that form fillers can upload files with their submission |
| Checkbox | Checkbox fields are `boolean` values |
| Date | Date fields are for `date` type data |
| Display text | Display text fields allow the form designer to display a line of fixed text to the form filler |
| Display value | Display value fields allow the form designer to display a value previously entered in the form |
| Dropdown | Dropdown fields allow the form designer to define a set of options a form filler must choose from a a dropdown menu|
| Header | Header fields are subtitle fields that can be used as section containers |
| Hyperlink | Hyperlink fields allow the form designer to expose a link that form fillers can click on |
| Multiline text | Multiline text fields are for entering `string` data across multiple lines |
| Number | Number fields are for entering `integer` data |
| Outcome | Outcomes can be added as options at the bottom of a form that a form filler needs to click on to complete the form |
| Radio buttons | Radio button fields allow the form designer to define a set of options a form filler must choose from a set of radio buttons |
| [Text](../forms-fields/fields-text.md) | Text fields are for entering `string` data in a single line |












## Text
Text fields are for entering `string` data in a single line. 

The advanced properties for a text field are the following: 

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |
| `Reversed` | |
| `Input mask placeholder` | |

## Multiline text
Multiline text fields are for entering `string` data across multiple lines.

The advanced properties for a multiline text field are the following:

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |

## Number
Number fields are for entering `integer` data. 

Number fields do not have an advanced properties tab.

## Checkbox
Checkbox fields are `boolean` values. They are either checked or unchecked. 

Checkbox fields do not have the `placeholder` property, nor do they have an advanced properties tab.  

## Date
Date fields are for `date` type data. 

The advanced properties for a date field are the following:

| Property | Description |
| -------- | ----------- |
| `Min date` | Sets the earliest date that can be entered into the field | 
| `Max date` | Sets the latest date that can be entered into the field | 
| `Date format` | Sets the format of how a date is entered into the field. For example: `YYYY-MM-DD` would display as *2001-10-01* for 1st October, 2001. |

## Dropdown
Dropdown fields allow the form designer to define a set of options a form filler must choose from a list. This list can be a manually entered set of options or can read from a REST service. 

## Amount
Amount fields are for entering a value depicting currency.

The advanced properties for an amount field are the following:

| Property | Description |
| -------- | ----------- |
| `Min value` | Sets the minimum value that can be entered into the field | 
| `Max value` | Sets the maximum value that can be entered into the field | 
| `Currency` | Sets the currency symbol for the field. For example: `$`, `£` or `€` |
| `Enable decimals` | Checking this box will allow for decimal places to be entered into the field |

## Radio button 
Radio button fields allow the form designer to define a set of options a form filler must choose from. This list can be a manually entered set of options or can read from a REST service.

## Hyperlink
Hyperlink fields allow the form designer to expose a link that form fillers can click on whilst they are filling out a form.

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
Display value fields allow the form designer to display a value previously entered in the form. The `label` property is used to enter the value to display.

Display value fields not have the `placeholder` and `required` fields, nor do they have an advanced properties tab. 

## Display text
Display text fields allow the form designer to display a line of fixed text to the form filler. This text is uneditable by the filler themselves. The `Text to display` property is used to enter the text.

Display text fields not have the `placeholder` and `required` fields, nor do they have an advanced properties tab.

## Outcomes
Outcomes can be added as options at the bottom of a form that a form filler needs to click on to complete the form, such as *Agree* or *Disagree*. 

---
Title: Text fields
--- 

# Text fields
Text fields are for entering `string` data in a single line. 

## Advanced properties
The advanced properties for a text field are the following: 

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |
| `Reversed` | |
| `Input mask placeholder` | |

## JSON definition
An example of a text field `JSON` definition is: 

```json
{
	"fieldType": "FormFieldRepresentation",
	"id": "uniquecode",
	"name": "Unique code",
	"type": "text",
	"value": null,
	"required": true,
	"readOnly": true,
	"overrideId": false,
	"colspan": 1,
	"placeholder": null,
	"minLength": 8,
	"maxLength": 8,
	"minValue": null,
	"maxValue": null,
	"regexPattern": "/^[A-Za-z]{4}\\d{4}$/",
	"visibilityCondition": {},
	"params": {
		"existingColspan": 1,
		"maxColspan": 2,
		"inputMaskReversed": false
}
```
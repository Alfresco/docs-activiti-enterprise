---
Title: Multiline text fields
---

# Multiline text fields
Multiline text fields are for entering `string` data across multiple lines.

## Advanced properties
The advanced properties for a multiline text field are the following:

| Property | Description |
| -------- | ----------- |
| `Min length` | Sets the minimum character count for text that can be entered into the field including whitespace |
| `Max length` | Sets the maximum character count for text that can be entered into the field including whitespace|
| `Regex pattern` | A regular expression can be entered that will validate the text entered into the field. For example, a regular expression that matches four letters followed by four digits would be: `/^[A-Za-z]{4}\d{4}$/` |

## JSON definition
An example of a multiline text field `JSON` definition is: 
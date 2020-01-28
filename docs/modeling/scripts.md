---
Title: Scripts
---

# Scripts 
Scripts are used to execute a script as part of a process. Process variables can be passed to the script and the results of a script can be sent back to a process instance as process variables.

Scripts are executed outside of the [runtime bundle]() by the [script service]() with the results being returned to the process after execution. Script design uses the functionality of  [Monaco](https://github.com/Microsoft/monaco-editor) and the [Graal javascript engine](https://github.com/graalvm/graaljs) for execution. 

Scripts can be added to a process definition by using a [script task](../modeling/processes/bpmn/script.md).

The properties of a script are:

| Property | Description | Example | Required |
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the script. Script names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. | order-script | Yes |
| `Language` | The development language the script is written in | Javascript | Yes | 

## Modeling scripts

### Script variables

The following are the data types that script variables can be set as:

| Type | Description | Example | 
| ---- | ----------- | ------- |
| String | A sequence of characters | `#Mint-Ice-Cream-4!`
| Integer | A positive whole number | `642` |
| Boolean | A value of either `true` or `false` | `true` |
| Date | A specific date in the format `YYYY-MM-DD` | `2020-04-22` | 
| Datetime | A specific date and time in the format `YYYY-MM-DD HH:mm:ss` | `2020-09-10 22:30:00`
| File | A [file](../modeling/files.md) uploaded into a process definition or as part of a process instance or task | 
| JSON | A JSON object | `{"flavor" : "caramel"}` | 

### Process commands

### Content APIs






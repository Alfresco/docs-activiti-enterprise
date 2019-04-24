---
Title: Decision tables
--- 

# Decision tables
Decision tables are used to manage business decisions within process workflows. They adhere to the Decision Model and Notation (DMN) standard. 

## Using decision tables
Decision tables can be selected from the palette when designing a process. Once they have been dragged into the process definition, a dropdown list of decision tables available to the current project is displayed. 

**Note**: Only a single decision table can be defined per process definition. 

Decision tables are handled as [service tasks](../modeling-connectors/README.md) by Activiti Enterprise and will always have the `implementation` value of `dmn-connector.EXECTUTE_TABLE`. The name of the decision table that is associated to the service task is stored in the [`<process-name>-extensions.json` file](../modeling-projects.md#files) as the `value` under the input `_activiti_dmn_table_`.

The following is an example of the XML for a decision table within a process definition:

```xml
<bpmn2:serviceTask id="ServiceTask_0gzhx4b" implementation="dmn-connector.EXECUTE_TABLE" />
```

## Designing decision tables

### General

### Inputs
Decision tables have at least one input. Inputs are the values that will evaluated against rules. 

### Outputs
Decision tables have at least one output. Outputs are the value(s) of the results a decision table can come to after inputs are evaluated against rules. 

### Rules


### Hit policies 
Hit policies define how many rules can be matched in a decision table and which of the results are included in the output. 

The default hit policy is `UNIQUE`. 

| Hit policy | Description |
| ---------- | ----------- |
| `U`: `UNIQUE` | <ul><li> Only a single rule can be matched </li></ul> <ul><li> If more than one rule is matched the hit policy is violated </ul></li> |
| `A`: `ANY` | <ul><li> Multiple rules can be matched </ul></li> <ul><li> All matching rules must have identical entries for their output </ul></li> <ul><li> If matching rules have different output entries the hit policy is violated </ul></li> | 
| `F`: `FIRST` | <ul><li> Multiple rules can be matched </ul></li> <ul><li> Only the output of the first rule that is matched will be used </ul></li> <ul><li> Rules are evaluated in the order they are defined in the decision table </ul></li> | 
| `R`: `RULE ORDER` | <ul><li> Multiple rules can be matched </ul></li> <ul><li> All outputs are returned in the order that rules are defined in the decision table </ul></li> | 
| `C`: `COLLECT` | Multiple rules can be satisfied and multiple outputs will be generated with no ordering |
| `P` : `PRIORITY` | <ul><li> Multiple rules can be matched </ul></li> <ul><li> Only the output with the highest priority will be used </ul></li> <ul><li> Priority is calculated based on the order rules are specified in descending order </ul></li> | 
| `O` : `OUTPUT ORDER` | <ul><li> Multiple rules can be matched </ul></li> <ul><li> All outputs are returned in the order that output values are defined in the decision table

### FEEL
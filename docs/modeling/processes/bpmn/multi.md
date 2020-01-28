---
Title: Multi-instance
---

# Multi-instance
Multi-instance allows for certain BPMN elements to be repeated within a process. There are two options for how to execute multi-instance elements: sequentially or in parallel. 

Sequential executions only ever have a single instance running at any one time. The next instance will only start after the previous one has been completed. 

Parallel executions start all instances at once, meaning they are all active and can all be worked on at the same time. 

Multi instance elements are graphically represented by three parallel lines at the bottom of the original element. Sequential lines are horizontal and parallel lines are vertical. 

The XML representation of multi instance includes a child property in the original element.

For sequential multi-instance elements:

```xml
<bpmn2:multiInstanceLoopCharacteristics isSequential="true" />
```

For parallel multi-instance elements:

```xml
<bpmn2:multiInstanceLoopCharacteristics  isSequential="false" />
```

or

```xml
<bpmn2:multiInstanceLoopCharacteristics />

```

The following BPMN elements can have multiple instances:

* [User tasks](../bpmn/user.md)
* [Service tasks](../bpmn/service.md)
* [Business rule tasks](../bpmn/business.md)
* [Call activities](../bpmn/call.md)
* [Embedded sub-processes](../bpmn/sub.md#expanded-and-collapsed-sub-processes)

## Variables 
Each multi-instance execution contains the following three variables: 

| Variable | Description |
| -------- | ----------- | 
| `nrOfInstances` | The total number of instances |
| `nrOfActiveInstances` | The number of currently active instances. For sequential multi-instances the value will always be 1 |
| `nrOfCompletedInstances` | The number of instances that have already been completed | 

**Note**: These variables can be used in multi-instance expressions without having to be declared as [process variables](../../processes/variables.md).

Each instance in the multi-instance execution also has an instance-local variable that is not visible to other instances, nor to the process instance:

| Variable | Description |
| -------- | ----------- | 
| `loopCounter` | The index in the for-each loop of that particular instance |

## Cardinality
The number of instances to be executed can be set by the cardinality of the multi-instance item. This can be set as a static value, a [process variable](../../processes/variables.md) or calculated as an expression. 

The XML representation of cardinality if the following: 

```xml
<bpmn2:multiInstanceLoopCharacteristics>
	<bpmn2:loopCardinality>5</bpmn2:loopCardinality>
</bpmn2:multiInstanceLoopCharacteristics>
```

## Collection
A collection can be used to set the number of instances to be executed by referencing a list of items. 

An element variable can optionally be used with a collection. An element variable is used to create a variable for each instance of the multi-instance element and each variable created by the element variable is assigned one value from the collection.

The following XML is an example of a [user task](../bpmn/user.md) using a collection:

```xml
<bpmn2:userTask id="UserTask_1n1uk4a" activiti:assignee="{users}">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:multiInstanceLoopCharacteristics activiti:collection="${userList.users}" activiti:elementVariable="users">
	</bpmn2:multiInstanceLoopCharacteristics>
</bpmn2:userTask>
```

The `activiti:collection` references a process variable called `userList` that contains the following JSON:

```json
{"userList":["user1", "user2", "user3"]}
```

In the example:

* Three user tasks will be created because there are three items in the process variable that `activiti:collection` uses.
* A variable will be created for each instance called `users` with the values `user1`, `user2` and `user3` because the `activiti:elementVariable` is set to `"users"`. 
* A user tasks will be assigned to each of the users because the `activiti:assignee` is set to `{users}` which is the name of the variable created in each instance by the element variable.

## Completion condition 
A completion condition can be optionally included for multi-instances. When the completion condition evaluates to `true`, all remaining instances are cancelled and the multi-instance activity ends.

In the following example, the completion condition will be met when 60% of instances have been completed and the remaining 4 instances will be cancelled:

```xml
<bpmn2:multiInstanceLoopCharacteristics>
	<bpmn2:loopCardinality>10</bpmn2:loopCardinality>
	<bpmn2:completionCondition>${nrOfCompletedInstances/nrOfInstances >= 0.6 }</bpmn2:completionCondition>
</bpmn2:multiInstanceLoopCharacteristics>
```

## Results
A result collection can be set to aggregate the results from instances into a variable. The result collection is created as a process variable after instance execution has finished. 

The result element variable is used to select the field or variable from the BPMN element to aggregate into the result collection. 

In the following example, the user task will run 4 times sequentially and the values of `flavor` from the form will be stored as a JSON object in the variable `choices`: 

```xml
<bpmn2:userTask id="UserTask_1n1uk4a" activiti:assignee="{users}">
	<bpmn2:multiInstanceLoopCharacteristics isSequential="true">
		<bpmn2:loopCardinality>4</bpmn2:loopCardinality>
		<bpmn2:loopDataOutputRef>choices</bpmn2:loopDataOutputRef>
		<bpmn2:outputDataItem name="flavor" />
	</bpmn2:multiInstanceLoopCharacteristics>
</bpmn2:userTask>
```

The process variable `choices` will contain JSON similar to the following:

```json
{"choices":["chocolate", "mint", "strawberry"]}
```
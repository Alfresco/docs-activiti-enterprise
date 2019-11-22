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
<bpmn2:multiInstanceLoopCharacteristics />
```

The following BPMN elements can have multiple instances:

* [User tasks]()
* [Service tasks]()
* [Business rule tasks]()
* [Call activities]()
* [Embedded sub-processes]()

## Variables 
Each multi-instance execution contains the following three variables: 

| Variable | Description |
| -------- | ----------- | 
| `nrOfInstances` | The total number of instances |
| `nrOfActiveInstances` | The number of currently active instances. For sequential multi-instances the value will always be 1 |
| `nrOfCompletedInstances` | The number of instances that have already been completed | 

**Note**: These variables can be used in multi-instance expressions without having to be declared as [process variables]().

Each instance in the multi-instance execution also has an instance-local variable that is not visible to other instances, nor to the process instance:

| Variable | Description |
| -------- | ----------- | 
| `loopCounter` | The index in the for-each loop of that particular instance |

## Cardinality
The number of instances to be executed can be set by the cardinality of the multi-instance item. This can be set as a static value, a [process variable]() or calculated as an expression. 

The XML representation of cardinality if the following: 

```xml
<bpmn2:multiInstanceLoopCharacteristics>
	<bpmn2:loopCardinality>5</bpmn2:loopCardinality>
</bpmn2:multiInstanceLoopCharacteristics>
```

## Collection
A collection can be used to set the number of instances to be executed. A collection can optionally be used with an element variable that is created as a process instance.

In the following example, three instances will be created with one assigned to each of the users listed in the process variable `userList`. To assign this to the three users in question the `assignee` field of a user task would need to be set to `${users.username}`:

```xml
<bpmn2:multiInstanceLoopCharacteristics isSequential="true" activiti:collection="${userList.users}" activiti:elementVariable="users"></bpmn2:multiInstanceLoopCharacteristics>
```

The `activiti:collection` in this example refers to a process variable called `userList`:

```json
{"users":[{"username":"hruser"},{"username":"superadminuser"},{"username":"foodmanager"}]}
```

## Completion condition 
A completion condition can be optionally included for multi-instances. When the completion condition evaluates to `true`, all remaining instances are cancelled and the multi-instance activity ends.

In the following example, the completion condition will be met when 60% of instances have been completed and the remaining 4 instances will be cancelled:

```xml
<bpmn2:multiInstanceLoopCharacteristics>
	<bpmn2:loopCardinality>10</bpmn2:loopCardinality>
	<bpmn2:completionCondition>${nrOfCompletedInstances/nrOfInstances >= 0.6 }</bpmn2:completionCondition>
</bpmn2:multiInstanceLoopCharacteristics>
```








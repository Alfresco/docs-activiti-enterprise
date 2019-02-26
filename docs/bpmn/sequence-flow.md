---
Title: Sequence flows
---

# Sequence flows

Sequence flows represent the direction of flow in a process.

When attached to specific BPMN elements, sequence flows gain additional attributes. For example, the condition variables that are evaluated at an exclusive gateway are defined within the sequence flow properties themselves using the Condition Expression property. This only appears on sequence flows that are adjacent to an element requiring condition evaluation.  

Sequence flows are graphically represented by single black lines with an arrow indicating the direction of flow. 

The XML representation of a sequence flow is: 

```xml	
<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
<bpmn2:outgoing>SequenceFlow_2</bpmn2:outgoing>
```

Or:

```xml
<bpmn2:sequenceFlow id="SequenceFlow_1" name="no" sourceRef="ExclusiveGateway_1" targetRef="Task_1">
	<bpmn2conditionExpression xsi:type="bpmn:tFormalExpression">${content.approved == false}</bpmn2:conditionExpression>
</bpmn2:sequenceFlow>
```
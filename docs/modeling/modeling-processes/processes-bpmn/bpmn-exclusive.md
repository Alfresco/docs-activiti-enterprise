---
Title: Exclusive gateways
---

# Exclusive gateways

Exclusive gateways represent a decision within a process.

Once the process flow reaches an exclusive gateway, all of the outgoing sequence flow options are evaluated in the order they are defined. The first option that evaluates to true is the sequence flow that is followed. 

Exclusive gateways are graphically represented by a single thin diamond shape with an X icon inside. 

Note: a single thin diamond on its own defaults to an exclusive gateway, however the BPMN specification does not allow diamonds with, and without, an X in the same process definition. 

The XML representation of exclusive gateways is:

```xml
<bpmn2:exclusiveGateway id="ExclusiveGateway_1" name="Content Accepted?">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_2</bpmn2:outgoing>
	<bpmn2:outgoing>SequenceFlow_3</bpmn2:outgoing>
</bpmn2:exclusiveGateway>
    <bpmn2:sequenceFlow id="SequenceFlow_1" sourceRef="Task_1" targetRef="ExclusiveGateway_1" />
    <bpmn2:sequenceFlow id="SequenceFlow_2" name="yes" sourceRef="ExclusiveGateway_1" targetRef="Task_2">
		<bpmn2:conditionExpression xsi:type="bpmn:tFormalExpression">${content.approved == true}</bpmn2:conditionExpression>
	</bpmn2:sequenceFlow>
    <bpmn2:sequenceFlow id="SequenceFlow_3" name="no" sourceRef="ExclusiveGateway_1" targetRef="Task_3">
		<bpmn2:conditionExpression xsi:type="bpmn:tFormalExpression">${content.approved == false}</bpmn2:conditionExpression>
    </bpmn2:sequenceFlow>
```
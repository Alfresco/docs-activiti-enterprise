---
Title: Gateways
---

# Gateways
Gateways are used to deal with convergence and divergence of the process flow. They allow for more than one fork of a process to be followed, or they can evaluate conditions so that a different route may be followed for each specific set of circumstances.

The following are gateways:

* [Exclusive gateways](#exclusive-gateways)
* [Parallel gateways](##parallel-gateways)

## Exclusive gateways
Exclusive gateways represent a decision within a process.

Once the process flow reaches an exclusive gateway, all of the outgoing sequence flow options are evaluated in the order they are defined. The first option that evaluates to true is the sequence flow that is followed. 

Exclusive gateways are graphically represented by a single thin diamond shape with an X icon inside. 

**Note**: A single thin diamond on its own defaults to an exclusive gateway, however the BPMN specification does not allow diamonds with, and without, an X in the same process definition. 

The XML representation of an exclusive gateways is:

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

## Parallel gateways
Parallel gateways represent a concurrent fork or a join in a process.

Forking means that all sequence flows exiting a parallel gateway are executed concurrently. A joining parallel gateway waits for all concurrent sequence flows to arrive at the gateway, before continuing with the process. 

Parallel gateways do not evaluate conditions. Any conditions set on a sequence flow will be ignored by the parallel gateway. 

It is possible for a single parallel gateway to execute both forking and joining behaviour. 

Parallel gateways are graphically represented by a single thin diamond shape with a + icon inside. 

The XML representation of a parallel gateway is: 

```xml
<bpmn2:parallelGateway id="Fork_1">
	<bpmn2:sequenceFlow id="SequenceFlow_1" sourceRef="Fork_1" targetRef="UserTask_1" />
 	</bpmn2:sequenceFlow>
 	<bpmn2:sequenceFlow id="SequenceFlow_2" sourceRef="Fork_1" targetRef="ServiceTask_1" />
 	</bpmn2:sequenceFlow>
</bpmn2:parallelGateway>
```
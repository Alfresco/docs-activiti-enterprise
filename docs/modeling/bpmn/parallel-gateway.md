---
Title: Parallel gateways
---

# Parallel gateways

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
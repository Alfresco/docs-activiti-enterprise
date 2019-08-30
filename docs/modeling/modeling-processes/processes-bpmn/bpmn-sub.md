---
Title: Sub-processes
---

# Sub-processes 
Sub-processes are also known as embedded sub-processes. A sub-process is an element that contains other BPMN elements that define an additional process within the parent process. Sub-processes can be expanded or collapsed. Elements for the sub-process can only be dragged into an expanded sub-process. Use the spanner icon against a sub-process to toggle between a collapsed and expanded state in the Modeling Application. 

A sub-process requires a start and end event. The sequence flow within a sub-process cannot cross the boundary without the sub-process completing. The advantage of a sub-process is that it creates its own scope within a process. This allows for [boundary events](../processes-bpmn/bpmn-boundary.md) to be attached to the sub-process. 

**Note**: When a sub-process is executed as part of a process instance, it does not receive a new `processInstanceId`. The elements within the sub-process will be executed under the ID of the parent process. [Process variables](../README.md#process-variables) are also shared between a sub-process and its parent with no additional mapping required. 

Whilst expanded, sub-processes are graphically represented by a single, thin rounded rectangle with the other BPMN elements they contain visible. 

Whilst collapsed, sub-processes are graphically represented by a single, thin rounded rectangle with a `+` symbol. The BPMN elements they contain are not visible in this state. 

The XML representation of a sub-process is: 

```xml
<bpmn2:subProcess id="SubProcess1">
	<bpmn2:incoming>SequenceFlow_8</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_9</bpmn2:outgoing> 
	...
</bpmn2:subProcess>
```

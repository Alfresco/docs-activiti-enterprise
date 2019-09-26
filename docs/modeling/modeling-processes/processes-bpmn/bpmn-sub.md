---
Title: Sub-processes
---

# Sub-processes 
A sub-process is an element that contains other BPMN elements that define an additional process within the parent process. 

**Note**: When a sub-process is executed as part of a process instance, it does not receive a new `processInstanceId`. The elements within the sub-process will be executed under the ID of the parent process. [Process variables](../README.md#process-variables) are also shared between a sub-process and its parent with no additional mapping required. 

The following are sub-processes: 

* [Expanded and collapsed sub-processes](#expanded-and-collapsed-sub-processes)
* [Event sub-processes](#event-sub-processes)

## Expanded and collapsed sub-processes
Sub-processes are also known as embedded sub-processes can be expanded or collapsed. Elements for the sub-process can only be dragged into an expanded sub-process. Use the spanner icon against a sub-process to toggle between a collapsed and expanded state in the Modeling Application. 

A sub-process requires a start and an end event. Only a [standard start event](../processes-bpmn/bpmn-start.md#start-events) can be used in embedded sub-processes. The sequence flow within a sub-process cannot cross its boundary without the sub-process completing. The advantage of a sub-process is that it creates its own scope within a process. This allows for [boundary events](../processes-bpmn/bpmn-boundary.md) to be attached to the sub-process. 

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

## Event sub-processes
Event sub-processes are triggered by an event and require a start and end event. As they are triggered by events an event sub-process can't be started by a standard start event. Instead start events such as [error start events](../processes-bpmn/bpmn-start.md#error-start-events) or [message start events](../processes-bpmn/bpmn-start.md#message-start-events) are used. 

Start events attached to event sub-processes can also be set as interrupting or non-interrupting by using the `activiti:isInterrupting` property. See [boundary events](../processes-bpmn/bpmn-boundary.md) for details on interrupting and non-interrupting behaviour. 

Event sub-processes are not connected to the main process flow as they can only be triggered by an event. The XML for an event sub-process contains the `triggeredByEvent` property set to `true`.  

Event sub-processes can be placed at the process level or within another [sub-process](#expanded-and-collapsed-sub-processes).

Event sub-processes are graphically represented by a single, thin dotted rectangle. 

The XML representation of an event sub-process is: 

```xml
<bpmn2:subProcess id="EventSubProcess2" triggeredByEvent="true">
	...
</bpmn2:subProcess>
```

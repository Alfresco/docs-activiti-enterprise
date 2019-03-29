---
Title: Start events
---

# Start events

A process must always contain at least one start event as they define how a process starts.

Start events are where the trigger is unspecified for starting a process. 

Start events can have a form associated with them to begin the process, though it is important to note that the process will not begin until the form has been submitted. Alternatively, processes with a start event can be started manually. 

Start events are also used when a process is started through the API by using the POST v1/process-instances startProcess method.

Start events are graphically represented by a single thin circle without an icon inside.

The XML representation of start events is a start event declaration without a sub-element declaring a specific type: 

```xml
<bpmn2:startEvent id="StartProcess_1">
	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
</bpmn2:startEvent>
```

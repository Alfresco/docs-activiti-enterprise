---
Title: Call activities
---

# Call activities
Call activities are used to start an instance of another process definition. 

The `calledElement` property uses the `processId` of a process definition to define the type of process to start.

Call activities are graphically represented by a single, thick rounded rectangle without an icon inside. 

The XML representation of a call activity is: 

```xml
<bpmn2:callActivity id="Task_5" name="Start request process" calledElement="process-a6d6ca00-cbb6-45d6-ae24-50ef53d37cc4">
	<bpmn2:incoming>SequenceFlow_8</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_9</bpmn2:outgoing>
</bpmn2:callActivity>
```
**Note**: Call activities can only be used to start a process instance of a process definition that exists in the same application as the process that is calling it.
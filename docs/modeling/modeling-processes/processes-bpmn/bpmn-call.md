---
Title: Call activities
---

# Call activities
Call activities are used to start an instance of another process definition. The original process waits until the called process is complete before continuing with its own process flow.

The `calledElement` property uses a `processDefinitionId` to define the type of process to start. 

[Process variables](../README.md#process-variables) need to be mapped if they need to be passed from the originating process to the called process as inputs. Similarly, mapping is required to transfer process variable values from the completed called process back to process variables in the originating process as outputs. The mapping between process variables is stored in the `<process-name>-extensions.json` file for the process definition under the `mappings` section. 

Call activities are graphically represented by a single, thick rounded rectangle without an icon inside. 

The XML representation of a call activity is: 

```xml
<bpmn2:callActivity id="Task_5" name="Start request process" calledElement="process-a6d6ca00-cbb6-45d6-ae24-50ef53d37cc4">
	<bpmn2:incoming>SequenceFlow_8</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_9</bpmn2:outgoing>
</bpmn2:callActivity>
```

**Note**: Call activities can only be used to start a process instance of a process definition that exists in the same application as the process that is calling it.
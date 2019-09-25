---
Title: Message events
---

# Message events
Messages have a name and contain a payload. 

Each property in the message payload can be mapped to an individual [process variable](../../modeling-processes/README.md#process-variables)


The following are message events:

* [Message intermediate catch events](#message-intermediate-catch-events)
* [Message start events](../processes-bpmn/bpmn-start.md#message-start-events)
* [Message boundary events](../processes-bpmn/bpmn-boundary.md#message-boundary-events)

## Message intermediate catch events
Message intermediate catching events cause the process flow to wait until the message named in the `messageRef` property is received before it proceeds. 

When used in the Modeling Application, a previously created `Message` can be selected from the dropdown in its properties, or a new one created using the `+` symbol.

Message intermediate catching events are graphically represented by two thin concentric circles with a hollow envelope icon inside. 

The XML representation of message intermediate catching events is:

```xml
<bpmn2:intermediateCatchEvent id="IntermediateCatchEvent2">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
    <bpmn2:messageEventDefinition messageRef="Message_6" />
</bpmn2:intermediateCatchEvent>
```
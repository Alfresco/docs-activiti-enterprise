---
Title: Message events
---

# Message events



The following are message events:

* [Message intermediate throw events](#message-intermediate-throw-events)
* [Message intermediate catch events](#message-intermediate-catch-events)
* [Message start events](../processes-bpmn/bpmn-start.md#message-start-events)
* [Message end events](../processes-bpmn/bpmn-end.md#message-end-events)
* [Message boundary events](../processes-bpmn/bpmn-boundary.md#message-boundary-events)

## Message intermediate throw events
Message intermediate throwing events are 

Message intermediate throwing events are graphically represented by two thin concentric circles with a filled envelope icon inside.

The XML representation of message intermediate throwing events is:

## Message intermediate catch events
Message intermediate catching events cause the process flow to wait until the message named in the `signalRef` property is received before it proceeds. 

Message intermediate catching events are graphically represented by two thin concentric circles with a hollow envelope icon inside. 

The XML representation of message intermediate catching events is:

```xml
<bpmn2:intermediateCatchEvent id="IntermediateCatchEvent2">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
    <bpmn2:messageEventDefinition signalRef="Message_6" />
</bpmn2:intermediateCatchEvent>
```
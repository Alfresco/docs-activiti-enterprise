---
Title: Message events
---

# Message events
Messages have a name and contain a payload. 

To create a new message in the Modeling Application, the `+` symbol is used against a message event. A `name` can be defined and an auto-generated `id` is created.

Message events are graphically represented by an envelope icon inside different shapes that differentiate between the event types. A solid envelope represents a throwing event, whilst a hollow envelope represents a catching event.

The XML representation of a message is the following:

```xml
<bpmn2:message id="Message_0k9hibo" name="payment-message" />
```

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
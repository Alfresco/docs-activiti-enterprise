---
Title: Message events
---

# Message events
Messages have a name and contain a payload. They are sent by message throwing events and received by message catching events in a 1:1 relationship between catch events and throw events.  

To create a new message in the Modeling Application, the `+` symbol is used against a message event. A `name` can be defined and an auto-generated `id` is created. The message `id` is matched against the `messageRef` property in the corresponding throw and catch message elements.

The XML representation of a message is the following:

```xml
<bpmn2:message id="Message_0k9hibo" name="payment-message" />
```

Message payloads can be configured in the Modeling Application for throwing events 





stored in the [`<process-name>-extensions.json`](../../projects.md#files) file in a process 

The following is an example of a message payload for a [message end event](../bpmn/end.md#message-end-events) with the payload containing the [process variable](../README.md#process-variables) `name` and an `order-number`:

```json
    "mappings": {
        "EndEvent_0ss2fp3": {
            "inputs": {
                "name": {
                    "type": "variable",
                    "value": "name"
                },
                "order-number": {
                    "type": "value",
                    "value": 1459283
                }
            }
        }
    },
    "properties": {
        "426ea9f7-7049-4a4c-b235-960144b483de": {
            "id": "426ea9f7-7049-4a4c-b235-960144b483de",
            "name": "name",
            "type": "string",
            "value": "",
            "required": false
        }
    }
```

Message events are graphically represented by an envelope icon inside different shapes that differentiate between the event types. A solid envelope represents a throwing event, whilst a hollow envelope represents a catching event.

The following are message events:

* [Message end events](../bpmn/end.md#message-end-events)
* [Message intermediate catch events](#message-intermediate-catch-events)
* [Message intermediate throw events](#message-intermediate-throw-events)
* [Message start events](../bpmn/start.md#message-start-events)
* [Message boundary events](../bpmn/boundary.md#message-boundary-events)

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

## Message intermediate throw events 
Message intermediate throw events send the message event named in the `messageRef` property when the process flow reaches them. 

When used in the Modeling Application, a previously created `Message` can be selected from the dropdown in its properties, or a new one created using the `+` symbol

Message intermediate throwing events are graphically represented by two thin concentric circles with a solid envelope icon inside. 

The XML representation of message intermediate throwing events is:

```xml
<bpmn2:intermediateThrowEvent id="IntermediateThrowEvent1">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
    <bpmn2:messageEventDefinition messageRef="Message_6" />
</bpmn2:intermediateThrowEvent>
```
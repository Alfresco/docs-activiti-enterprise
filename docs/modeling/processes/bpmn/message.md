---
Title: Message events
---

# Message events
Messages have a name and contain a payload. They are sent by message throwing events and received by message catching events in a 1:1 relationship between throw events and catch events.  

To create a new message in the Modeling Application, use the `+` symbol against a message event. A `name` can be defined and an auto-generated `id` is created. The message `id` is matched against the `messageRef` property in the corresponding throw and catch message elements.

Messages contain a payload known as a [message payload](#message-payloads).

Message events are graphically represented by an envelope icon inside different shapes that differentiate between the event types. A solid envelope represents a throwing event, whilst a hollow envelope represents a catching event.

The XML representation of a message is the following:

```xml
<bpmn2:message id="Message_0k9hibo" name="payment-message" />
```

The following are message events:

* [Message end events](../bpmn/end.md#message-end-events)
* [Message intermediate catch events](#message-intermediate-catch-events)
* [Message intermediate throw events](#message-intermediate-throw-events)
* [Message start events](../bpmn/start.md#message-start-events)
* [Message boundary events](../bpmn/boundary.md#message-boundary-events)

## Message payloads
Message payloads contain a set of values that are sent from a throwing event and received by a catching event. 

Message payloads are created with message throw events and contain one or more properties that have a `name`, `type` and `value`. The following are the property types available to use in the payload: 

* String
* Integer
* Date
* Boolean
* [Process variable](../README.md#process-variables)

The receiving message catch event is then used to map the received values in the payload to process variables in its own scope. 

The message payload mappings are stored in the [`<process-name>-extensions.json`](../../projects.md#files) file in a process. Throwing events are mapped as `inputs` and catching events are mapped as `outputs`. 

The following is an example of a message payload for a [message end event](../bpmn/end.md#message-end-events) with the payload containing the process variable `username` and an integer of `order-number`:

```json
    "mappings": {
        "EndEvent_0ss2fp3": {
            "inputs": {
                "name": {
                    "type": "variable",
                    "value": "username"
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
            "name": "username",
            "type": "string",
            "value": "",
            "required": false
        }
    }
```

### Correlation keys
Message throwing events can optionally contain a correlation key. Correlation keys contain an expression that will be evaluated when the process flow reaches the message throwing event. If the result evaluates true, then the message is sent and if it evaluates to false then it is not sent. 

The expression must be written using Java Unified Expression Language (JUEL). 

In the following example, the message payload will be sent if `intVar` is equal to 5:

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_8</bpmn2:incoming>
	<bpmn2:messageEventDefinition messageRef="Message_1hxecs2" activiti:correlationKey="${{intVar} == 5}" />
```

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
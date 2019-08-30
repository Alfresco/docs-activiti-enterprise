---
Title: End events
---

# End events
End events indicate where process execution stops and the process completes. There can be no outgoing sequence flows from end events. 

The following are end events: 

* [End events](#end-events)
* [Error end events](#error-end-events)
* [Message end events](#message-end-events)
* [Terminate end events](#terminate-end-events)

## End events
End events complete the process flow with no additional behaviour. 

End events are graphically represented by a single thick circle without an icon inside. 

The XML representation of end events is an end event declaration without a sub-element declaring a specific type:

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
</bpmn2:endEvent>
```
## Error end events

## Message end events
Message end events complete the process flow and send a message event. See [message events](../processes-bpmn/bpmn-message.md) for more information about messages.

Message end events are graphically represented by a single thick circle with an envelope icon inside. 

The XML representation of a message end event is: 

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
	<bpmn2:messageEventDefinition id="" name="" />
</bpmn2:endEvent>
```

## Terminate end events

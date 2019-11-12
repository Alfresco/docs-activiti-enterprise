---
Title: End events
---

# End events
End events indicate where the current process flow ends. There can be no outgoing sequence flows from end events. Different types of end event can have have actions other than just ending the current process flow execution path. 

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
Error end events throw an error when the process flow reaches them. 

When used in the Modeling Application, a previously created `Error` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. 

See [error events](../bpmn/error.md) for more information regarding errors and how they can be thrown. 

Error end events are graphically represented by a single thick circle with a solid lightning bolt icon inside. 

The XML representation of an error end events is: 

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_8</bpmn2:incoming>
	<bpmn2:errorEventDefinition errorRef="Error_3vbkafg" />
</bpmn2:endEvent>
```

The XML representation of an error is:

```xml
<bpmn2:error id="Error_3vbkafg" name="payment-failed-error" errorCode="404" />
```

## Message end events
Message end events complete the process flow and send a message event. 

When used in the Modeling Application, a previously created `Message` can be selected from the dropdown in its properties, or a new one created using the `+` symbol.

See [message events](../bpmn/message.md) for more information about messages and how they can be generated.

Message end events are graphically represented by a single thick circle with a solid envelope icon inside. 

The XML representation of a message end event is: 

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
	<bpmn2:messageEventDefinition messageRef="Message_45sdihj" />
</bpmn2:endEvent>
```

## Terminate end events
Terminate end events cause the current process scope to be immediately ended, including any parallel process flows. The scope is determined by the location of the terminate end event. If the terminate end event is in a [sub-process](../bpmn/sub.md) or [call activity](../bpmn/call.md), only the sub-process or call activity instance will be ended, not the parent or originating process instance. In the case of a multi-instance sub-process or call activity only a single instance will be ended. 

The property `activiti:terminateAll` can be added to terminate end event definitions. When this property is set to `true` the root process instance will be terminated when a terminate end event is reached, regardless of where the event is placed.  

Terminate end events are graphically represented by a single thick circle with a solid circle inside.

The XML representation of a terminate end event with the `activiti:terminateAll` property is: 

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
	<bpmn2:terminateEventDefinition activiti:terminateAll="true"/>
</bpmn2:endEvent>
```
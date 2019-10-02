---
Title: Boundary events
---

# Boundary events
Boundary events are assigned to other BPMN elements such as [service tasks](../processes-bpmn/bpmn-service.md) and [user tasks](../processes-bpmn/bpmn-user.md). They are used by dragging the selected boundary type onto the BPMN element to influence and using the spanner icon to select the type of boundary event to use. 

Whilst the element that the boundary event is attached to is being executed within a process instance, the boundary event is waiting for its trigger event. Once that event occurs the behaviour can follow one of two paths: 

* Interrupting behaviour where the element's execution is terminated by the boundary event and the sequence flow out of the boundary event is followed. Interrupting boundary events are graphically represented by solid lines.

* Non-interrupting behaviour where the element's execution continues and a new sequence flow is followed from the boundary event in parallel to the main sequence flow. Non-interrupting boundary events are graphically represented by dashed lines.

**Note**: Depending on the boundary type, a trigger may never reach the attached boundary event. For example a signal may not be thrown for a signal boundary event to catch. 

Boundary events use the `attachedToRef` property to indicate the `id` of the element they are attached to. Interrupting behaviour is the default for boundary events. Non-interrupting events contain the `cancelActivity=false` property. 

The following are boundary events:

* [Error boundary events](#error-boundary-events)
* [Message boundary events](#message-boundary-events)
* [Signal boundary events](#signal-boundary-events)
* [Timer boundary events](#timer-boundary-events)

## Error boundary events
Error boundary events catch error events on the boundary of another element. Error boundary events are always interrupting, so as soon as an error is caught all process execution within the element they are attached to ceases. 

When used in the Modeling Application, a previously created `Error` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. 

See [error events](../processes-bpmn/bpmn-error.md) for more information regarding error events and how they can be thrown.

Error boundary events are graphically represented by two thin concentric circles with a hollow lightning bolt icon inside attached to the border of another BPMN element. 

The XML representation of an error boundary event is: 

```xml
<bpmn2:boundaryEvent id="BoundaryEvent2" attachedToRef="ServiceTask1">
	<bpmn2:errorEventDefinition errorRef="Error_0vbkbeb" />
</bpmn2:boundaryEvent>
```

The XML representation of an error is:

```xml
<bpmn2:error id="Error_0vbkbeb" name="payment-failed-error" errorCode="404" />
```

## Message boundary events
Message boundary events are attached to the boundary of another element. When a named message is received by the message boundary event, the process flow will be interrupted or a concurrent flow will be created depending on whether the event is interrupting or non-interrupting.

When used in the Modeling Application, a previously created `Message` can be selected from the dropdown in its properties, or a new one created using the `+` symbol.

See [message events](../processes-bpmn/bpmn-message.md) for more information about messages and how they can be generated.

Message boundary events are graphically represented by two thin concentric circles, or two thin dashed concentric circles, with a hollow envelope icon inside attached to the border of another BPMN element. 

The XML representation of an interrupting message boundary event is:

```xml
<bpmn2:boundaryEvent id="BoundaryEvent1" attachedToRef="UserTask2">
	<bpmn2:outgoing>SequenceFlow5</bpmn2:outgoing>
	<bpmn2:messageEventDefinition messageRef="Message_15xakkk" />
</bpmn2:boundaryEvent>
```
The XML representation of a non-interrupting message boundary event is: 

```xml
<bpmn2:boundaryEvent id="BoundaryEvent3" cancelActivity="false" attachedToRef="SubProcess2">
	<bpmn2:outgoing>SequenceFlow8</bpmn2:outgoing>
	<bpmn2:messageEventDefinition messageRef="Message_02satcd" />
</bpmn2:boundaryEvent>
```

The XML representation of a message is: 

```xml
<bpmn2:message id="Message_02satcd" name="Message_02satcd" />
```

## Signal boundary events
Signal boundary events can be considered catching events as they always wait to receive a named signal from a throwing event. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

See [signal events](../processes-bpmn/bpmn-signal.md) for more information regarding signals.

Signal boundary events are graphically represented by two thin concentric circles with a hollow triangle icon inside attached to the border of another BPMN element. 

The XML representation of a signal boundary event is: 

```xml
<bpmn2:boundaryEvent id="BoundaryEvent1" attachedToRef="ServiceTask3">
      <bpmn2:signalEventDefinition signalRef="Signal_0iikg75" />
</bpmn2:boundaryEvent>
```

The XML representation of a signal with a global scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" />
```

The XML representation of a signal with a process instance scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" activiti:scope="processInstance" />
```

## Timer boundary events
Timer boundary events can be interrupting or non-interrupting. They wait for a specified time before triggering and can also be set to trigger at multiple intervals. 

See [timer events](../processes-bpmn/bpmn-timer.md) for more information regarding timers. 

Timer boundary events are graphically represented by two thin concentric circles, or two thin dashed concentric circles, with a clock icon inside attached to the border of another BPMN element. 

The XML representation of an interrupting timer boundary event is:

```xml
<bpmn2:boundaryEvent id="BoundaryEvent3" attachedToRef="UserTask1">
	<bpmn2:outgoing>SequenceFlow5</bpmn2:outgoing>
	<bpmn2:timerEventDefinition>
		<bpmn2:timeDuration xsi:type="bpmn2:tFormalExpression">PT10M</bpmn2:timeDuration>
	</bpmn2:timerEventDefinition>
</bpmn2:boundaryEvent>
```

The XML representation of a non-interrupting timer boundary event is: 

```xml
<bpmn2:boundaryEvent id="BoundaryEvent4" cancelActivity="false" attachedToRef="SubProcess1">
	<bpmn2:outgoing>SequenceFlow8</bpmn2:outgoing>
	<bpmn2:timerEventDefinition>
		<bpmn2:timeDuration xsi:type="bpmn2:tFormalExpression">P5D</bpmn2:timeDuration>
	</bpmn2:timerEventDefinition>
</bpmn2:boundaryEvent>
```

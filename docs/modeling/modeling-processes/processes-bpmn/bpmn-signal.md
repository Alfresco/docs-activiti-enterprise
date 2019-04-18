---
Title: Signal events
---

# Signal events
Signal events can be either catching or throwing. A throwing signal event will emit a signal when it is reached in a process instance that will be picked up by any catching signal event with a matching signal name.  

Signal events are graphically represented by a triangle icon inside different shapes that differentiate between the event types. A solid triangle represents a throwing event, whilst a hollow triangle represents a catching event.

The following are signal events:

* [Signal intermediate throw events](#signal-intermediate-throw-events)
* [Signal intermediate catch events](#signal-intermediate-catch-events)
* Also see [start signal events](../processes-bpmn/bpmn-start.md#start-signal-events)

## Signal intermediate throw events
Signal intermediate throwing events are events that emit a signal when they are reached in the process flow. The signal that is emitted is then caught by any catching signal events with a name matching the signal that was thrown. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

Signal intermediate throwing events are graphically represented by two thin concentric circles with a solid triangle icon inside.

The XML representation of signal intermediate throwing events is: 

```xml
<bpmn2:intermediateThrowEvent id="IntermediateThrowEvent1">
	<bpmn2:incoming>SequenceFlow_3</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_4</bpmn2:outgoing>
   <bpmn2:signalEventDefinition signalRef="Signal_1jiw9tp" />
</bpmn2:intermediateThrowEvent>
```

The XML representation of a signal with a global scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" />
```

The XML representation of a signal with a process instance scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" activiti:scope="processInstance" />
```

## Signal intermediate catch events
Signal intermediate catching events cause the process flow to wait until the signal named in the `signalRef` property is received before it proceeds. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

Signal intermediate catching events are graphically represented by two thin concentric circles with a hollow triangle icon inside. 

The XML representation of signal intermediate catching events is:
	
```xml
<bpmn2:intermediateCatchEvent id="IntermediateThrowEvent2">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
    <bpmn2:signalEventDefinition signalRef="Signal_0hnsd2r" />
</bpmn2:intermediateCatchEvent>
```
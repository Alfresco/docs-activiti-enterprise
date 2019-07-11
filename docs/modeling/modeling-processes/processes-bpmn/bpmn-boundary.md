---
Title: Boundary events
---

# Boundary events
Boundary events are assigned to other BPMN elements such as [service tasks](../processes-bpmn/bpmn-service.md) and [user tasks](../processes-bpmn/bpmn-user.md). They are used by dragging the selected boundary type onto the BPMN element to influence and using the spanner icon to select the type of boundary event to use. 

Whilst the element that the boundary event is attached to is being executed within a process instance, the boundary event is waiting for its trigger event. Once that event occurs the behaviour can follow one of two paths: 

* Interrupting behaviour where the element's execution is terminated by the boundary event and the sequence flow out of the element is followed. Interrupting boundary events are graphically represented by solid lines.

* Non-interrupting behaviour where the element's execution continues and a new sequence flow is followed from the boundary event in parallel to the main sequence flow. Non-interrupting boundary events are graphically represented by dashed lines.

**Note**: Depending on the boundary type, a trigger may never reach the attached boundary event. For example a signal may not be thrown for a signal boundary event to catch. 

Boundary events use the `attachedToRef` property to indicate the `id` of the element they are attached to. 

The following are boundary events:

* [Signal boundary events](#signal-boundary-events)
* [Timer boundary events](#timer-boundary-events)

## Signal boundary events
Signal boundary events are always interrupting. They can be considered catching events as they always wait to receive a named signal from a throwing event. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

See [signal events](../processes-bpmn/bpmn-signal.md) for more information regarding signals.

Signal boundary events are graphically represented by two thin concentric circles with a hollow triangle icon inside attached to the border of another BPMN element. 

The XML representation of a signal boundary event is: 

```xml
<bpmn2:boundaryEvent id="BoundaryEvent1" attachedToRef="ServiceTask_0fr5st4">
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

Timer boundary events are graphically represented by two thin concentric circles, or two thin dotted concentric circles, with a clock icon inside attached to the border of another BPMN element. 

The XML representation of an interrupting timer boundary event is:

```xml

```

The XML representation of a non-interrupting timer boundary event is: 

```xml

```
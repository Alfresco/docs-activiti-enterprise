---
Title: Boundary events
---

# Boundary events
Boundary events are assigned to a [service task](../processes-bpmn/bpmn-service.md). They are used by dragging the relevant event type onto the service task and using the spanner icon to select the type of boundary event to use. 

Whilst the service task that the boundary event is assigned to is being executed within a process instance, the boundary event is listening for its trigger event. If the event is caught the behaviour can follow one of two paths:

* Interrupting behaviour where the service task execution is terminated and the sequence flow out of the service task is followed. Interrupting boundary events are graphically represented by solid lines.
* Non-interrupting behaviour where the service task execution continues and a new sequence flow of is followed from the boundary event in parallel. Non-interrupting boundary events are graphically represented by dashed lines.

Boundary events use the `attachedToRef` property to indicate the `id` of the task they are attached to. 

The following are boundary events:

* [Boundary signal events](#boundary-signal-events)

## Boundary signal events
Boundary signal events are always interrupting. They can be considered catching events as they always wait to receive a named signal from a throwing event. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

See [signal events](../processes-bpmn/bpmn-signal.md) for more information regarding signals.

Boundary signal events are graphically represented by two thin concentric circles with a hollow triangle icon inside attached to the border of a service task. 

The XML representation of a boundary signal event is: 

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
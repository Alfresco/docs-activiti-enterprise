---
Title: Start events
---

# Start events
A process must always contain at least one start event as they define how a process starts.

The following are start events:

* [Start events](#start-events)
* [Error start events](#error-start-events)
* [Message start events](#message-start-events)
* [Signal start events](#signal-start-events)
* [Timer start events](#timer-start-events)

## Start events
Start events are where the trigger is unspecified for starting a process. 

Start events can have a form associated with them to begin the process, though it is important to note that the process will not begin until the form has been submitted. Alternatively, processes with a start event can be started manually using a [REST API](../../../apis/README.md) or through the [Process Workspace](../../../workspace/workspace-processes.md). 

Start events are graphically represented by a single thin circle without an icon inside.

The XML representation of a start event without a form defined is:

```xml
<bpmn2:startEvent id="StartProcess_1" name="FormStart_4">
	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
</bpmn2:startEvent>
```
The XML representation of a start event with a form defined is:

```xml
<bpmn2:startEvent id="StartProcess_1" name="FormStart_4" activiti:formKey="form-4ccd023b-d607-4cab-8623-da4c87dd9611">
	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
</bpmn2:startEvent>
```

**Note**: The `activiti:formKey` is the `id` of the form used to start the process. This can be seen in the JSON of the form definition. 

## Error start events


## Message start events
Message start events begin a process instance when a named message is received. See [message events](../processes-bpmn/bpmn-message.md) for more information regarding messages and how they can be generated. 

Message start events are graphically represented by a single thin circle with a hollow envelope icon inside.

The XML representation of a message start event is:

```xml
<bpmn2:startEvent id="StartEvent2">
	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
 	<bpmn2:messageEventDefinition id="" name="" />
</bpmn2:startEvent>
```

## Signal start events
Signal start events begin a process instance using a caught, named signal. See [signal events](../processes-bpmn/bpmn-signal.md) for more information regarding signals and how they can be thrown. 

When used in the Modeling Application, a previously used `Signal` can be selected from the dropdown in its properties, or a new one created using the `+` symbol. Signals can be restricted to the process instance they are thrown in, or be global in scope. The scope of a global signal is restricted to the application they are used in. 

Signal start events are graphically represented by a single thin circle with a hollow triangle icon inside. 

The XML representation of a signal start event is:

```xml
<bpmn2:startEvent id="StartEvent1">
	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
 	<bpmn2:signalEventDefinition signalRef="Signal_0hnsd2r" />
</bpmn2:startEvent>
```

The XML representation of a signal with a global scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" />
```

The XML representation of a signal with a process instance scope is:

```xml
<bpmn2:signal id="Signal_0hnsd2r" name="Signal_0hnsd2r" activiti:scope="processInstance" />
```

## Timer start events
Timer start events begin a process at a specific time once, or repeatedly at intervals. See [timer events](../processes-bpmn/bpmn-timer.md) for more information regarding timers and the different ways to set them. 

Timer start events are graphically represented by a single thin circle with a clock icon inside. 

The XML representation of a timer start event is: 

```xml
<bpmn2:startEvent id="StartEvent3">
 	<bpmn2:outgoing>SequenceFlow_1</bpmn2:outgoing>
	<timerEventDefinition>
		<timeCycle xsi:type="bpmn2:tFormalExpression">R10/2020-12-10T13:00/PT12H</timeCycle>
	</timerEventDefinition>
</bpmn2:startEvent>
```

**Note**: This will start the process 10 times, at 12 hour intervals starting on the 10th December 2020. 
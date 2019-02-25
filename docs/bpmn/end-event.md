# End events
End events indicate where process execution stops and the process completes. 

End events are graphically represented by a single thick circle without an icon inside. 

The XML representation of end events is an end event declaration without a sub-element declaring a specific type:

```xml
<bpmn2:endEvent id="EndEvent_1">
	<bpmn2:incoming>SequenceFlow_1</bpmn2:incoming>
</bpmn2:endEvent>
```
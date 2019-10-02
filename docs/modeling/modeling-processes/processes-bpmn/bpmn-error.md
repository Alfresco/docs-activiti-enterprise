---
Title: Error events
---

# Error events
Error events are used to model an exception in a business process. Errors are thrown by error throwing events such as error end events and caught by catching error events such as error start events. 

To create a new error in the Modeling Application, the `+` symbol is used against an error event. A `name` and `errorCode` are defined and an auto-generated `id` is created.

The `errorRef` property in the `errorEventDefinition` of an error event will match against the `id` of an error defined in a process.  

Error events are graphically represented by a lightning bolt icon inside different shapes that differentiate between the event types. A solid lightning bolt represents a throwing event, whilst a hollow lightning bolt represents a catching event.

The XML representation of an error is the following:

```xml
<bpmn2:error id="Error_04mep1k" name="payment-error" errorCode="404" />
```

The following are error events: 

* [Error start events](../processes-bpmn/bpmn-start.md#error-start-events)
* [Error end events](../processes-bpmn/bpmn-end.md#error-end-events)
* [Error boundary events](../processes-bpmn/bpmn-boundary.md#error-boundary-events)
---
Title: Service tasks
---

# Service tasks

Service tasks are used in conjunction with connectors to allow two-way communication between external systems and a process. 

The implementation property value of a service task matches with a connector name and the appropriate action(s) defined in the connector can be chosen from a dropdown. The implementation value within the XML of the process definition will be the name of the relevant connector. Actions within the Modeling Application will automatically match to the corresponding process variable(s) if their names and data types match.

Service tasks are graphically represented by a single, thin rounded rectangle with a cog icon inside. 

The XML representation of a service task is: 

```xml
<bpmn2:serviceTask id="Task_4" name="Update Approvals" implementation="updateApprovalConnector">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
</bpmn2:serviceTask>
```

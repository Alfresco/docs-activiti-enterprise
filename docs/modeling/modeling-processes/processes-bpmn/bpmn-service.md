---
Title: Service tasks
---

# Service tasks
Service tasks are used in conjunction with connectors to allow two-way communication between external systems and a process. 

The `implementation` property value of a service task matches with a connector name and the appropriate action defined in the connector definition and can be chosen from a dropdown. The `implementation` value within the XML of the process definition will be in the format `<connector-name>.<action-name>`. The input and output parameters of the connector action will automatically match to corresponding process variables if their names and data types match. Alternatively, parameters can be entered as values directly into the service task. 

Service tasks are graphically represented by a single, thin rounded rectangle with a cog icon inside. 

<img align="left" width="100" height="100" src="../../../images/service-task.svg">

The XML representation of a service task is: 

```xml
<bpmn2:serviceTask id="Task_4" name="Update Approvals" implementation="updateApprovalConnector">
	<bpmn2:incoming>SequenceFlow_5</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_6</bpmn2:outgoing>
</bpmn2:serviceTask>
```

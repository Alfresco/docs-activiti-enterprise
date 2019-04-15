---
Title: Twilio connector
---

# Twilio connector
The Twilio connector is used to integrate with an instance of [Twilio](https://twilio.com) to send SMS messages. The Twilio connector is graphically represented by the Twilio logo under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the email connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1che7zm" implementation="emailConnector.SEND" />
```

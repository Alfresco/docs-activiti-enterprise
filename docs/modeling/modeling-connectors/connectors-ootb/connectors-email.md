---
Title: Email connector
---

# Email connector
The email connector is used to send emails using the SMTP protocol as part of a process instance. The email connector is graphically represented by an envelope under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the email connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1che7zm" implementation="emailConnector.SEND" />
```

## Input parameters
The following are the parameters that can be passed to the email connector as input parameters using the `SEND` function. 

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| to | An email address or list of addresses | `String` |
| from  | An email address | `String` |
| cc | An email address or list of email addresses | `String`| 
| bcc | An email address or array of addresses | `String` |
| subject | A plain text subject | `String` | 
| text | A plain text email body | `String` |
| html | An HTML email body | `String` |
| charset | Define the *charset* of the email. | `String` | 


## Output parameters
The following is the parameter that is returned to the process by the email connector as an output parameter using the `SEND` function.

**Note**: The execution of the email connector is always successful. Any errors will be returned in the `email.error` parameter. The parameter will be null if there were no errors.

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| email.error | A list of errors if any are caught by the connector | `String` |





## Configuration / Environment variables? 

For the email connector to function certain properties must be defined in the `application.properties` file of the Spring Boot application. By default, these properties are set to environment variables.

The connector requires an SMTP server to be configured, the connector uses the org.springframework.mail package to manage communication to a mail server. As such `spring.mail.properties.*` can be used to configure the desired email server.

The following are a set of properties that would be defined for a Secure (TLS) connection:

```
spring.mail.host=${EMAIL_HOST}
spring.mail.port=${EMAIL_PORT}
spring.mail.username=${EMAIL_USERNAME}
spring.mail.password=${EMAIL_PASSWORD}
spring.mail.properties.mail.smtp.auth=${EMAIL_SMTP_AUTH:true}
spring.mail.properties.mail.smtp.starttls.enable=${EMAIL_SMTP_STARTTLS:true}
```

As the connector uses a stream mechanism to send/receive information between APS and the connector, the following property is used to identify the connector:

```
spring.cloud.stream.bindings.emailConnectorConsumer.destination=emailConnector.SEND
```

The name of the channel requires to match the implementation value defined in the Service Task as part of the BPMN definition.



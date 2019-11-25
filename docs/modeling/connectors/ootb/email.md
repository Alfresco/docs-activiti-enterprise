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
The following are the parameters that can be passed to the email connector as input parameters using the `SEND` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `to` | An email address or list of addresses | String | Yes |
| `from`  | An email address | String | Yes |
| `cc` | An email address or list of email addresses | String | No |
| `bcc` | An email address or array of addresses | String | No |
| `subject` | A plain text subject | String | No |
| `text` | A plain text email body | String | No |
| `html` | An HTML email body | String | No |
| `charset` | Define the *charset* of the email | String | No | 

## Output parameter
The following is the parameter that is returned to the process by the email connector as an output parameter using the `SEND` action:

**Note**: The execution of the email connector is always successful. Any errors will be returned in the `email.error` parameter. The parameter will be null if there were no errors.

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `email.error` | A list of errors if any are caught by the connector | String |

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

For the email connector the connector variables are the details for the SMTP host. The email connector uses the `org.springframework.mail` package for managing communication to the email server. This allows `spring.mail.properties*` to be set to configure the desired email server. 

The following are the properties that need to be set for a secure (TLS) connection:

| Variable | Description |
| -------- | ----------- |
| `EMAIL_HOST` | The host address of the SMTP server |
| `EMAIL_PORT` | The port the SMTP server is running on |
| `EMAIL_USERNAME` | The username the connector will use to contact the SMTP server |
| `EMAIL_PASSWORD` | The password of the user the connector will use to contact the SMTP server |
| `EMAIL_SMTP_AUTH` | This is a `boolean` value that sets whether the connection to the SMTP server requires authentication |
| `EMAIL_SMTP_STARTTLS` | This is a `boolean` value that sets whether the connection uses TLS |

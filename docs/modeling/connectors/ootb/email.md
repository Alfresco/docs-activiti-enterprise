---
Title: Email connector
---

# Email connector
The email connector is used to send emails using the SMTP protocol as part of a process instance. The email connector is graphically represented by an envelope under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the email connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1che7zm" implementation="emailConnector.SEND" />
```

## Actions
The email connector contains an action called `SEND` that sends an email using an external SMTP host. 

### Input parameters
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
| `nodeId` | The node ID of the file to attach to the email | String | No |
| `uri` | The URI of the file to attach to the email | String | No |
| `files` | A [file](../../files.md) uploaded in a process and set as a process variable or uploaded as part of a form or another connector to attach to the email | File | No |

### Output parameters
The following is the parameter that is returned to the process by the email connector as an output parameter using the `SEND` action:

**Note**: The execution of the email connector is always successful. Any errors will be returned in the `email.error` parameter. The parameter will be null if there were no errors.

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `email.error` | A list of errors if any are caught by the connector | String |

## Events
The email connector contains an event called `EMAIL_RECEIVED` that can be used by a [trigger](../../triggers.md) to configure an action when an email subject line meets a specific pattern.   

### Input parameters

| Parameter | Description | Example | Required? |
| --------  | ----------- | ------- | --------- |
| `pattern` | A regular expression that selects which emails are published as events. The syntax `(?<variable>)` can be used to create  | `Order Number (?<orderNumber>.+)` | Yes | 
| `echo` | The message sent to the original sender if a message is matched | `Your reference number is ${orderNumber}` | No | 
| `echoError` | The message sent to the user if an error occurs publishing the event | There was a problem publishing that event. | No | 

**Note**: Match groups created in a `pattern` can be referenced in `echo` and `echoError` using the syntax `${matchGroup}`.

### Output parameters

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `matchGroups` | Any match groups created in the regular expression `pattern` will be returned as a JSON map  | JSON | No |
| `emailSubject` | The subject of the matched email | String | No |
| `emailTo` | The recipient of the matched email | String | No |
| `emailFrom` | The sender of the matched email | String | No |
| `emailBody` | The message body of the matched email | String | No |

**Note**: The match groups can be used to map to process variables in a [trigger](../../../modeling/triggers.md) by referencing the variable in a JSON field, for example using `${matchGroups.orderNumber}`

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

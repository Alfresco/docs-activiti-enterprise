---
Title: Lambda connector
---

# Lambda connector
The Lambda connector is used to invoke [Amazon Web Services (AWS) Lambda functions](https://aws.amazon.com/lambda/). The Lambda connector is graphically represented by the AWS Lambda logo under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the Lambda connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_6thy7kl" implementation="lambdaConnector.INVOKE" />
```

## Input parameters
The following are the parameters that can be passed to the email connector as input parameters using the `INVOKE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `function` | The name of the Lambda function to invoke | `String` | Yes |
| `payload` | The payload that will be passed to the Lambda function | `String` or `Object` | No |

## Output parameters
The following are the parameters that are returned to the process by the Lambda connector as output parameters using the `INVOKE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `lambdaPayload` | The Lambda function result payload | `String` |
| `lambdaStatus` | The Lambda function invocation status code | `Integer` |
| `lambdaLog` | The log produced during the function invocation | `String` |
| `lambdaError` | A list of errors if any are caught by the connector or the Lambda function | `String` |

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Lambda connector: 

| Variable | Description |
| -------- | ----------- |
| `AWS_LAMBDA_AWS_ACCESS_KEY` | The access key to be used to authenticate against AWS |
| `AWS_LAMBDA_AWS_SECRET_KEY` | The secret key to be used to authenticate against AWS |
| `AWS_LAMBDA_AWS_REGION` | The region of AWS to invoke the Lambda functions in |
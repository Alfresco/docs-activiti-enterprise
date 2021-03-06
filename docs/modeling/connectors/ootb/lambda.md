---
Title: Lambda connector
---

# Lambda connector
The Lambda connector is used to invoke [Amazon Web Services (AWS) Lambda functions](https://aws.amazon.com/lambda/). The Lambda connector is graphically represented by the AWS Lambda logo under the OOTB Connectors menu whilst modeling a process. 

**Important**: The Lambda connector requires an AWS account to access Amazon features. 

The `implementation` value of the Lambda connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_6thy7kl" implementation="lambdaConnector.INVOKE" />
```

## Input parameters
The following are the parameters that can be passed to the Lambda connector as input parameters using the `INVOKE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `function` | The name of the Lambda function to invoke | String | Yes |
| `payload` | The payload that will be passed to the Lambda function | String or Object | No |

## Output parameters
The following are the parameters that are returned to the process by the Lambda connector as output parameters using the `INVOKE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `lambdaPayload` | The Lambda function result payload | String |
| `lambdaStatus` | The Lambda function invocation status code | Integer |
| `lambdaLog` | The log produced during the function invocation | String |
| `lambdaError` | A list of errors if any are caught by the connector or the Lambda function | String |

## Configuration parameters
Values for configuration parameters that are specific to a connector instance can be set in the modeling application or during application deployment.

The following are the configuration parameters that need to be set for the Lambda connector: 

| Parameter | Description | Required? |
| --------- | ----------- | --------- |
| `AWS_LAMBDA_AWS_ACCESS_KEY` | The access key to be used to authenticate against AWS | Yes |
| `AWS_LAMBDA_AWS_SECRET_KEY` | The secret key to be used to authenticate against AWS | Yes |
| `AWS_LAMBDA_AWS_REGION` | The region of AWS to invoke the Lambda functions in | Yes | 
| `AWS_LAMBDA_LOG` | The level of logging for the action, for example `DEBUG`, `TRACE`, `ALL`. The default value is `INFO` | No |
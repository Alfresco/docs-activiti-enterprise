---
Title: Rekognition connector
---

# Rekognition connector
The Rekognition connector uses the [Amazon Rekognition service](https://aws.amazon.com/rekognition/) to identify and label the objects in `JPEG` and `PNG` files that are less than 15mb in size.

The `implementation` value of the Rekognition connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_2tfy1pr" implementation="rekognitionConnector.LABEL" />
```

## Amazon Web Services (AWS) configuration
The Amazon Rekognition API that is called is the [Detect Labels API](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectLabels.html). 

The Rekognition connector requires an AWS account to access Amazon features. It also requires an [Identity and Access Management (IAM)](https://aws.amazon.com/iam/) user to have the `rekognition:DetectLabels` permission. 

Files between 5mb and 15mb are uploaded to an Amazon S3 bucket before processing. The IAM user configured previously requires access to this bucket, as does the [Rekognition service itself](https://docs.aws.amazon.com/rekognition/latest/dg/access-control-overview.html).

## Input parameters
The following are the parameters that can be passed to the Rekognition connector as input parameters using the `LABEL` action:

| Parameter | Description | Type | Required? |
| --------- | ----------- | ---- | --------- | 
| `nodes` | 
| `confidenceLevel` | 
| `maxLabels` | The maximum number of labels returned by the service. The default value is `10` | Integer | No |
| `timeout` | 

## Output parameters
The following are the parameters that are returned to the process by the Rekognition connector as output parameters using the `LABEL` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `awsResult` | The result of the image analysis | Object |
| `aisResponse` |
| `labels` |
| `analysis.error` |

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Rekognition connector: 

| Variable | Description | Required? |
| -------- | ----------- | --------- |
| `AWS_ACCESS_KEY_ID` | The access key to be used to authenticate against AWS | Yes |
| `AWS_SECRET_KEY` | The secret key to be used to authenticate against AWS | Yes |
| `AWS_REGION` | The region of AWS to use the Textract service in | Yes | 
| `AWS_S3_BUCKET` | The name of the S3 bucket to use | Yes |
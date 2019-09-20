---
Title: Comprehend connector
---

# Comprehend connector
The Comprehend connector uses the [Amazon Comprehend service's](https://aws.amazon.com/comprehend/) natural language processing (NLP) to identify and analyze text from `UTF-8` plain text files.

The `implementation` value of the Comprehend connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_8tacy8mn" implementation="comprehendConnector.ENTITY" />
```

## Amazon Web Services (AWS) configuration
The following are the Amazon Comprehend APIs that are called using the connector:

* [Detect Dominant Language API](https://docs.aws.amazon.com/comprehend/latest/dg/API_DetectDominantLanguage.html)
* [Detect Entities API](https://docs.aws.amazon.com/comprehend/latest/dg/API_DetectEntities.html)
* [Batch Detect Entities API](https://docs.aws.amazon.com/comprehend/latest/dg/API_BatchDetectEntities.html)
* [Start Entities Detection Job API](https://docs.aws.amazon.com/comprehend/latest/dg/API_StartEntitiesDetectionJob.html)
* [Describe Entities Detection Job API](https://docs.aws.amazon.com/comprehend/latest/dg/API_DescribeEntitiesDetectionJob.html)



The Comprehend connector requires an AWS account to access Amazon features. It also requires an [Identity and Access Management (IAM)](https://aws.amazon.com/iam/) user to have the `textract:DetectDocumentText` and `textract:AnalyzeDocument` permissions. 

## Input parameters
The following are the parameters that can be passed to the Comprehend connector as input parameters using the `ENTITY` action:

| Parameter | Description | Type | Required? |
| --------- | ----------- | ---- | --------- | 
| `nodes` | 
| `text` | 
| `maxEntities` |  | | |
| `confidenceLevel` | 
| `timeout` |  | | |


## Output parameters
The following are the parameters that are returned to the process by the Comprehend connector as output parameters using the `ENTITY` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `analysis.error` | A list of errors if any are caught by the connector | String |
| `awsResponse` | The text analysis response from AWS with no transformation applied | Object |
| `aisResponse` | The text analysis response in | Object | 

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Comprehend connector: 

| Variable | Description | Required? |
| -------- | ----------- | --------- |
| `AWS_ACCESS_KEY_ID` | The access key to be used to authenticate against AWS | Yes |
| `AWS_SECRET_KEY` | The secret key to be used to authenticate against AWS | Yes |
| `AWS_REGION` | The region of AWS to use the Comprehend service in | Yes | 
| `AWS_S3_BUCKET` | The name of the S3 bucket to use | Yes |
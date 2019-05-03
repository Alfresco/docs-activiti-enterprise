---
Title: Camel connector
---

# Camel connector
The camel connector is used to integrate with [Apache Camel]() for routing options as part of a process. The Camel connector is graphically represented by the picture of a camel under the OOTB connectors menu whilst modeling a process. 




## Input parameters
The following are the parameters that can be passed to the Camel connector as input parameters using the `INVOKE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `camelPayload` | The payload | `JSON object` | Yes |
| `camelRouteId` | | `String`| Yes | 


## Output parameters
The following are the parameters that are returned to the process by the Camel connector as output parameters using the `INVOKE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `camelResult` | The response from the connector | `JSON object` | 
| `camelError` | A list of errors if any are caught by the connector | `String` | 
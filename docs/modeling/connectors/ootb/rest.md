---
Title: REST connector
---

# REST connector
The REST connector is used to provide a connection with a REST service. The REST connector is graphically represented by a pair of curly brackets under the OOTB Connectors menu whilst modeling a process. 

The REST connector has the following actions that correspond to HTTP methods with the same name:

* `restConnector.GET`
* `restConnector.HEAD`
* `restConnector.POST`
* `restConnector.PUT`
* `restConnector.PATCH`
* `restConnector.DELETE`
* `restConnector.OPTIONS`
* `restConnector.TRACE`

An an example, the `implementation` value of the `GET` action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_5xed7dm" implementation="restConnector.GET" />
```

## Input parameters
The following are the parameters that can be passed to the REST connector as input parameters using any of the actions:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `restUrl` | URL of the REST endpoint including the protocol and path | String | Yes |
| `restUrlParams` | URL parameters to append to the URL | Map <String, String> | No |
| `restUrlEncoded` | Whether the URL should be encoded or not | Boolean | No |
| `requestPayload` | The HTTP body to be sent | JSON object | No |
| `requestHeaders` | A map of the request header names and values. Values can be fixed, variables or form fields | Map <String,String> | No |
| `circuitBreaker` | Any value entered will enable the circuit breaker | Boolean | No |
| `timeout` | The request timeout value in milliseconds | Integer | No |

**Note**: The parameters can accept variables as part of their values in the format `${variable}`. The variable names can be viewed and their values set using [connector variables](#connector-variables). For example: if the connector variable `REST_HOST` is set to `localhost` and the input parameter `restURL` is set to `http://{$host}/service` the REST service will resolve this to `http://localhost/service`.

## Output parameters
The following are the parameters that are returned to the process by the REST connector as output parameters using any of the actions:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `restResult` | The response sent back to the process from the REST service | JSON object |
| `restStatus` | The HTTP response status code if the request was performed | Integer | 
| `restError` | If an error was encountered it will be described in this parameter | String |

**Note**: The execution of the REST connector is always successful. Any errors will be returned in the `restError` parameter.

## Configuration parameters
Values for configuration parameters that are specific to a connector instance can be set in the modeling application or during application deployment.

The following are the configuration parameters that need to be set for the REST connector: 

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| `REST_HOST` | The host address of the REST service that can be used as the variable `${host}` |
| `REST_HTTP_PORT` | The port used for HTTP calls that can be used as the variable `${rest-http-port}` | 80 |
| `REST_HTTPS_PORT` | The port used for HTTPS calls that can be used as the variable `${rest-https-port}` | 443 |
| `REST_AUTH_TOKEN` | The token used by the REST service that can be used as the variable `${rest-auth-token}` | |
| `REST_AUTH_USER` | The user that the REST service uses that can be used as the variable `${rest-auth-user}` | |
| `REST_CIRCUIT_BREAKER_BUFFER` | The size of the ring buffer when the circuit breaker is closed | 5 |
| `REST_CIRCUIT_BREAKER_WAIT_INTERVAL` | The duration the circuit breaker should stay open before switching to half open | 1000 |
| `REST_CIRCUIT_BREAKER_FAILURE_RATE` | The percentage above which the circuit breaker should trip and start short circuiting calls | 20 |


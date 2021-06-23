---
Title: REST APIs
---

# REST APIs
The REST APIs for Activiti Enterprise are accessed differently depending on whether a service is application or platform specific. If a service is application specific then the *application name* will form part of the endpoint. 

## Platform endpoints
The OpenAPI specifications for platform endpoints are as follows:

* Modeling service: `gateway.{domain-name}/modeling-service/swagger-ui`
* Deployment service: `gateway.{domain-name}/deployment-service/swagger-ui`

## Application endpoints
The OpenAPI specifications for application endpoints require the `{application-name}` element in the URL as demonstrated in the following:

* Application runtime bundle: `gateway.{domain-name}/{application-name}/rb/swagger-ui`
* Application query service: `gateway.{domain-name}/{application-name}/query/swagger-ui`

### GraphQL
The query service can also use [GraphQL](https://graphql.org/learn/) to expand on querying. It can be accessed at the following URL: 

`gateway.{domain-name}/{application-name}/notifications/graphiql`. 


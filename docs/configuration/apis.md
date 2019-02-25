# APIs
APIs are accessed differently depending on which service and which application you are querying.

The application name forms part of the API URL:

* For process instance and task queries use the query service API. These APIs are are all read-only. The URL for the API documentation is:  

	`{protocol}://{my-domain}/{my-application-name}-query/swagger-ui.html`

* For audit events, the URL for the API documentation is: 

	`{protocol}://{my-domain}/{my-application-name}-audit/swagger-ui.html`

* To query and execute calls against processes and tasks in a runtime bundle, the URL for the API documentation is: 

	`{protocol}://{my-domain}/{my-application-name}-rb/swagger-ui.html`

For example, if you want to view all process instances in an application called *holiday-request*, on the domain *example.softwarecompany.com* you would use the following:

GET https://example.softwarecompany.com/holiday-request-rb/admin/v1/process-instances
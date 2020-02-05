---
Title: Realm file
---

# Realm file
A realm file is a JSON format file that describes the security metadata for clients, users, groups and roles. 

The realm for Activiti Enterprise can be customized with the exception of the following items:

* Roles
* Users
* Clients

## Roles
The roles that are included in the default Activiti Enterprise realm are required to manage access to specific user interfaces and APIs. The roles cannot be renamed or replaced. Information regarding [roles and the permissions they grant](../administrator/identity/README.md#roles) is also available. 

The following is the `ACTIVITI_ADMIN` role from the realm file: 

```json
"roles": {
    "realm": [
      {
        "name": "ACTIVITI_ADMIN",
        "composites": {
          "client": {
            "realm-management": [
              "view-users",
              "view-clients"
            ]
          }
        }
      },
```

## Users
There is one service account and one user in the Activiti Enterprise realm that are used for communication between services:

* The `storage-service` is used for storing task and process data in Alfresco Content Services. It is used by the [process storage service](../architecture/application.md#process-storage-service). 
* The user `client` is called by the [runtime bundle](../architecture/application.md#runtime-bundle) to query user and group permissions to complete jobs such as assigning tasks. 

The following is the `storage-service` service account from the realm file:

```json
    {
      "clientId": "storage-service",
      "enabled": true,
      "authorizationServicesEnabled": true,
      "directAccessGrantsEnabled": true,
      "secret": ""
    }
```

## Clients
Two clients are created under the realm `alfresco`. The `activiti` realm is used by Activiti Enterprise whilst the `alfresco` client is used by Alfresco Content Services. The clients can be renamed but the values and settings configured by the default realm file should be retained as these are set during deployment.

The following is the `activiti` client from the realm file:

```json
    {
	      "clientId": "activiti",
	      "enabled": true,
	      "publicClient": true,
	      "redirectUris": "*",
	      "webOrigins": "*",
	      "directAccessGrantsEnabled": true,
	      "implicitFlowEnabled": true
	    },
```

**Note**: `redirectUris` and `webOrigins` can be updated to list only valid URLs rather than using a wildcard `*`. 
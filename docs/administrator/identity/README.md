---
Title: Identity management
--- 

# Identity management
The Administrator Application is used to create and manage users and their assigned groups and roles. Users require the `ACTIVITI_IDENTITY` role to view the identity management section. 

The identity management section of the Administrator Application allows users with access to it to undertake common user maintenance tasks without having to access the [Identity Service](../identity/service.md) directly.

## Roles
The roles section displays the current roles available to choose from and allows them to be deleted or have their descriptions updated. It is also possible to create a new role.

The following roles are available with a default installation: 

| Role | Description |
| ---- | ----------- |
| `ACTIVITI_ADMIN` | Provides access to the [Administrator Application](../README.md). Users require this role to be given administrator access to an application. If a user has administrator access to an application they will be able to see the **Process Admin** functions meaning they can monitor process instances and tasks and view the audit log |
| `ACTIVITI_DEVOPS` | Provides access to the [Administrator Application](../README.md). Users with this role will be able to see the **DevOps** functions meaning they can deploy projects, create deployment descriptors and monitor applications |
| `ACTIVITI_IDENTITY` | Provides access to the [Administrator Application](../README.md). Users with this role will be able to see the **Identity** functions meaning they can manage users, groups and roles |
| `ACTIVITI_MODELER` | Provides access to the [Modeling Application](../../modeling/README.md). Users with this role will be able to model and release projects |
| `ACTIVITI_USER` | Users require this role to be given user access to an application. If a user has user access to an application they will be able to start processes and tasks and access the [Process Workspace](../../workspace/README.md) for that application if it is deployed.  | 

## Users
The users section displays the current list of users in the system. New users can be added in this section. 

Once a user has been created, the groups and roles that each are assigned can be managed by editing the user. It is also possible to reset a user's password in case they have forgotten it using this section. 

There are several users provided with the deployment that have various roles attached to them. You can use these for testing purposes by updating the passwords using the Administrator Application or create your own. 

The following are the default users:

| User | Assigned roles | 
| ---- | -------------- |
| modeler | `ACTIVITI_MODELER` |
| processadminuser | `ACTIVITI_ADMIN` |
| devopsuser | `ACTIVITI_DEVOPS` |
| hruser | `ACTIVITI_USER` |
| salesuser | `ACTIVITI_USER` |
| identityuser | `ACTIVITI_IDENTITY` |
| superadminuser | `ACTIVITI_ADMIN`, `ACTIVITI_DEVOPS`, `ACTIVITI_IDENTITY`, `ACTIVITI_MODELER` |

## Groups
The groups sections displays the current groups available to assign users to and allows them to be deleted or have their name changed. It is also possible to create a new group.

## Identity Service
Whilst the majority of user management can be handled within the Administrator Application, it may be necessary to access the [Identity Service](../identity/service.md) administration console directly for some tasks. 


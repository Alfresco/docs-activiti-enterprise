---
Title: Identity management
--- 

# Identity management
The Administrator Application is used to create and manage users and their assigned groups and roles. Users require the *APS_IDENTITY* role to view the identity management section. 

The identity management section of the Administrator Application allows users with access to it to undertake common user maintenance tasks without having to access the [Identity Service](../admin-identity/identity-service.md) directly.

## Users
The users section displays the current list of users in the system. New users can be added in this section. 

Once a user has been created, the groups and roles that each are assigned can be managed by editing the user. It is also possible to reset a user's password in case they have forgotten it using this section. 

There are several users provided with the deployment that have various roles attached to them. You can use these for testing purposes by updating the passwords using the Administrator Application or create your own. 

## Groups
The groups sections displays the current groups available to assign users to and allows them to be deleted or have their name changed. It is also possible to create a new group.

## Roles
The roles section displays the current roles available to choose from and allows them to be deleted or have their descriptions updated. It is also possible to create a new role.

The following following roles are available with a default installation: 

| Role | Description |
| ---- | ----------- |
| *APS_PROCESS_ADMIN* |Provides administrator access to all functions including access to admin APIs but can also act as a user of the applications and have tasks assigned to it, whereas APS_ADMIN cannot | 
| *APS_DEVOPS* | Provides access to the DevOps functions in the Administrator Application (application definitions and application instances) |
| *APS_ADMIN* | Provides administrator access to all functions including access to admin APIs |
| *APS_MODELER* | Provides access to the Modeling Application |
| *APS_USER* | Provides access to fill in forms and initiate processes. Will not have access to any admin APIs | 

## Identity Service
Whilst the majority of user management can be handled within the Administrator Application, it may be necessary to access the [Identity Service](administrator/admin-identity/identity-service.md) administration console directly for some tasks. 


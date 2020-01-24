---
Title: Identity Service
---

# Identity Service
The [Identity Service](https://docs.alfresco.com/identity/concepts/identity-overview.html) is used by Activiti Enterprise for authorization and to manage user and role management.

You can log into the administration console for the Identity Service by navigating to `identity.{domain-name}/auth/admin` and using the username **admin** and password of **admin**.

**Note**: it is strongly recommended that you update the password for the administration console when you first log into it. 

A default realm of **alfresco** is already setup with the installation of Alfresco Activiti Enterprise. 

## Improving API call performance 
It is possible to improve the performance of API calls to the runtime bundle, query service and audit service by including a user's group membership in the JSON Web Token (JWT). 

**Note**: Adding group membership to the JWT increases its size. It is advisable to add group membership only when there are not an excessive number of groups per user, otherwise the performance will be degraded rather than improved.

To include a list of user groups in the JWT that a user belongs to, make the following change to the client within the Identity Service:

1. Sign into the administration console for the Identity Service: `identity.{domain-name}/auth/admin`.
2. Navigate to the **Clients** section of the realm used by Activiti Enterprise.
3. Select the client to update and navigate to the **Mappers** tab. 
4. Create a new protocol mapper and set the following at a minimum:
	* The **Mapper Type** to **Group Membership**
	* Set **Token Claim Name** to `groups`.
	* Set **Full group path** off.
	* Set **Add to ID token** on.
	* Set **Add to access token** on. 
	* Set **Add to user info** on.  
5. Repeat the above steps for any other clients. 


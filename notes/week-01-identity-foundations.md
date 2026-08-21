# Week 1 - Identity Foundations

## 1. Authentication vs Authorization

Authentication is the process of proving who a user is.

Authorization determines what that authenticated user is allowed to access or do.

## 2. Microsoft Entra ID

Microsoft Entra ID is Microsoft's cloud-based identity and access management service. It manages users, groups, applications, authentication, and access.

## 3. Security Groups

Security groups are used to manage access to resources.

Instead of assigning permissions individually to many users, permissions can be assigned to a group and users are added or removed from the group as needed.

### Groups created in my lab

- SG-IT-Users
- SG-Security-Analysts
- SG-HR-Users
- SG-Finance-Users
- SG-Clinical-Users
- SG-Contractors

## 4. Security Groups vs Microsoft 365 Groups

Security groups are mainly used for access control.

Microsoft 365 groups are mainly used for collaboration and can provide resources such as shared mailboxes, calendars, SharePoint sites, and Teams.

## 5. Assigned Membership

Assigned membership means an administrator manually selects which users belong to a group.

## 6. Least Privilege

Least privilege means giving users only the permissions required to perform their job and nothing more.

For example, a Security Analyst should not automatically receive Global Administrator access.

## 7. Role-Based Access Control

Role-Based Access Control provides permissions based on a user's job or function instead of assigning permissions separately to every person.

## 8. Administrative Roles

David Mensah was assigned the Security Reader role because his Security Analyst role requires read-only access to security information for investigation.

A higher privilege role such as Security Administrator or Global Administrator was not assigned because it would provide unnecessary permissions.

## 9. What I Learned

- Groups make access easier to manage at scale.
- Removing a user from a group can remove access associated with that group.
- Least privilege reduces unnecessary access.
- Administrative roles should match actual job responsibilities.
- Security Reader is more appropriate for an analyst who investigates security events but does not need to modify security settings.

## Administrative Role Assignment

David Mensah was assigned the Security Reader role.

This role was selected because a Security Analyst needs visibility into security information and reports for investigation, but does not require permission to modify security settings.

This follows the principle of least privilege by granting only the access required for the job function.

## Entra Administrative Roles vs Azure RBAC

Microsoft Entra administrative roles control permissions within the identity directory, such as users, groups, authentication, and security administration.

Azure RBAC controls access to Azure resources such as virtual machines, storage accounts, resource groups, and subscriptions.

Entra roles focus on identity administration, while Azure RBAC focuses on Azure resource management.

---
title: System and application users | Microsoft Docs
description: About system and application users 
author: jimholtz
ms.service: power-platform
ms.component: pa-admin
ms.topic: quickstart
ms.date: 12/20/2019
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---

# System and application users

There is a list of special system and application users that is created when the system is provisioned.  Special system users are created for integration and support scenarios. Application users are created during system provisioning for setup and configuration management.  [Application users](create-users-assign-online-security-roles.md#create-an-application-user) can also be used for performing back-end services.  

Most of these users are hidden from user views but they can be found by using the Advanced Find on the Users entity.  Do not delete or modify these users including changing or reassigning security role. 

|User type  |Full name  |User name  |Purpose  | Security role assigned |
|---------|---------|---------|---------|---------|
|System     | SYSTEM  | n/a        | See below         | n/a |
|     | Support user     |crmoln@microsoft.com          |To allow Microsoft support staff to have restricted/limited access to any customer environment for customer support  |Support user (does not have privilege to customer data)    |
|    | Delegated admin        |crmoln2@microsoft.com          |See [For partners: the Delegated admin](for-partners-delegated-administrator.md)        |System admin |
|Application     | Business Application Platform Service account |bap_sa@microsoft.com   |To setup Power Apps system and configurations |System admin   |
|    | Power Apps Checker Application        | Pacheckerapp@microsoft.com         |To perform static analysis of Power Apps solutions to assist in identifying performance and stability risks |Export customizations and Solution checker   |
|    | Provision User      | provisionapp@fabrikam.com         |To perfom Application installation from AppSource or System updates from Microsoft |System admin   |

**The purpose of the system account?** 
- The System user is a built-in user account that is used to allow customers to perform system updates via plug-ins. 
- The primary usage of this user account is to meet special business requirements that require elevation of privileges; for example, running background processes to integrate with other applications. 
- It can also be used to handle rollup scenarios where individual users do not have the required privilege. For example, the priority of a Case is automatically set to the highest priority of an individual user’s tasks and individual users can only update their own task priority but not the Case priority. 

**Technical details on permissions?**
- This user account can perform any actions and has all system privileges. 
- Records created/updated by this user account are audited. 

**Technical details on the security?**
- This user account cannot sign in to Dynamics 365 apps.  
- Administrators have the option to use this user account when registering their plug-ins. 
- This user account does not have a mailbox, so they cannot be used to send or receive emails. 
- The details of this user account cannot be modified from the User Form interface. 
- This user account does not show up in any views.





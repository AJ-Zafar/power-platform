---
title: Environments overview | Microsoft Docs
description: Learn about environments in Power Apps and how to use them
author: jimholtz
manager: kvivek
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---

# Environments overview
An environment is a space to store, manage, and share your organization's business data, apps, and flows. They also serve as containers to separate apps that may have different roles, security requirements, or target audiences. How you choose to leverage environments depends on your organization and the apps you are trying to build. For example:

* You may choose to only build your apps in a single environment.
* You might create separate environments that group the Test and production versions of your apps.
* You might create separate environments that correspond to specific teams or departments in your company, each containing the relevant data and apps for each audience.
* You might also create separate environments for different global branches of your company.  
* Get early access to the upcoming Power Apps functionalities by joining [Power Apps Preview Program](preview-environments.md).

## Environment scope
Each environment is created under an Azure AD tenant, and its resources can only be accessed by users within that tenant. An environment is also bound to a geographic location, like the US. When you create an app in an environment, that app is routed to only datacenters in that geographic location. Any items that you create in that environment (including connections, gateways, flows using Microsoft Power Automate, and more) are also bound to their environment's location.

Every environment can have zero or one Common Data Service databases, which provides storage for your apps. The ability to create a database for your environment will depend on the license you purchase for Power Apps and your permission within that environment. For more information, see [Pricing info](pricing-billing-skus.md).

When you create an app in an environment, that app is only permitted to connect to the data sources that are also deployed in that same environment, including connections, gateways, flows, and Common Data Service databases.  For example, let's consider a scenario where you have created two environments named 'Test' and 'Dev' and created a Common Data Service database in each of the environments. If you create an app in the 'Test' environment, it will only be permitted to connect to the 'Test' database, it won't be able to connect to the 'Dev' database.

There is also a process to move resources between environments. For more information, see [Migrate resources](environment-and-tenant-migration.md).

![](./media/environments-overview/Environments.png)

## Environment permissions
Environments have two built-in roles that provide access to permissions within an environment:

* The Environment Admin role can perform all administrative actions on an environment including the following:

    * Add or remove a user or group from either the Environment Admin or Environment Maker role

    * Provision a Common Data Service database for the environment

    * View and manage all resources created within an environment

    * Set data loss prevention policies. For more information see [Data loss prevention policies](prevent-data-loss.md).

    After creating the database in the environment, you can use System Administrator role instead of Environment Admin role.

* The Environment Maker role can create resources within an environment including apps, connections, custom connectors, gateways, and flows using Power Automate.

Environment Makers can also distribute the apps they build in an environment to other users in your organization by sharing the app with individual users, security groups, or to all users in the organization. For more information, see [Share an app in Power Apps](/powerapps/maker/canvas-apps/share-app).

Users or groups assigned to these environment roles are not automatically given access to the environment's database (if it exists) and must be given access separately by a Database owner. For more information, see [Configure database security](database-security.md).

Users or security groups can be assigned to either of these two roles by an Environment Admin from the [Power Platform admin center](https://admin.powerplatform.microsoft.com) or [Power Apps Admin center](https://admin.powerapps.com). For more information, see [Administer environments in Power Apps](environments-administration.md).

![](./media/environments-overview/EnvironmentRoles.png)

## Types of environments

There are multiple types of environments. The type of environment indicates the purpose and determines the environment characteristics. The following table summarizes the current types of environments that you might encounter.

<table style="width:100%">
<tr>
<th>Type</th>
<th>Description</th>
<th>Security</th>
</tr>
<tr>
<td width="20%"> Production</td>
<td width="50%">  This is intended to be used for permanent work in an organization. It can be created and owned by an administrator or anyone with a Power Apps license, provided there is 1GB available database capacity. These environments are also created for each existing Common Data Service database when it is upgraded to version 9.0 or later. Production environments are what you should use for any environments on which you depend.        </td>
<td width="30%"> Full control.  </td>
</tr>
<tr>
<td width="20%"> Default</td>
<td width="50%"> These are a special type of production environments. Each tenant will have a default environment created automatically and it has special characteristics described below in further detail. </td>
<td width="30%">  Limited control - all licensed users<sup>1</sup> are Environment Makers.</td>
<tr>
<td width="20%"> Sandbox</td>
<td width="50%">   These are non-production environments and offer features like copy and reset. Sandbox environments are used for development and testing, separated from production. Provisioning sandbox environments can be restricted to admins (since production environment creation can be blocked), but conversion from production cannot be blocked.   </td>
<td width="30%">  Full control. <br />If used for testing, only end user access is needed. <br />Developers require Environment Maker access to create resources.</td>
</tr>
<tr>
<td width="20%"> Trial</td>
<td width="50%">  Trial environments are intended to support short term testing needs and are automatically cleaned up after a short period of time. Expires after 30 days and are limited to 1 user. Provisioning trial environments can be restricted to admins.</td>
<td width="30%">  Full control.</td>
</tr>
<tr>
<td width="20%"> Developer</td>
<td width="50%">  Developer environments are created by users with the Community Plan license. They are special environments intended only for use by the owner. Sharing with other users is not possible. Provisioning developer environments can't be restricted unless through a support ticket. </td>
<td width="30%">  Only a single user account with the Community Plan has access.</td>
</tr>
</table>

<sup>1</sup>Users licensed for Power Apps, Power Automate, Office 365 and Dynamics 365 Online, stand-alone licenses, free and trial licenses.

## The default environment
A single default environment is automatically created by Power Apps for each tenant and shared by all users in that tenant. Whenever a new user signs up for Power Apps, they are automatically added to the Maker role of the default environment. The default environment is created in the closest region to the default region of the Azure AD tenant.

> [!NOTE]
> No users will be added to the Environment Admin role of the default environment automatically. For more information, see [Administer environments in Power Apps](environments-administration.md).
>
> The default environment is limited to 32GB of storage capacity. In case you need to store more data, you can create a production environment. See [Provisioning a new environment](create-environment.md#provisioning-a-new-environment).

The default environment is named as follows: "{Azure AD tenant name} (default)"

![](./media/environments-overview/DefaultEnvironment.png)

## Production and trial environments
You can create environments for different purpose. A trial environment is for trying out the environment and database with Common Data Service experience. It expires after certain period. 

## Manage environments in Power Platform admin center

You can view and manage your environments in the **Environments** page. 

> [!div class="mx-imgBorder"] 
> ![Environment list](media/environment-list.png "Environment list")

You can sort and search the list of environments - useful for those of you with a large number of environments to manage.

### Environment details

You can see some the details of your environments by selecting an environment. Select **See all** to see more environment details.

> [!div class="mx-imgBorder"] 
> ![Environment details](media/environment-details-see-all.png "Environment details")

Select **Edit** to review and edit all your environment details.

> [!div class="mx-imgBorder"] 
> ![More environment details](media/environment-details-more.png "More environment details")

## Choosing an environment in Power Apps admin center
With the introduction of environments, you will now see a new experience when you come to [https://make.powerapps.com](https://make.powerapps.com/?utm_source=padocs&utm_medium=linkinadoc&utm_campaign=referralsfromdoc).  The apps, connections, and other items that are visible in the site will now be filtered based on the current environment that is selected.  Your current environment is specified in the environment picker near the right edge of the header. To choose a different environment, click or tap the picker, and a list of available environments appears. Click or tap the one you wish to enter.

An environment will show up in your picker if you meet one of the following conditions:

* You are a member of the Environment Admin role for the environment.
* You are a member of the Environment Maker role for the environment.
* You are not an Environment Admin or Environment Maker of the environment, but you have been given 'Contributor' access to at least one app within the environment. For more information, see [share an app](/powerapps/maker/canvas-apps/share-app). In this case, you will not be able to create apps in this environment. You will only be able to modify the existing apps that have been shared with you.

![](./media/environments-overview/EnvironmentPicker.png)

### See also
[Microsoft Learn: Create and manage environments in Common Data Service](https://docs.microsoft.com/learn/modules/create-manage-environments/)<br />
[About environments](wp-environments.md)

---
title: Manage environments in the Power Apps Admin center| Microsoft Docs
description: Learn how to manage environments in Power Apps, including creation, renaming, deletion, and security
author: jimholtz
manager: kvivek
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---

# Manage environments in the Power Apps Admin center

> [!NOTE]
> We are moving environment management from other admin centers to the Power Platform admin center. Until this is completed, some management can be or must be done in other admin centers such as the Power Apps Admin center.

In the [Power Apps Admin center][1], manage environments that you've created and those for which you have been added to the Environment Admin or System Administrator role. From the admin center, you can perform these administrative actions:

* Create environments.
* Rename environments.
* Add or remove a user or group from either the Environment Admin or Environment Maker role.
* Provision a Common Data Service database for the environment.
* Set Data Loss Prevention policies.
* Set database security policies (as open or restricted by database roles).
* Members of the Azure AD tenant Global administrator role (includes Global admins) can also manage all environments that have been created in their tenant and set tenant-wide policies.

For more information, see [Environments overview](environments-overview.md).

## Access the Power Apps Admin center
To access the Power Apps Admin center:

* Go directly to [admin.powerapps.com][1], OR

* Go to [powerapps.com][2], and then select the gear icon in the  navigation header.

    ![](./media/environment-admin/powerapps-gear-dropdown.png)

To manage an environment in the Power Apps Admin center, you must have one of these roles:

* The Environment Admin or System Administrator role of the environment, OR

* The Global Administrator role of your Azure AD or Microsoft 365 tenant.

You also need either a Power Apps plan or Power Automate plan to access the admin center. For more information, see the [Power Apps pricing page][3].

> [!IMPORTANT]
> Any changes that you make in Power Apps Admin center affect the [Power Automate admin center][4] and vice versa.

## Create an environment
For instructions on how to create an environment, see [Create an environment](create-environment-powerapps.md).

## View your environments
When you open the admin center, the Environments tab appears by default and lists all the environments for which you are an Environment Admin (as shown below):

![](./media/environment-admin/environment-list-new.png)

If you are a member of the Global Administrator role of your Azure AD or Microsoft 365 tenant, all the environments that have been created by users in your tenant appear, because you're automatically an Environment Admin for all of them.

## Rename your environment
1. Open the [Power Apps Admin center][1], find the environment to be renamed in the list, and click or tap it.

    ![](./media/environment-admin/environment-list-updated3.png)

2. Click or tap **Details**.

    ![](./media/environment-admin/environment-rename-details-2.png)
3. in the **Name** text box, enter the new name, then click **Save**.

    ![](./media/environment-admin/environment-rename-2.png)

    If you have created the database in the environment, then you will not see this option. You can rename the environment from Dynamics 365 Admin center by clicking on the link in **Details** tab.

    ![](./media/environment-admin/Delete-D365AdminCenter.png)

## Delete your environment
1. In the [Power Apps Admin center][1], click or tap the environment that you want to delete.

    ![](./media/environment-admin/environment-list-updated3.png)
2. Click or tap **Details**.

    ![](./media/environment-admin/environment-rename-details-2.png)
3. Click or tap **Delete environment** to delete your environment.

    ![](./media/environment-admin/delete-environment-2.png)

## Create a Common Data Service database for an environment
If an environment doesn't already have a database, an Environment Admin can create one in the [Power Apps Admin center][1] by following these steps. Only users with a Power Apps plan  can create Common Data Service databases.

1. Select an environment in the environments table.

    ![](./media/environment-admin/choose-environment-updated.png)
2. Select the **Details** tab.
3. Select **Create a database**.

    ![](./media/environment-admin/Create-DB-From-Details.png)

After you create a database, choose a security model. For more information, see [Configure database security](database-security.md).

## Manage security for your environments

### Environment permissions
In an environment, all the users in the Azure AD tenant are users of that environment. However, for them to play a more privileged role, they need to be added to a specific environment role. Environments have two built-in roles that provide access to permissions within an environment:

* The **Environment Admin** role (or **System Administrator** role) can perform all administrative actions on an environment including the following:
    * Add or remove a user from either the Environment Admin or Environment Maker role.

    * Provision a Common Data Service database for the environment.

    * View and manage all resources created within an environment.

    * Set data loss prevention policies. For more information, see [Data loss prevention policies](prevent-data-loss.md).

  > [!NOTE]
  > If the environment has the database, then you need to assign users the **System Administrator** role, instead of the **Environment Admin** role.

* The **Environment Maker** role can create resources within an environment including apps, connections, custom connectors, gateways, and flows using Power Automate. Environment Makers can also distribute the apps they build in an environment to other users in your organization. They can share the app with individual users, security groups, or all users in the organization. For more information, see [Share an app in Power Apps](https://docs.microsoft.com/powerapps/maker/canvas-apps/share-app).

To assign a user or a security group to an environment role, an Environment Admin can take these steps in the [Power Apps Admin center][1]:

1. Select the environment in environments table.

    ![](./media/environment-admin/choose-environment-updated.png)
2. Select **Security** tab.
3. If there is no database created in the environment:

    a. Select either the **Environment Admin** or **Environment Maker** role.

    ![](./media/environment-admin/choose-environment-role.png)

    b. Specify the names of one or more users or security groups in Azure Active Directory, or specify that you want to add your entire organization.

    ![](./media/environment-admin/enter-name.png)

    c. Select **Save** to update the assignments to the environment role.

4. If database is created in the environment:

    a. Add the user to the environment and click on the link to assign the user a role.

    ![](./media/database-security/security-adduser.png)

    b. Select the user from the list of users in the environment / environment.

    ![](./media/environment-admin/D365-Select-User.png)

    c. Assign the role to the user.

    ![](./media/environment-admin/D365-Assign-Role.png)

    d. Select **OK** to update the assignments to the environment role.

> [!NOTE]
> Users or groups assigned to these environment roles are not automatically given access to the environment's database (if it exists) and must be given access separately by a Database owner. For more information, see [Configure database security](database-security.md).  

### Database security
The ability to create and modify a database schema and to connect to the data stored within a database that is provisioned in your environment is controlled by the database's user roles and permission sets. You can manage the user roles and permission sets for your environment's database from the **User roles** and **Permission sets** section of the **Security** tab. For more information, see [Configure database security](database-security.md).

![](./media/environment-admin/D365-Assign-Role.png)

## Data policies
An organization's data must be protected so that it isn't shared with audiences that should not have access to it. To protect this data, you can create and enforce policies that define which consumer services and connector-specific business data can be shared with. Policies that define how data can be shared are referred to as data loss prevention (DLP) policies. You can manage the DLP policies for your environments  from the **Data Policies** section of the [Power Apps Admin center][1].  For more information, see [Data loss prevention policies](prevent-data-loss.md).

![](./media/environment-admin/data-policies.png)

## Frequently asked questions

### How many environments and databases can I create?
Provisioning environments is based on the available storage in your organization. You need at least 1GB minimum database storage to create an environment.For more information, see [Environments overview](environments-overview.md). 

### Which license includes Common Data Service?
Power Apps plan.  See [Power Apps pricing page][3] for details on all the plans that include this license.

### While trying to create a new environment, I am getting an error. How should I resolve it?
If you are getting the following error message: "Either your plan doesn't support the environment type selected or you've reached the limit for that type of environment.", it can mean one of the two things:

1. You have already utilized your quota to create a specific type of environments. Say you were creating  a trial environment and you get this error message. That means, that you have already provisioned two trial environments. You can view all the environments in [Power Apps Admin center][1].
If you want, you can delete an existing environment of that specific type and create a new one. But, please make sure that you don't lose your data, apps, flows and other resources which you want to retain.

2. You do not have a quota to create that specific type of the environment. <!-- Check what type of environment you can create [here](environments-overview.md#creating-an-environment). -->

If you are getting any other error message or have more questions, please connect with us [here][5].

### When will my trial environment expire?   
Trial environments expire after 30 days from their creation. If you don't want your trial environments to expire, you can [convert them to production environments](trial-environments.md#convert-a-trial-environment-to-production). 

### Does my current database (created with previous version of the Common Data Service) also gets counted in the quota?
If you had a database (created with previous version of the Common Data Service), they will also get counted with your production environment quota. If you now create a database in an environment (created prior to March 15, 2018) then it will also get counted as production environment.

### Can I rename an environment?
Yes, this functionality is available from the Power Apps Admin center. See [Environments Administration](environments-administration.md#rename-your-environment) for more details.

### Can I delete an environment?
Yes, this functionality is available from the Power Apps Admin center. See [Environments Administration](environments-administration.md#delete-your-environment) for more details.

Please note that you currently can't delete a production environment with a database (with latest version of the Common Data Service). This will be coming soon!

### As an Environment Admin, can I view and manage all resources (apps, flows, APIs, etc.) for an environment?
Yes, the ability to view the apps and flows for an environment is available from the Power Apps Admin center. See [View Apps](admin-view-apps.md) for more details.



<!--Reference links in article-->
[1]: https://admin.powerapps.com
[2]: https://make.powerapps.com
[3]: https://powerapps.microsoft.com/pricing/
[4]: https://admin.flow.microsoft.com
[5]: https://go.microsoft.com/fwlink/p/?linkid=871628

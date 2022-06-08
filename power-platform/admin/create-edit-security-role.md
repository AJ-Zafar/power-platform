---
title: "Create or edit a security role  | MicrosoftDocs"
description: Create or edit a security role
author: paulliew
ms.subservice: admin
ms.author: paulliew
ms.reviewer: jimholtz
ms.custom: "admin-security"
ms.component: pa-admin
ms.topic: conceptual
ms.date: 06/16/2022
search.audienceType: 
  - admin
search.app:
  - D365CE
  - PowerApps
  - Powerplatform
  - Flow
---
# Create or edit a security role to manage access 

You can create new security roles to accommodate changes in your business requirements or you can edit the privileges associated with an existing security role.
  
 If you need to back up your security role changes, or export security roles for use in a different implementation, you can export them as part of exporting customizations. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Export your customizations as a solution](/powerapps/maker/common-data-service/use-solutions-for-your-customizations)  

For information on predefined security roles, see [Predefined security roles](database-security.md#predefined-security-roles).
  
## Create a security role  
  
1. Make sure that you have the System Administrator permission.
  
    Check your security role  
  
   - [!INCLUDE[proc_check_your_security_role](../includes/proc-check-your-security-role.md)]  
  
   - [!INCLUDE[proc_dont_have_correct_permissions](../includes/proc-dont-have-correct-permissions.md)]  
  
2. In the Microsoft Power Platform admin center, select an environment. 

3. Select **Settings** > **Users + permissions** > **Security roles**.  
   
4. On the Actions toolbar, select **New**.  
  
5. Set the privileges on each tab.  
  
    To change the access level for a privilege, select the symbol until you see the symbol you want. The possible access levels depend on whether the record type is organization-owned or user-owned.  
  
   > [!TIP]
   >  To cycle through the access levels, you can also select the privilege column heading, or select the record type multiple times.  
   >  There are a set of minimum privileges that are required in order for the new security role to be used - see below Minimum Privileges for common tasks.
  
6. When you have finished configuring the security role, on the toolbar, select or tap **Save and Close**.  
  
## Create a security role by Copy Role

1. Make sure that you have the System Administrator permission.
  
    Check your security role  
  
   - [!INCLUDE[proc_check_your_security_role](../includes/proc-check-your-security-role.md)]  
  
   - [!INCLUDE[proc_dont_have_correct_permissions](../includes/proc-dont-have-correct-permissions.md)]  
  
2. In the Power Platform admin center, select an environment. 

3. Select **Settings** > **Users + permissions** > **Security roles**.  
  
4. Select the Security role you want to copy from.

5. On the Actions toolbar, select **Copy Role**.  

6. Enter the New Role Name, and check the box for **Open the new security role when copying is complete**.

7. Select **OK**.
  
8. When Copying Role is complete, navigate to each tab, ie Core Records, Business Management, Customization, etc.

9. Set the privileges on each tab.

> [!TIP]
>  To cycle through the access levels, you can also select the privilege column heading, or select the record type multiple times.  
>  There are a set of minimum privileges that are required in order for the new security role to be used - see below Minimum Privileges for common tasks.

## Edit a security role  
 Before you edit an existing security role, make sure that you understand the principles of data access. [!INCLUDE[proc_more_information](../includes/proc-more-information.md)] [Controlling Data Access](../admin/security-roles-privileges.md)  
  
> [!NOTE]
> You can't edit the System Administrator security role. To create a security role similar to the System Administrator security role, copy the System Administrator security role, and make changes to the new role.  
  
1. Make sure that you have the System Administrator or equivalent permissions.
  
    Check your security role  
  
   - [!INCLUDE[proc_check_your_security_role](../includes/proc-check-your-security-role.md)]  
  
   - [!INCLUDE[proc_dont_have_correct_permissions](../includes/proc-dont-have-correct-permissions.md)]  
  
2. In the Power Platform admin center, select an environment. 

3. Select **Settings** > **Users + permissions** > **Security roles**.  
  
4. In the list of security roles, double-select or tap a name to open the page associated with that security role.  
  
5. Set the privileges on each tab.  
  
    To change the access level for a privilege, select the symbol until you see the symbol you want. The possible access levels depend on whether the record type is organization-owned or user-owned.  
  
   > [!TIP]
   >  To cycle through the access levels, you can also select the privilege column heading, or select the record type multiple times.  
   >  There are a set of minimum privileges that are required in order for the new security role to be used - see below Minimum Privileges for common tasks. 
    
6. When you have finished configuring the security role, on the toolbar, select or tap **Save and Close**.  
  
## Remove predefined security role edits

All edits done to [predefined security roles](database-security.md#predefined-security-roles) by the environment admin are managed by our solution layering. These edits can be removed to restore the predefined security role to the default system settings. 

1. Sign in to [Power Apps](https://make.powerapps.com/) as an admin (local environment admin, Dynamics 365 admin, Global admin, or Microsoft Power Platform admin). 

2. In the environment selector, select the environment that your security role resides.  

   :::image type="content" source="media/security-select-environment.png" alt-text="Select an environment.":::

3. From the left-navigation, select **Solutions**. 

4. Scroll down and double-click on **Default Solution**. 

5. In the Objects column, scroll down and select **Security roles**. 

   :::image type="content" source="media/security-objects-security-roles.png" alt-text="Select Objects > Security roles.":::

6. Select the check mark to the left of the security role that you want to update. 

   :::image type="content" source="media/security-select-security-role.png" alt-text="Select a security role.":::

7. Select **Advanced** in the action bar, and then select **See solution layers**. 

   :::image type="content" source="media/security-select-see-solution-layers.png" alt-text="Select Advanced > See solution layers.":::

8. Select the row with a solution name of **Unmanaged** layer. 

9. Select **Remove unmanaged layer button** on the action bar. 

   :::image type="content" source="media/security-remove-unmanaged-layer.png" alt-text="Select Unmanaged layer > Remove unmanaged layer.":::

All your edits to the security role will be removed. 

## Minimum privileges for common tasks  
 It's helpful to keep in mind the minimum privileges that are needed for some common tasks. This means that a user is required to have a security role with these privileges in order to run applications. 
 
 We've created a solution you can import that provides a security role with the required minimum privileges.

Start by downloading the solution from the Download Center: [Microsoft Dataverse minimum privilege security role](https://download.microsoft.com/download/6/5/5/6552A30E-05F4-45F0-AEE3-9BB01E13118A/MinprivilegeSecRole_1_0_0_2.zip).

Then, follow the directions to import the solution: [Import, update, and export solutions](/powerapps/maker/common-data-service/import-update-export-solutions).

When you import the solution, it creates the **min prv apps use** role which you can copy (see: [Create a security role by Copy Role](#create-a-security-role-by-copy-role)). When Copying Role is complete, navigate to each tab - Core Records, Business Management, Customization, etc - and set the appropriate privileges. 

> [!IMPORTANT]
> You should try out the solution in a development environment before importing into a production environment. 
  
- When logging in to customer engagement apps:  
  
  -   Assign the min prv apps use security role or a copy of this security role to your user.
  
  -   To render an table grid (that is, to view lists of records and other data), assign the following privileges on the Core Records tab: Read privilege on the table, Read Saved View, Create/Read/Write User Entity UI Settings
      and assign the following privilege on the Business Management tab: Read User.   

- When logging in to [!INCLUDE[pn_crm_for_outlook_short](../includes/pn-crm-for-outlook-short.md)]:

   - To render navigation for customer engagement apps and all buttons: assign the min prv apps use security role or a copy of this security role to your user   
  
  - To render an table grid: assign Read privilege on the table  
  
  - To render tables: assign Read privilege on the table  
  
## Privacy notices  
[!INCLUDE[cc_privacy_crm_for_phones](../includes/cc-privacy-crm-for-phones.md)]
  
[!INCLUDE[cc_privacy_crm_for_tablets](../includes/cc-privacy-crm-for-tablets.md)]
  
[!INCLUDE[cc_privacy_crm_go_offline_in_outlook](../includes/cc-privacy-crm-go-offline-in-outlook.md)]
  
[!INCLUDE[cc_privacy_crm_sync_to_outlook](../includes/cc-privacy-crm-sync-to-outlook.md)]
  
 [!INCLUDE[cc_privacy_export_to_excel](../includes/cc-privacy-export-to-excel.md)]
  
 [!INCLUDE[cc_privacy_crm_print](../includes/cc-privacy-crm-print.md)]
  
### See also  
 [Security concepts](../admin/wp-security-cds.md)   
 [Copy a security role](../admin/copy-security-role.md)


[!INCLUDE[footer-include](../includes/footer-banner.md)]
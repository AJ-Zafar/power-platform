---
title: Download a list of active users in your tenant | Microsoft Docs
description: In this quickstart, you learn how to download a list of active users in your tenant
author: jimholtz
ms.service: power-platform
ms.component: pa-admin
ms.topic: quickstart
ms.date: 03/21/2018
ms.author: jimholtz
search.audienceType: 
  - admin
search.app:
  - D365CE
  - PowerApps
  - Powerplatform
  - Flow
---

# Download a list of active users in your tenant
If you're a 365 Global admin or Power Platform admin, you can download a list of active users in your tenant, so you can see not only who's accessed Power Apps, Power Automate, or both, but also the licenses assigned to those users.

In this topics, you'll learn how to download a list of active users to a .csv file, and then view that list in Excel.

To follow the steps, you need Global admin or Power Platform service admin permissions.

## Sign in to the Power Apps Admin center
Sign in to the Admin center at [https://admin.powerapps.com](https://admin.powerapps.com).

## Download the list of users
In the navigation pane, click or tap **User licenses**, and then click or tap **Download a list of active user licenses**.

![File and Share](./media/admin-view-user-licenses/download-list.png)

The list of users is downloaded into a .csv file. This process could take several minutes. Make sure that you don't close the window before the list completely downloads or you may have to restart the process.

## View the list
After the .csv file is created, open it in Excel. The list contains each user's name, email address, license type, and other information.

A user who's accessed a product at least once is considered an *active user*. Since this is a list of active users, it does not contain users who have licenses for Power Apps and Power Automate but have never accessed them. You can view all user licenses from the [Microsoft 365 admin center](https://support.office.com/article/Assign-or-remove-licenses-for-Office-365-for-business-997596b5-4173-4627-b915-36abac6786dc).

The following example shows two users who have licenses to both Power Apps and Power Automate. Jane Doe has access through a subscription to Office 365, and John Doe has a trial license for each product.

![File and Share](./media/admin-view-user-licenses/table2.png)

If a user has left the organization, the list will show **Unknown** in the **User name** and **Email address** columns. If the list shows **Unknown** but nobody has left the organization, wait several minutes, and then download the list again.

To add user licenses, open the [Microsoft 365 admin center](https://support.office.com/article/Assign-or-remove-licenses-for-Office-365-for-business-997596b5-4173-4627-b915-36abac6786dc).

## Next steps
In this topic, you learned how to download and view a list of active users in your tenant. To learn how to download and view a list of apps created in your environments, continue to the next topic.

> [!div class="nextstepaction"]
> [Download a list of apps created in your environments](admin-view-apps.md)

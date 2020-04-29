---
title: "Set up audit log components | MicrosoftDocs"
description: "The *Audit Log Sync* flow connects to the Office 365 Audit Log to gather telemetry data (unique users, launches) for apps"
author: manuelap-msft
manager: devkeydet
ms.service: power-platform
ms.component: pa-admin
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: mapichle
ms.reviewer: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---
# Set up the Audit Log connector

The *Audit Log Sync* flow connects to the Office 365 Audit Log to gather telemetry data (unique users, launches) for apps. The flow uses a custom connector to connect to the Office 365 Audit Log. In the following instructions, you'll set up the custom connector and configure the flow.

The Center of Excellence (CoE) Starter Kit will work without this flow, but the usage information (app launches, unique users) in the Power BI dashboard will be blank.

There are two options for connecting to the audit log: one uses basic authentication (username and password), and one uses Azure App Registration to establish an identity for your app and allow access to the APIs.

## Option 1: Connect to the audit log by using basic authentication

Make sure the account that is used to configure this section has permission to access the audit logs and doesn't have multifactor authentication enabled. Global tenant admins have access to the audit logs by default and can grant access to the audit logs for other user accounts or groups through the Exchange admin center.

Keep in mind that after a user account has access to the audit logs, that user has access to all audit logs across every Microsoft service that reports telemetry to audit logs.

### Install the custom connector

1. Go to [flow.microsoft.com](https://flow.microsoft.com), and set the current environment to the same environment where the CoE solution is installed.

1. In the left navigation pane, expand **Data**, and then select **Custom Connectors**.

1. Select **+ New custom connector – Import an Open API file**.

   ![Create a custom connector page](media/coe27.png "Create a custom connector page")

1. Provide a connector name (**Office 365 Audit Logs**), and then select the .swagger file that can be found in the CoE Starter Kit pack you downloaded.

1. Select **Create Connector**. You don't need to change the Security and Definition information.

1. Select **4. Test**.

   ![Test your custom connector page](media/coe28.png "Test your custom connector page")

1. Select **+ New Connection** to create a connection to your connector.

1. Enter the email address and password of the user who has access to audit logs, and then select **Create connection**.

   ![Create new connection option](media/coe29.png "Create new connection option")

1. Select the refresh icon in the upper-right corner corner of the **Connections** area to ensure that the new connection is selected.

1. Provide a **Start Date** and **End Date** for the **GetActivitiesByOperation**.

1. Select **Test Operation**.

   You should receive a (200) response, which indicates the query has been successfully executed.
   
   ![The Get Activities By Operation action of the custom connector](media/coe30.png "The Get Activities By Operation action of the custom connector]")

More information: [custom connector documentation](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition).

### Import the flow template compressed (.zip) package named SyncAuditLogs.zip

1. Go to [flow.microsoft.com](https://flow.microsoft.com), and set the current environment to the same environment where the CoE solution is installed.

1. In the left navigation pane, navigate to the **My Flows** tab.

1. Select **Import**.

1. Select the **Flow-SyncAuditLogs.zip** package, and then select **Import**.

1. Update the connections:
    1. For Admin \| Sync Audit Logs, select **Create as new**, and then select **Save**.

    1. For the Office 365 Audit Logs connector, Common Data Service Connection, and Office 365 Audit Log Connection, select **Select during import**, and then choose your connection.
    
       ![Import options when importing a new flow](media/coe31.png "Import options when importing a new flow]")

    1. After the connections are configured, select **Import**.

    1. Open the flow, and make sure there are no errors for any of the actions.

    1. Select the back arrow in the upper-left corner to go back to the flow details
        screen.

    1. If the flow isn't on yet, turn on the flow.

    1. Run the flow to start syncing audit log data to the Common Data Service entity.

## Option 2: Connect to the audit log by using an Azure app registration

The Office 365 Management APIs use Azure AD to provide authentication services that you can use to grant rights for your application to access them.

### Create an Azure app registration for the Office 365 Management API

Using these steps, you'll set up an Azure AD app registration that will be used in a custom connector and Power Automate flow to connect to the Office 365 audit log. More information: [Get started with Office 365 Management APIs](https://docs.microsoft.com/office/office-365-management-api/get-started-with-office-365-management-apis)

1. Sign in to [portal.azure.com](https://portal.azure.com).

1. Go to **Azure Active Directory / App registrations**.

   ![Azure Active Directory - App registration](media/coe33.png "Azure Active Directory - App registration")

1. Select **+ New Registration**.

1. Provide a name (for example, **Office 365 Management**), don't change any of the other settings, and then select **Register**.

1. Select **API Permissions** > **+ Add a permission**.

   ![API Permissions - Add a permission](media/coe34.png "Add a permission")

1. Select **Office 365 Management API**, and configure permissions:

   1. Select **Delegated permissions**, and then select **ActivityFeed.Read**.

      ![Delegated permissions](media/coe36.png "Delegated permissions")

   1. Select **Application permissions**, and then select **ActivityFeed.Read** and **ServiceHealth.Read**.

      ![Application permissions](media/coe37.png "Application permissions")

    1. Select **Add permissions**.

1. Select **Grant Admin Consent for (your organization)**.

   The API permissions should now reflect delegated **ActivityFeed.Read** and application **ActivityFeed.Read / ServiceHealth.Read** permissions, with a status of **Granted for _(your organization)_**.
   
   ![API permissions](media/coe38.png "API permissions")

1. Select **Certificates and secrets**.

1. Select **+ New client secret**.

   ![New client secret](media/coe39.png "New client secret")

1. Add a description and expiration (in line with your company policies), and then select **Add**.

1. Copy and paste the **Secret** to a text document in Notepad for the time being.

1. Select **Overview**, and copy and paste the application (client) ID and directory (tenant) ID values to the same text document; be sure to make a note of which GUID is for which value. You'll need these values in the next step as you configure the custom connector.

1. Leave the Azure portal open, because you'll need to make some configuration updates after you set up the custom connector.

### Set up the custom connector

Now you'll configure and set up a custom connector that uses the [Office 365 Management APIs](https://docs.microsoft.com/office/office-365-management-api/get-started-with-office-365-management-apis).

1. Download the **Center of Excellence – Audit Logs** solution from [GitHub](https://github.com/microsoft/powerapps-tools/tree/master/Administration/CoEStarterKit/Audit%20Log%20(MFA)).

1. Go to [make.powerapps.com](https://make.powerapps.com).

1. Import the **Center of Excellence – Audit Logs** (CenterofExcellenceAuditLogs_x_x_x_xxx_managed.zip) solution, which contains the custom connector and Power Automate flow to sync audit logs to CoE Common Data Service entities.

   ![CoE – Audit Logs solution](media/coe40.png "CoE - Audit Logs solution")

1. Open the solution, select the **Office 365 Management API custom connector**, and then select **Edit**.

   ![Custom connector setup](media/coe41.png "Custom connector setup")

1. Leave the **1. General** page as-is, and then select **2. Security**.

1. Select **Edit** at the bottom of the **OAuth 2.0** area to edit the authentication parameters.

   ![Edit OAuth configuration](media/coe42.png "Edit OAuth configuration")

1. Paste the application (client) ID you copied from the app registration into **Client Id**.

1. Paste the client secret you copied from the app registration into **Client secret**.

1. Don't change the **Tenant ID**.

1. Set the **Resource URL** to https://manage.office.com

1. Copy the **Redirect URL** into your text document in Notepad.

1. Select **Update Connector**.

### Update Azure AD app registration with the redirect URL

1. Go back to the Azure portal and your app registrations.

1. Under Overview, select **Add a Redirect URI**.

1. Select **+ Add a platform** > **Web**.

1. Enter the URL you copied from the **Redirect URL** section of the custom connector.

1. Select **Configure**.

### Start a subscription to the Audit Log Content

Go back to the custom connector to set up a connection to the custom connector and [start a subscription to the audit log content](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference#start-a-subscription), as described in the following steps.

> [!IMPORTANT]
> It's important to complete these steps for the flow to work.

1. On the **Custom Connector** page, select **4. Test**.

1. Select **+ New connection**, and then sign in with your account.

1. Under **Operations**, select **StartSubscription**.

   ![Custom connector Start Subscription](media/coe43.png "Custom connector Start Subscription")

1. Paste the directory (tenant) ID under **Tenant**, and paste the application (client) ID under **PublisherIdentifier**.

 1. Select **Test Operation**.

You should see a (200) status returned, which means the query was successful.

![Successful status being returned from the StartSubscription activity](media/coe44.png "Successful status being returned from the StartSubscription activity")

> [!IMPORTANT]
> If you don't see a (200) response, the request has failed and there is an error with your setup. Therefore, the flow won't work.

## Set up the Power Automate flow

A Power Automate flow uses the custom connector, queries the audit log daily, and writes the Power Apps launch events to a Common Data Service entity, which is then used in the Power BI dashboard to report on sessions and unique users of an app.

1. Go to [make.powerapps.com](https://make.powerapps.com).

1. Open the **Center of Excellence – Audit Log solution**, and edit the **Admin \| Sync Audit Logs**. 

    1. Select the flow in the solution to open it in the maker portal.

       ![Select Admin Sync Audit Flow from solution](media/coe45.PNG "Select Admin Sync Audit Flow from solution")

    1. Select **Edit**. (You won't be able to select **Edit** directly from the solution.)
    
       ![Edit flow](media/coe46.PNG "Edit flow")

1. Look at the **Initialize tenantID** action, and paste in the ID you copied for directory (tenant) ID. 

   ![Initialize Tenant ID in Power Automate](media/coe47.png "Initialize Tenant ID in Power Automate")

1. (Optional) Update the time interval at which the log clusters should be retrieved. The default is set to one-day intervals (from the options of Month, Week, Day, Hour, Minute, or Second).

1. (Optional) Update the start time and end time during which the logs will be read. The maximum is seven days in the past, and the end time must be after the start time. Use a positive number in the interval field.

   [Update the start time and end time during which the logs will read](media/coe48.png "Update the start time and end time during which the logs will read")

1. Go back to the Center of Excellence - Audit Log solution, and open the flow details screen of the *(Child) Admin \| Sync Logs* flow by selecting the display name.

1. Edit the **Run only users** settings.

   ![Child Flow - Run Only Users](media/coe49.png "Child FLow - Run Only Users")

1. For both connections (the custom connector and Common Data Service), change the drop-down value to **Use this connection (userPrincipalName\@company.com)**. If there is no connection for any of the connectors, go to **Data** > **Connections**, and create one for the connector.

   ![Configure Run Only Users](media/coe50.png "Configure Run Only Users")

1. Select **Save**, and then close the **Flow details** tab.

1. Select **Turn on** in the top navigation to turn on the child flow.

1. Refresh the page to make sure the status has changed to **On**.

1. Go back to the Center of Excellence – Audit Log solution, select **Admin \| Sync Audit Log** to open the flow details page, and then select **Turn on** for this flow as well.

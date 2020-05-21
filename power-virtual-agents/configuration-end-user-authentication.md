---
title: "Configure user authentication in Power Virtual Agents"
description: "Configure authentication with your identity provider to enable users to sign in when having a bot conversation."
keywords: "Authentication, IdP,"
ms.date: 1/24/2020
ms.service:
  - dynamics-365-ai
ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.custom: authentication
ms.collection: virtual-agent
---

# Configure end-user authentication in Power Virtual Agents

You can configure a Power Virtual Agents bot to provide authentication capabilities, so users can sign in with any [OAuth2 identity provider](/azure/active-directory/develop/v2-oauth2-auth-code-flow), such as Azure Active Directory (Azure AD), a Microsoft account, or Facebook. 

To learn how to add authentication to a bot topic, see [Add user authentication to a Power Virtual Agents bot](advanced-end-user-authentication.md).

Power Virtual Agents supports any identity provider that is compliant with the [OAuth2 standard](/azure/active-directory/develop/v2-oauth2-auth-code-flow).

> 
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4n4G2]
>

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]


## Register a new app with your identity provider

You need to register a new app with your identity provider and get a Client ID and Client Secret before you can configure authentication in Power Virtual Agents. This section describes how to do that with the [Azure portal](https://portal.azure.com).

Make sure to configure the redirect URL to be `https://token.botframework.com/.auth/web/redirect`, and that the assigned API permissions and scopes for the app are the same permissions you need the bot to access.

> [!IMPORTANT] 
> Your app registration redirect URL must be `https://token.botframework.com/.auth/web/redirect`.<br/>
> Ensure that the app has the correct API permissions and its related scopes.

### Use Azure Active Directory as your identity provider

**Create an app registration**

1. Sign in to the [Azure portal](https://portal.azure.com), using an admin account on the same tenant as your Power Virtual Agents chatbot.
1. Go to [App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade), either by selecting the icon or searching in the top search bar. Create a new 'Application Registration'
1. Select **New registration** and enter a name for the registration. It can be helpful to use the name of the bot you're enabling authentication for. For example, if your bot is called "Contoso sales help", you might name the app registration as "ContosoSalesReg" or something similar. 
1. Select **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)** 
1. Leave the **Redirect URI** section blank for now, as you'll enter that information in the next steps. Select **Register**.

**Add the redirect URL**

1. Once the registration is completed, it will open to the **Overview** page. Go to **Authentication** and then select **Add a platform**.
1. On the **configure platforms** blade select **Web**. Under **Redirect URIs** add `https://token.botframework.com/.auth/web/redirect`. Under the **Implicit grant** section, select the **Id Tokens** and **Access Tokens** checkboxes.
1. Select **Configure** to confirm your changes.

**Generate a client secret**

1. Go to **Certificates & Secrets**.
2. Under the **Client secrets** section, select  **New client secret**. Enter a description (one will be provided if you leave this blank), and select the expiry period. Select the shortest period that will be relevant for the life of your bot.
3. Select **Add** to create the secret. Take note of the secret's **Value** and store this in a temporary place (such as an open Notepad document), as you'll enter it in your bot's authentication settings.


## Configure authentication

1. Sign in to Power Virtual Agents. If you're using Azure AD as your identity provider, ensure you log in on the same tenant where you created the app registration.
1. Confirm you've selected the bot for which you want to enable authentication by selecting the bot icon on the top menu and choosing the bot. 
1. Select **Manage** on the side navigation pane, and then go to the **Authentication** tab.

   ![Go to Manage and then Authentication](media/auth-manage-sm.png)

2. Enter the information as described for each of the fields in the following table. The information required depends on your setup and provider. If you have questions about the required information, contact your administrator or identity provider.

3. Click **Save** to finish the configuration.

> [!NOTE]
> The examples provided below are for an Azure AD common endpoint. For more information, see [OAuth generic providers](/azure/bot-service/bot-builder-concept-identity-providers?view=azure-bot-service-4.0&tabs=adv1%2Cga2) documentation.
>Only use Azure AD V2 token endpoints, as specified in the table.

Field name | Description | Where to get this information for Azure AD
---|---
Connection name | Friendly name for your identity provider connection. This can be any string, but can't be changed once configured. | Not applicable.
Service Provider | This field can't be edited because Power Virtual Agents only supports generic OAuth2 providers. | Not applicable.
Client ID | Your client ID obtained from the identity provider. | On the app registration's **Overview** page as **Application (client) ID**.
Client Secret | Your client secret obtained from the identity provider registration. | When generating a new client secret. If you navigate away from the **Certificates & secrets** page, the secret's **Value** will be obfuscated and you'll need to create a new one. 
Token exchange URL (required for single sign-on) | This is an optional field used when [configuring single sign-on](configure-sso.md). | xx
Refresh URL Query String Template | Refresh URL query string separator for the token URL. Usually a question mark '?'. | Use a question mark `?`.
Refresh Body Template | Template for the refresh body.  | Use `refresh_token={RefreshToken}&redirect_uri={RedirectUrl}&grant_type=refresh_token&client_id={ClientId}&client_secret={ClientSecret}`.
Scopes | List of [scopes](/azure/active-directory/develop/developer-glossary#scopes) you want authenticated users to have once signed in. Make sure you're only setting the necessary scopes, and follow the [Least privilege access control principle](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models).<br/>For example, `User.Read`. <br/>Note: If you're using a custom scope, use the full URI including the exposed Application ID URI. | On the **API permissions** page, note the scopes listed under the **API / Permissions** name section. Use spaces to separate multiple scopes. For custom scopes defined by an exposed API, include the API ID: on the **Expose an API** page, prepend the **Application ID URI** and ending slash `/` to the scope name. For example, if your custom scope name is `app.scope.sso`, and the **Application ID URI** is `api://1234-4567`, then you would enter `api://1234-4567/app.scope.sso` as the scope. 
Token URL Template | URL Template for tokens, provided by your identity provider. <br />For example, `https://login.microsoftonline.com/common/oauth2/v2.0/token` <br />Note: Replace this with your Identity Provider URL. For Azure Apps, you need to replace the base URL with your Azure App URL. | On the app registration's **Overview** page, select **Endpoints**. This is listed as the **OAuth 2.0 token endpoint (v2)**.
Token URL Query String Template | Query string separator for the token URL. Usually a question mark `?`. | Use a question mark `?`.
Token Body Template | Template for the token body. | Use `code={Code}&grant_type=authorization_code&redirect_uri={RedirectUrl}&client_id={ClientId}&client_secret={ClientSecret}`.
Refresh URL Template | URL template for refresh. <br />For example, `https://login.microsoftonline.com/common/oauth2/v2.0/token` <br />Note: Replace this with your Identity Provider URL. For Azure Apps, you want to replace the base URL with your Azure App URL. | On the app registration's **Overview** page, select **Endpoints**. This is listed as the **OAuth 2.0 token endpoint (v2)**.
Scope List delimiter | The separator character for the scope list. Empty spaces (` `) are not supported in this field, but can be used in the **Scopes** field if required by the identity provider. In that case, use a comma (`,`) for this field, and spaces (` `) in the **Scopes** field. | Use a comma (`,`)
Authorization URL Template | URL template for authorization, defined by your identity provider. <br />For example, `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` | On the app registration's **Overview** page, select **Endpoints**. This is listed as the **OAuth 2.0 authorization endpoint (v2)**.
Authorization URL Query String Template | Query template for authorization, provided by your identity provider. <br />Keys in the query string template will vary depending on the identity provider. | Use `?client_id={ClientId}&response_type=code&redirect_uri={RedirectUrl}&scope={Scopes}&state={State}`.



## Test your configuration

After the setup steps are complete, save your configuration and test it by [creating a new topic using authentication](advanced-end-user-authentication.md).


## Remove the authentication configuration

1. Select **Manage** on the side navigation pane, and then go to the **Authentication** tab.

2. Select **Delete connection**.

If authentication is being used in a topic, you'll be warned that the connection can't be deleted until references to it are removed. A list of the affected topics will be provided in the warning note.

### Permanently remove the authentication configuration

> [!Note]
> Deleting the authentication information from the bot does not remove it from Azure Bot Service. If you want to clear the configuration from Azure Bot Service, you will need to contact your subscription owner, who will need to follow these steps. If these steps can't be followed, contact your Microsoft Support manager to have the issue resolved.

1. Sign in to the [Azure Portal](https://portal.azure.com).

1. Go to **Bot Services**.

1. Select the connection to be deleted.

1. Delete the connection.


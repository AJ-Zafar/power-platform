---
title: "Configure user authentication (contains video)"
description: "Configure authentication with your identity provider to enable users to sign in when having a bot conversation."
keywords: "Authentication, IdP, PVA, AAD"
ms.date: 01/25/2022

ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.reviewr: micchow
ms.custom: authentication, ceX
ms.collection: virtual-agent
---

# Configure end-user authentication in Power Virtual Agents

Select the version of Power Virtual Agents you're using here:

> [!div class="op_single_selector"]
>
> - [Power Virtual Agents web app](configuration-end-user-authentication.md)
> - [Power Virtual Agents app in Microsoft Teams](teams/configuration-end-user-authentication-teams.md)

You can configure a Power Virtual Agents bot to provide authentication capabilities, so users can sign in with an Azure Active Directory (AAD), or any [OAuth2 identity provider](/azure/active-directory/develop/v2-oauth2-auth-code-flow), such as a Microsoft account, or Facebook.

You can [add user authentication to a Power Virtual Agents bot](advanced-end-user-authentication.md) when editing a topic.

Power Virtual Agents supports the following authentication providers:

- Azure Active Directory v1
- Azure Active Directory v2
- Any identity provider that is compliant with the [OAuth2 standard](/azure/active-directory/develop/v2-oauth2-auth-code-flow).

> [!IMPORTANT]
> Changes to the authentication configuration will only take effect after you publish your bot. Make sure to plan ahead before making authentication changes to your bot.

>
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4n4G2]
>

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]

## Choose the best authentication option

Power Virtual Agents supports a set of different authentication options, each targeted to a different usage scenario.

To change the authentication settings, go to **Manage** on the side pane, and then go to the **Security** tab and select the **Authentication** card.

:::image type="content" source="media/configuration-end-user-authentication/security-authentication.png" alt-text="Screenshot of the Security page under Manage menu highlighting the Authentication card.":::

You will see the following three options to configure your authentication:

- No authentication
- Only for Teams
- Manual (for any channel including Teams)

:::image type="content" source="media/configuration-end-user-authentication/security-authentication-pane.png" alt-text="Screenshot of the Authentication pane showing the three authentication options.":::

### No Authentication

This configuration option provides no authentication for the bot. This is the standard configuration for bots that are not created from Teams.

### Only for Teams

> [!IMPORTANT]
> When choosing this option, only the Teams channel will be available. All other channels will be disabled and a warning will be displayed.

This configuration option is optimized for Teams channel usage. It automatically sets up Azure Active Directory (Azure AD) authentication for Teams without the need for any manual configuration.

It uses the Teams authentication itself to identify the user, meaning the user will not be prompted to sign-in while in Teams, unless there is a need for expanded scope. Only the Teams channel is available once this configuration is selected.

If you need other channels but still want authentication for your bot, you need to choose the **Manual** authentication option. This is the standard configuration for bots that are created from Teams.

The following variables will be available in the authoring canvas after the **Only for Teams** option is selected:

- `UserID`
- `UserDisplayName`

For more information about these variables and how to use them, see [Add end-user authentication to a Power Virtual Agents bot](advanced-end-user-authentication.md#authentication-variables).

`AuthToken` and `IsLoggedIn` variables are not available for this configuration option. If you need an authentication token, use the **Manual** option.

If you changed from **Manual** to **Only for Teams**, and your topics contained one of the variables `AuthToken` or `IsLoggedIn`, they will be displayed as **Unknown** variables after the change. Make sure to correct any topics with errors before publishing your bot.

### Manual (for any channel including Teams)

You can configure any Azure AD, Azure AD V2, or OAuth compatible identity provider with this option. The following variables will be available in the authoring canvas after manual authentication is configured:

- `UserID`
- `UserDisplayName`
- `AuthToken`
- `IsLoggedIn`

For more information about these variables and how to use them, see [Add end-user authentication to a Power Virtual Agents bot](advanced-end-user-authentication.md#authentication-variables).

Once the configuration is saved, make sure to publish your bot so the changes take effect.

> [!NOTE]
> Authentication changes only take effect after the bot is published.

## Required user sign in and bot sharing

**Require users to sign in** controls if a user needs to sign in before talking with the bot. This is only available to **Only for Teams** and **Manual** authentication options. It's highly recommended to turn this setting on when the bot contains sensitive information.

:::image type="content" source="media/configuration-end-user-authentication/auth-require-user-to-sign-in.PNG" alt-text="Screenshot of the Authentication pane showing require user to sign in.":::

Bots with this configuration turned **Off** won't ask users to sign in until they encounter a topic which requires them to do so.

When the **Require users to sign in** option is turned **On**, a new system topic called **Require users to sign in** is created. This topic is only relevant for the "Manual" authentication setting, as users are always authenticated on Teams.

This topic is automatically triggered for any user who talks to the bot without being authenticated. This topic is read-only and cannot be customized. If the user fails to sign in, this topic redirects the user to the **Escalate** system topic. You can see the topic by selecting **Go to the authoring canvas**.

### Control who can chat with bot in the organization

Your bot's **authentication option** and **Require user to sign in** combination determines whether you can [share the bot](admin-share-bots.md) to control who in your organization can chat with your bot or not.  Sharing a bot for collaboration is not impacted by the end-user authentication setting.

- **No authentication**: Any user who has a link to the bot (or can find it, for example, on your website) can chat with it. You cannot control which users can chat with the bot in your organization.

- **Only for Teams**. The bot will only work on [the Teams channel](publication-add-bot-to-microsoft-teams.md). This means the user will always be signed in, and therefore the **Require users to sign in** option will be enabled and can't be changed. You can control who can chat with the bot in your organization with bot sharing.

- **Manual (for any channel including Teams)**:
  
  - If your authentication setting is configured to **Manual**, and the service provider is either **Azure Active Directory** or **Azure Active Directory V2**, you can enable the **Require users to sign in** option to control who can chat with the bot in your organization via bot sharing.
  
  - If your authentication provider is set as **Generic OAuth 2**, you can toggle the **Require users to sign in** option. When turned on, a user who signs in can chat with the bot, but you cannot control which specific users are allowed to chat with the bot in your organization using bot sharing.

When a bot's authentication option can't control who can chat with the bot, selecting **Share** on the bot's homepage will inform you that anyone can chat with the bot.

:::image type="content" source="media/configuration-end-user-authentication/auth-allow-everyone-chat-with-bot.PNG" alt-text="Everyone in the organization can chat with bot because of authentication setting.":::

## Register a new app with your identity provider when using Manual (for any channel including Teams)

You need to register a new app with your identity provider and get a Client ID and Client Secret before you can configure a manual authentication in Power Virtual Agents. This section describes how to do that with the [Azure portal](https://portal.azure.com) for Azure AD. If you have a different identity provider, you should consult its setup instructions.

Make sure to configure the redirect URL to `https://token.botframework.com/.auth/web/redirect`, and that the assigned API permissions and scopes for the app are the same permissions you need the bot to access.

> [!IMPORTANT]
> Your app registration redirect URL must be `https://token.botframework.com/.auth/web/redirect`.  
> Ensure that the app has the correct API permissions and its related scopes.

### Use Azure Active Directory as your identity provider

#### Create an app registration

1. Sign in to the [Azure portal](https://portal.azure.com), using an admin account on the same tenant as your Power Virtual Agents chatbot.

1. Go to [App registrations](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade), either by selecting the icon or searching in the top search bar. Create a new **Application Registration**.

1. Select **New registration** and enter a name for the registration. It can be helpful to use the name of the bot you're enabling authentication for. For example, if your bot is called "Contoso sales help", you might name the app registration as "ContosoSalesReg" or something similar.

1. Select **Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)**

1. Leave the **Redirect URI** section blank for now, as you'll enter that information in the next steps. Select **Register**.

#### Add the redirect URL

1. Once the registration is completed, it will open to the **Overview** page. Go to **Authentication** and then select **Add a platform**.

1. On the **configure platforms** blade, select **Web**.

1. Under **Redirect URIs**, add `https://token.botframework.com/.auth/web/redirect`.

1. Under the **Implicit grant** section, select the **Id Tokens** and **Access Tokens** checkboxes.

1. Select **Configure** to confirm your changes.

#### Generate a client secret

1. Go to **Certificates & Secrets**.

1. Under the **Client secrets** section, select **New client secret**. Enter a description (one will be provided if you leave this blank), and select the expiry period. Select the shortest period that will be relevant for the life of your bot.

1. Select **Add** to create the secret. Take note of the secret's **Value** and store this in a temporary place (such as an open Notepad document), as you'll enter it in your bot's authentication settings.

## Configure Manual authentication

1. Sign in to Power Virtual Agents. If you're using Azure AD as your identity provider, ensure you log in on the same tenant where you created the app registration.

1. Confirm you've selected the bot for which you want to enable authentication by selecting the bot icon on the top menu and choosing the bot.

1. Select **Manage** on the side pane, and then go to the **Security** tab and select the **Authentication** card.

    :::image type="content" source="media/configuration-end-user-authentication/auth-manage-sm.png" alt-text="Screenshot of the Authentication under Manage left bar menu.":::

1. Select **Manual (for any channel including Teams)**.

    :::image type="content" source="media/configuration-end-user-authentication/auth-select-manual.png" alt-text="Select the Manual authentication option.":::

1. [Enter the information as described for each of the fields](#manual-authentication-fields). The information required depends on your setup and provider. If you have questions about the required information, contact your administrator or identity provider.

1. Click **Save** to finish the configuration.

### Manual authentication fields

The following are all possible fields you'll see when configuring manual authentication. However, depending on your choice for [service provider](#service-provider), some fields won't be present.

#### Service provider

The service provider you want to use for authentication.

If you're using Azure AD as a provider, we recommend using "Azure Active Directory" or "Azure Active Directory V2" for easier configuration.

For more information, see [OAuth generic providers](/azure/bot-service/bot-builder-concept-identity-providers?view=azure-bot-service-4.0&tabs=adv1%2Cga2&preserve-view=true) documentation.  

#### Tenant ID

Your Azure AD tenant ID. Refer to [Use an existing Azure AD tenant](/azure/active-directory/develop/quickstart-create-new-tenant#use-an-existing-azure-ad-tenant) to learn how to find your tenant ID.

#### Client ID

Your client ID obtained from the identity provider.

To find this information when using Azure AD, go to the app registration's **Overview** page as **Application (client) ID**.

#### Client secret

Your client secret obtained from the identity provider registration.

To find this information when using Azure AD, generate a new client secret. If you navigate away from the **Certificates & secrets** page, the secret's **Value** will be obfuscated and you'll need to create a new one.

#### Token exchange URL (required for SSO)

This is an optional field used when [configuring single sign-on](configure-sso.md).

#### Refresh URL query string template

Refresh URL query string separator for the token URL. Usually a question mark (`?`).

#### Refresh body template

Template for the refresh body.

When using Azure AD, use `refresh_token={RefreshToken}&redirect_uri={RedirectUrl}&grant_type=refresh_token&client_id={ClientId}&client_secret={ClientSecret}`.

#### Scopes

List of [scopes](/azure/active-directory/develop/developer-glossary#scopes) you want authenticated users to have once signed in. 

Use spaces to separate multiple scopes, only set necessary scopes, and follow the [Least privilege access control principle](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models).

To find this information when using Azure AD, go to the **API permissions** page under the **API / Permissions** name section.

For custom scopes defined by an exposed API, you'll need to use the full URI, including the exposed Application ID URI. On the **Expose an API** page, prepend the **Application ID URI** and ending slash (`/`) to the scope name. For example, if your custom scope name is `app.scope.sso`, and the **Application ID URI** is `api://1234-4567`, then you would enter `api://1234-4567/app.scope.sso` as the scope.

#### Token URL template

URL Template for tokens, provided by your identity provider. For example, `https://login.microsoftonline.com/common/oauth2/v2.0/token`. For Azure Apps, you need to replace the base URL with your Azure App URL.

To find this information when using Azure AD, go to the app registration's **Overview** page and then selecting **Endpoints**. This is listed as the **OAuth 2.0 token endpoint (v2)**.

#### Token URL query string template

Query string separator for the token URL. Usually a question mark (`?`).

When using Azure AD, use a question mark (`?`).

#### Token body template

Template for the token body.

When using Azure AD, use `code={Code}&grant_type=authorization_code&redirect_uri={RedirectUrl}&client_id={ClientId}&client_secret={ClientSecret}`.

#### Refresh URL template

URL template for refresh. For example, `https://login.microsoftonline.com/common/oauth2/v2.0/token`. For Azure Apps, you want to replace the base URL with your Azure App URL.

To find this information when using Azure AD, go to the app registration's **Overview** page and then select **Endpoints**. This is listed as the **OAuth 2.0 token endpoint (v2)**.

#### Scope list delimiter

The separator character for the scope list. 

Empty spaces (` `) are not supported in this field, but can be used in the **Scopes** field if required by the identity provider. In that case, use a comma (`,`) for this field, and spaces (` `) in the **Scopes** field.

When using Azure AD, use a comma (`,`).

#### Authorization URL template

URL template for authorization, defined by your identity provider. For example, `https://login.microsoftonline.com/common/oauth2/v2.0/authorize`

To find this information when using Azure AD, go to the app registration's **Overview** page and then selecting **Endpoints**. This is listed as the **OAuth 2.0 token endpoint (v2)**.

#### Authorization URL query string template

Query template for authorization, provided by your identity provider. 

Keys in the query string template will vary depending on the identity provider.

When using Azure AD, use `?client_id={ClientId}&response_type=code&redirect_uri={RedirectUrl}&scope={Scopes}&state={State}`.

## Test your configuration

After the setup steps are complete, save your configuration and test it by [creating a new topic using authentication](advanced-end-user-authentication.md).

## Remove the authentication configuration

1. Select **Manage** on the side pane, and then go to the **Security** tab and select the **Authentication** card.
1. Select **No authentication**.
1. Publish the bot.

If authentication variables are being used in a topic, they will become **Unknown** variables. Go to the Topics page to see which topics have errors and fix them before publishing.

[!INCLUDE[footer-include](includes/footer-banner.md)]

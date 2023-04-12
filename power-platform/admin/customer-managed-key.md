---
title: Manage your customer-managed encryption key in Power Platform 
description: Learn how to manage your encryption key. 
author: paulliew
ms.author: paulliew
ms.reviewer: matp, ratrtile
ms.topic: how-to
ms.date: 03/20/2023
ms.custom: template-how-to
---
# Manage your customer-managed encryption key (preview)

[!INCLUDE [cc-beta-prerelease-disclaimer](../includes/cc-beta-prerelease-disclaimer.md)]

Customers have data privacy and compliance requirements to secure their data by encrypting their data at-rest. This secures the data from exposure in an event where a copy of the database is stolen. With data encryption at-rest, the stolen database data is protected from being restored to a different server without the encryption key.

All customer data stored in Power Platform is encrypted at-rest with strong Microsoft-managed encryption keys by default. Microsoft stores and manages the database encryption key for all your data so you don't have to. However, Power Platform provides this customer-managed encryption key (CMK) for your added data protection control where you can self-manage the database encryption key that is associated with your Microsoft Dataverse environment. This allows you to rotate or swap the encryption key on demand, and also allows you to prevent Microsoft's access to your customer data when you revoke the key access to our services at any time.

> [!IMPORTANT]
> This is a preview feature.
>
> These encryption key operations are available with this release of customer-managed key (CMK):
>
> - Create a RSA (RSA-HSM) key from your Azure Key vault.
> - Create a Power Platform enterprise policy for your key.
> - Grant the Power Platform enterprise policy permission to access your key vault.
> - Grant the Power Platform service admin to read the enterprise policy.
> - Grant the Power Platform service admin to read the enterprise policy.
> - Apply encryption key to your environment.
> - Revert/remove environment’s CMK encryption to Microsoft-managed key.
> - Change key by creating a new enterprise policy, removing the environment from CMK and re-apply CMK with new enterprise policy.
> - Lock CMK environments by revoking CMK key vault and/or key permissions.
> - Migrate [bring-your-own-key (BYOK)](cmk-migrate-from-byok.md) environments to CMK by applying CMK key.
>
> This feature will be available in all public cloud regions March 2023.
>
> There's no additional Power Platform licensing requirement during this preview. When this feature becomes generally available, there will be a licensing requirement.
> 
Currently, all your customer data stored *only* in the following apps and services can be encrypted with customer-managed key:

- Dataverse (Custom solutions and Microsoft services)
- [Power Automate](https://learn.microsoft.com/power-automate/customer-managed-keys) ** 
- Chat for Dynamics 365
- [Dynamics 365 Sales](/dynamics365/sales/sales-gdpr-faqs#can-the-dynamics-365-sales-data-be-encrypted-using-customer-managed-encryption-key-cmk)
- Dynamics 365 Customer Service
- Dynamics 365 Customer Insights
- Dynamics 365 Omnichannel
- Dynamics 365 Commerce (Finance and operations)
- Dynamics 365 Field Service
- Dynamics 365 Retail
- Dynamics 365 Finance (Finance and operations)
- Dynamics 365 Intelligent Order Management (Finance and operations)
- Dynamics 365 Project Operations (Finance and operations)
- Dynamics 365 Supply Chain Management (Finance and operations)
- Dynamics 365 Fraud Protection (Finance and operations)

   > [!NOTE]
   > 
   > ** Power Automate - When you apply CMK to an environment that has Flows, the Flows data continues to be encrypted with Microsoft-managed key. For Flows created in an environment that didn't have Flows when CMK was applied will be encrypted with CMK. [Power Automate CMK](https://learn.microsoft.com/power-automate/customer-managed-keys).
   > 
   > During preview, the connection settings for connectors will continue to be encrypted with a Microsoft-managed key.
   >
   > Contact a representative for services not listed above for information about customer-managed key support.

Environments with finance and operations apps where [Power Platform integration is enabled](/dynamics365/fin-ops-core/dev-itpro/power-platform/enable-power-platform-integration) can also be encrypted. Finance and operations environments without Power Platform integration will continue to use the default Microsoft managed key to encrypt data. More information: [Encryption in finance and operations apps](/dynamics365/fin-ops-core/dev-itpro/sysadmin/customer-managed-keys)

:::image type="content" source="media/cmk-power-platform-diagram.png" alt-text="Customer-managed encryption key in the Power Platform":::

## Introduction to customer-managed key

With customer-managed key, administrators can provide their own encryption key from their own Azure Key Vault to the Power Platform storage services to encrypt their customer data. Microsoft doesn't have direct access to your Azure Key Vault. For Power Platform services to access the encryption key from your Azure Key Vault, the administrator creates a Power Platform enterprise policy, which references the encryption key and grants this enterprise policy access to read the key from your Azure Key Vault.

The Power Platform service administrator can then add Dataverse environments to the enterprise policy to start encrypting all the customer data in the environment with your encryption key. Administrators can change the environment's encryption key by creating another enterprise policy and add the environment (after removing it) to the new enterprise policy. In the event that the environment no longer needs to be encrypted using your customer-managed key, the administrator can remove the Dataverse environment from the enterprise policy to revert the data encryption back to Microsoft-managed key.

The administrator can lock the customer-managed key environments by revoking key access from the enterprise policy and unlock the environments by restoring the key access. More information: [Lock environments by revoking key vault and/or key permission access (preview)](cmk-lock-unlock.md)

To simplify the key management tasks, the tasks are broken down into three main areas:

1. Create encryption key.
1. Create enterprise policy and grant access.
1. Manage environment's encryption.

> [!WARNING]
> When environments are locked, they can't be accessed by anyone, including Microsoft support. Environments that are locked become disabled and data loss can occur.

## Understand the potential risk when you manage your key

As with any business critical application, personnel within your organization who have administrative-level access must be trusted. Before you use the key management feature, you should understand the risk when you manage your database encryption keys. It's conceivable that a malicious administrator (a person who is granted or has gained administrator-level access with intent to harm an organization's security or business processes) working within your organization might use the manage keys feature to create a key and use it to lock your environments in the tenant.

Consider the following sequence of events.

The malicious key vault administrator creates a key and an enterprise policy on the Azure portal. The Azure Key Vault administrator goes to Power Platform admin center, and add environments to the enterprise policy. The malicious administrator then returns to the Azure portal and revokes key access to the enterprise policy thus locking all the environments. This causes business interruptions as all the environments become inaccessible, and if this event isn't resolved, that is, the key access restored, the environment data can be potentially lost.

> [!NOTE]
>
> - Azure Key Vault has built-in safeguards that assist in restoring the key, which require the **Soft Delete** and **Purge protection** key vault settings enabled.
> - Another safeguard to be considered is to make sure that there is separation of tasks where the Azure Key Vault administrator isn't granted access to the Power Platform admin center.

### Separation of duty to mitigate the risk

This section describes the customer-managed key feature duties that each admin role is responsible for. Separating these tasks helps mitigate the risk involved with customer-managed keys.

#### Azure Key Vault and Power Platform/Dynamics 365 service admin tasks

To enable customer-managed keys, first the key vault administrator creates a key in the Azure key vault and creates a Power Platform enterprise policy. When the enterprise policy is created, a special Azure Active Directory (Azure AD) managed identity is created. Next, the key vault administrator returns to the Azure key vault and grants the enterprise policy/managed identity access to the encryption key.

The key vault administrator then grants the respective Power Platform/Dynamics 365 service admin read access to the enterprise policy. Once read permission is granted, the Power Platform/Dynamics 365 service admin can go to the Power Platform Admin Center and add environments to the enterprise policy. All added environments customer data is then encrypted with the customer-managed key linked to this enterprise policy.

##### Prerequisites

- An Azure subscription that includes Azure Key Vault.
- Global tenant admin or an  Azure AD with contributor permission to the Azure AD subscription and permission to create an Azure Key Vault and key. This is required to set up the key vault.

##### Create the key and grant access overview

The Azure Key Vault administrator performs these tasks in Azure.

1. Create an Azure paid subscription and Key Vault. Ignore this step if you already have a subscription that includes Azure Key Vault.
1. Go to the Azure Key Vault service, and create a key. More information: [Create a key in the key vault](#create-a-key-in-the-key-vault)
1. Enable the Power Platform enterprise policies service for your Azure subscription. Do this only once. More information: [Enable the Power Platform enterprise policies service for your Azure subscription](#enable-the-power-platform-enterprise-policies-service-for-your-azure-subscription)
1. Create a Power Platform enterprise policy. More information: [Create enterprise policy](#create-enterprise-policy)
1. Grant enterprise policy permissions to access the key vault. More information: [Grant enterprise policy permissions to access key vault](#grant-enterprise-policy-permissions-to-access-key-vault)
1. Grant Power Platform and Dynamics 365 administrators permission to read the enterprise policy. More information: [Grant the Power Platform admin privilege to read enterprise policy](#grant-the-power-platform-admin-privilege-to-read-enterprise-policy)

#### Power Platform/Dynamics 365 service admin Power Platform admin center tasks

##### Prerequisite

- Power Platform administrator must be assigned to either the Power Platform or Dynamics 365 Service administrator Azure AD role.

##### Manage environment's encryption in Power Platform admin center

The Power Platform administrator manages customer-managed key tasks related to the environment in Power Platform admin center.

1. Add the Power Platform environments to the enterprise policy to encrypt data with the customer-managed key. More information: [Add an environment to the enterprise policy to encrypt data](#add-an-environment-to-the-enterprise-policy-to-encrypt-data)
1. Remove environments from enterprise policy to return encryption to Microsoft managed key. More information: [Remove environments from policy to return to Microsoft managed key](#remove-environments-from-policy-to-return-to-microsoft-managed-key)
1. Change the key by removing environments from the old enterprise policy and adding environments to a new enterprise policy. More information: [Change the environment's encryption key](#change-the-environments-encryption-key)
1. Migrate from BYOK. If you are using the earlier self-managed encryption key feature, you can migrate your key to customer managed key. More information: [Migrate bring-your-own-key environments to customer-managed key (preview)](cmk-migrate-from-byok.md)

## Create encryption key and grant access

### Create an Azure paid subscription and key vault

In Azure, perform the following steps:

1. Create a Pay-as-you-go or its equivalent Azure subscription. This step isn't needed if the tenant already has a subscription.
1. Create a resource group. More information: [Create resource groups](/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)
1. Create a key vault using the paid subscription that includes soft-delete and purge protection with the resource group you created in the previous step.
   > [!IMPORTANT]
   >
   > - To ensure that your environment is protected from accidental deletion of the encryption key, the key vault must have soft-delete and purge protection enabled. You won’t be able to encrypt your environment with your own key without enabling these settings. More information: [Azure Key Vault soft-delete overview](/azure/key-vault/general/soft-delete-overview) More information: [Create a key vault using Azure portal](/azure/key-vault/general/quick-create-portal)
   > - During preview, your Azure Key Vault must be accessible from an unrestricted internet connection. It can't be behind your firewall or vNet.

#### Create a key in the key vault

1. Make sure you've met the [prerequisites](#prerequisites).
1. Go to the [Azure portal](https://ms.portal.azure.com/) > **Key Vault** and locate the key vault where you want to generate an encryption key.
1. Verify the Azure key vault settings:
   1. Select **Properties** under **Settings**.
   1. Under **Soft-delete**, set or verify that it's set to **Soft delete has been enabled on this key vault** option.
   1. Under **Purge protection**, set or verify that **Enable purge protection (enforce a mandatory retention period for deleted vaults and vault objects)** is enabled.
   1. If you made changes, select **Save**.

   :::image type="content" source="media/cmk-key-vault-purge-protect.png" alt-text="Enable purge protection on the key vault":::
1. Create or import a key that has these properties:
   1. On the **Key Vault** properties pages, select **Keys**.
   1. Select **Generate/Import**.
   1. On the **Create a key** screen set the following values, and then select **Create**.
      - **Options**: **Generate**
      - **Name**: Provide a name for the key
      - **Key type**: **RSA** or **RSA-HSM**
      - **RSA key size**: **2048**

### Enable the Power Platform enterprise policies service for your Azure subscription

Register Power Platform as a resource provider. You only need to do this task once.

1. Sign in to the [Azure portal](https://ms.portal.azure.com/) and go to **Subscription** > **Resource providers**.
1. In the list of **Resource providers**, search for **Microsoft.PowerPlatform**, and **Register** it.

### Enable Power Platform enterprise policies service

1. Azure CLI is required on your local machine. [Download Azure CLI](https://aka.ms/InstallAzureCliWindows).
1. Run the downloaded Azure cli.MSI.
1. Download the ARMClient: [ARMClient/README.md projectkudu/ARMClient GitHub](https://github.com/projectkudu/ARMClient/blob/master/README.md).
1. Start PowerShell and run the ARMClient to sign into your Azure subscription.
   > [!NOTE]
   > Depending on where you are running the command from, you could either use armclient.login from PowerShell, or armclient.azlogin from Azure CLI.
1. Enable `enterprisePoliciesPreview` feature for your subscription. Replace {subscriptionId} with your *subscriptionId* number. More information: [Find your Azure subscription](/azure/azure-portal/get-subscription-tenant-id#find-your-azure-subscription)

   `PS C:\> ARMClient.exe POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Features/providers/Microsoft.PowerPlatform/features/enterprisePoliciesPreview/register?api-version=2014-08-01-preview`

## Create enterprise policy

1. Install PowerShell MSI. More information: [Install PowerShell on Windows, Linux, and macOS](https://ms.portal.azure.com/#create/Microsoft.Template)
1. After the PowerShell MSI is installed, go back to [Deploy a custom template](https://ms.portal.azure.com/#create/Microsoft.Template) in Azure.
1. Select the **Build your own template in the editor** link.
1. Copy the JSON template into a text editor such as Notepad. More information: [Enterprise policy json template](#enterprise-policy-json-template)
1. Replace the values in the JSON template for: *EnterprisePolicyName*, *location where EnterprisePolicy needs to be created*, *keyVaultId*, *keyName*, and *keyVersion*. More information: [Field definitions for json template](#field-definitions-for-json-template)
1. Copy the updated template from your text editor then paste it into the **Edit template** of the **Custom deployment** in Azure, and select **Save**.
   :::image type="content" source="media/cmk-keyvault-template.png" alt-text="Azure key vault template":::
1. Select a **Subscription** and **Resource group** where the enterprise policy is to be created.
1. Select **Review + create**, and then select **Create**.

A deployment is started. When it's done, the enterprise policy is created.
 > [!NOTE]
   > During preview, you can only create up to two enterprise policies.

### Enterprise policy json template

```json
 {
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.PowerPlatform/enterprisePolicies",
            "apiVersion": "2020-10-30",
            "name": {EnterprisePolicyName},
            "location": {location where EnterprisePolicy needs to be created},
            "kind": "Encryption",
            "identity": {
                "type": "SystemAssigned"
            },
            "properties": {
                "lockbox": null,
                "encryption": {
                    "state": "Enabled",
                    "keyVault": {
                        "id": {keyVaultId},
                        "key": {
                            "name": {keyName},
                            "version": {keyVersion}
                        }
                    }
                },
                "networkInjection": null
            }
        }
    ]
   }
```

### Field definitions for JSON template

- **name**. Name of the enterprise policy. This is the name of the policy that appears in Power Platform admin center.
- **location**. One of the following. This is the location of the enterprise policy and it must correspond with the Dataverse environment’s region:

  - `"unitedstates"`
  - `"southafrica"`
  - `"switzerland”`
  - `"germany"`
  - `"unitedarabemirates"`
  - `"france"`
  - `"uk”`
  - `"japan"`
  - `"india"`
  - `"canada"`
  - `"southamerica"`
  - `"europe"`
  - `"asia"`
  - `"australia"`
  - `"korea"`
  - `"norway"`
  - `"singapore"`
- Copy these values from your key vault properties in the Azure portal:
  - **keyVaultId**: Go to **Key vaults** > select your key vault > **Overview**. Next to **Essentials** select **JSON View**. Copy the **Resource ID** to the clipboard and paste the entire contents into your JSON template.
  - **keyName**: Go to **Key vaults** > select your key vault > **Keys**. Notice the key **Name** and type the name into your JSON template.
  - **keyVersion**: Go to **Key vaults** > select your key vault > **Keys**. Select the key, copy the **CURRENT VERSION** number, and then paste it into your JSON template.

### Grant enterprise policy permissions to access key vault

Once the enterprise policy is created, the key vault administrator grants the enterprise policy’s managed identity access to the encryption key.

1. Sign into the [Azure portal](https://ms.portal.azure.com/) and go to **Key vaults**.
1. Select the key vault where the key was assigned to the enterprise policy.
1. Select the **Access policies** tab, and then select **+ Create**.
1. Under the **Key permissions** section,
   Key Management Operations, select this option:
   - **Get**

   Cryptographic Operations, select these options:
   - **Unwrap key**
   - **Wrap key**
   :::image type="content" source="media/cmk-keyvault-access-policy.png" alt-text="Key vault add access policy":::
1. Select **Next**.
1. On the **Principal** page, enter your Enterprise policy and select it.
1. Select **Next**, and then select **Create**.

### Grant the Power Platform admin privilege to read enterprise policy

Administrators who have Azure global, Dynamics 365, and Power Platform administration roles can access the Power Platform admin center to assign environments to the enterprise policy. To access the enterprise policies, the global admin with Azure key vault access is required to grant the **Reader** role to the Power Platform admin. Once the **Reader** role is granted, the Power Platform administrator will be able to view the enterprise policies on the Power Platform admin center.  

> [!NOTE]
> Only Power Platform and Dynamics 365 administrators who are granted the reader role to the enterprise policy can add an environment to the policy. Other Power Platform or Dynamics 365 administrators might be able to view the enterprise policy but they'll get an error when they try to **Add environment** to the policy.

### Grant reader role to a Power Platform administrator

1. Sign into the [Azure portal](https://ms.portal.azure.com/).
1. Copy the Power Platform or Dynamics 365 admin’s object ID. To do this:
   1. Go to the **Users** area in Azure.
   1. In the **All users** list, find the user with Power Platform or Dynamics 365 admin permissions using **Search users**.
   1. Open the user record, on the **Overview** tab copy the user’s **Object ID**. Paste this into a text editor such as NotePad for later.
1. Copy the enterprise policy resource ID. To do this:
   1. Go to **Resource Graph Explorer** in Azure.
   1. Enter `microsoft.powerplatform/enterprisepolicies` in the **Search** box, and then select the **microsoft.powerplatform/enterprisepolicies** resource.
   1. Select **Run query** on the command bar. A list of all the Power Platform enterprise policies is displayed.
   1. Locate the enterprise policy where you want to grant access.
   1. Scroll to the right of the enterprise policy and select **See details**.
   1. On the **Details** page, copy the **id**.
1. Start [Azure Cloud Shell](https://azure.microsoft.com/get-started/azure-portal/cloud-shell/#features), and run the following command replacing *objId* with the user’s object ID and *EP Resource Id* with the `enterprisepolicies` ID copied in the previous steps:
    `New-AzRoleAssignment -ObjectId { objId} -RoleDefinitionName Reader -Scope {EP Resource Id}`

## Manage environment's encryption

To manage the environment's encryption, you need the following permission:

- Azure AD active user who has a Power Platform and/or Dynamics 365 admin security role.
- Azure AD user who has either a global tenant admin, Power Platform or Dynamics 365 service admin role.

The key vault admin notifies the Power Platform admin that an encryption key and an enterprise policy were created and provides the enterprise policy to the Power Platform admin. To enable the customer-managed key, the Power Platform admin assigns their environments to the enterprise policy. Once the environment is assigned and saved, Dataverse initiates the encryption process to set all the environment data, and encrypt it with the customer-managed key.

### Add an environment to the enterprise policy to encrypt data

> [!IMPORTANT]
> The environment will be disabled when it is added to the enterprise policy for data encryption.

1. Sign into the [Power Platform admin center](https://admin.powerplatform.microsoft.com), and go to **Policies** > **Enterprise policies**.
1. Select a policy, and then on the command bar select **Edit**.
1. Select **Add environments**, select the environment you want, and then select **Continue**.
   :::image type="content" source="media/cmk-add-environments-enterprise-policy.png" alt-text="Add environment to enterprise policy on Power Platform admin center":::
1. Select **Save**, and then select **Confirm**.

> [!IMPORTANT]
>
> - The environment is disabled temporarily during this process and re-enabled to allow users to access while the encryption process continues. It can take up to a day to complete the encryption process.
> - Only environments that are in the same region as the enterprise policy are displayed in the **Add environments** list.

> [!NOTE]
> During preview, you can only add sandbox environments.

### Remove environments from policy to return to Microsoft managed key

Follow these steps if you want to return to a Microsoft managed encryption key.

> [!IMPORTANT]
> The environment will be disabled when it is removed from the enterprise policy to return data encryption using the Microsoft managed key.

1. Sign into the [Power Platform admin center](https://admin.powerplatform.microsoft.com), and go to **Policies** > **Enterprise policies**.
1. Select the **Environment with policies** tab, and then find the environment you want to remove from customer-managed key.
1. Select the **All policies** tab, select the environment you verified in step 2, and then select **Edit policy** on the command bar.
   :::image type="content" source="media/cmk-ppac-remove-env-policy.png" alt-text="Remove an environment from customer-managed key":::
1. Select **Remove environment** on the command bar, select the environment you want to remove, and then select **Continue**.
1. Select **Save**.

### Change the environment's encryption key

To rotate your encryption key, create a new key and a new enterprise policy. You can then change the enterprise policy by removing the environments and then adding the environments to the new enterprise policy.
 > [!NOTE]
   > During preview, using **New key version** and setting **Rotation policy** to rotate your encryption key is not supported. Activating new key version and disabling the current version will lock the environment.

1. In [Azure portal](https://ms.portal.azure.com/), create a new key and a new enterprise policy. More information:  [Create the key and grant access](#create-the-key-and-grant-access-overview) and [Create an enterprise policy](#create-enterprise-policy)
1. Once the new key and enterprise policy are created, go to **Policies** > **Enterprise policies**.
1. Select the **Environment with policies** tab, and then find the environment you want to remove from customer-managed key.
1. Select the **All policies** tab, select the environment you verified in step 2, and then select **Edit policy** on the command bar.
   :::image type="content" source="media/cmk-ppac-remove-env-policy.png" alt-text="Remove an environment from customer-managed key":::
1. Select **Remove environment** on the command bar, select the environment you want to remove, and then select **Continue**.
1. Select **Save**.
1. Repeat steps 2-6 until all environments in the enterprise policy have been removed.

  > [!IMPORTANT]
  > The environment will be disabled when it is removed from the enterprise policy to revert the data encryption to Microsoft managed key.

1. Once all the environments are removed, from the Power Platform admin center go to **Enterprise policies**.
1. Select the new enterprise policy, and then select **Edit policy**.
1. Select **Add environment**, select the environments that you want to add, and then select **Continue**.

> [!IMPORTANT]
> The environment will be disabled when it's added to the new enterprise policy.

### View the list of encrypted environments

1. Sign into the [Power Platform admin center](https://admin.powerplatform.microsoft.com), and go to **Policies** > **Enterprise policies**.
1. On the **Enterprise policies** page, select the **Environments with policies** tab. The list of environments that were added to enterprise policies are displayed.

> [!NOTE]
> During preview, there might be situations where the **Environment status** or the **Encryption status** show a **Failed** status. When this occurs, submit a Microsoft Support request for help.

## Next steps

[About Azure Key Vault](/azure/key-vault/general/overview)

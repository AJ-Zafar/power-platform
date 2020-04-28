---
title: "Create conversational bots in Power Virtual Agents"
description: "You can easily create new bots using the bot-builder wizard in Power Virtual Agents."
keywords: ""
ms.date: 4/15/2020
ms.service:
  - dynamics-365-ai
ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.custom: onboarding
ms.collection: virtual-agent
---

# Create your first Power Virtual Agents bot

To help get you started and tailor the bot to your specific needs, Power Virtual Agents lets you create a bot using built-in content building blocks containing topics, trigger phrases, and pre-authored bot conversations.

**Create your first bot:**

1. Go to [Power Virtual Agents marketing page](https://go.microsoft.com/fwlink/?linkid=2106332) and select **Try Preview**. Sign in using your work account.

   If you don't have a license, you can sign up for one by following the prompts.
    
1. In the **Create a new bot** dialog box, enter a name for your bot. This can be something generic to your company or specific to the scenario you are tailoring your bot to.

   ![New bot dialog](media/create-new-bot-screen.png)

1. Your bot is created in the [default Power Apps environment for your organization or tenant](environments-first-run-experience.md). Usually this is sufficient, but if necessary you can choose a different environment by selecting **More options** to see a list of available environments.
  
1. Select **Create** to begin the bot-building process, which can take up to 15 minutes for the first bot you create in an environment. Subsequent bots take less than a few minutes to create.


During the building process, you'll see a notification at the top of the Power Virtual Agents portal indicating that the bot is still being built.

You can navigate to some tabs during this period, after which the banner will change and you'll have full access to the Power Virtual Agents portal. 
 

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]


## Create additional bots

If you have already created a bot, you can create a new bot by selecting the icon on the title bar to open the **Bots** pane and then selecting **New bot**.

   ![New bot icon in title bar](media/first-bot-new.png)

## Delete a bot

You can delete bots to remove them from your environment. 

1. Select the bot icon on the top menu bar, and then select the bot you want to delete.

2. Select the **Settings** icon on the top menu bar, then select **General settings**.

   ![General settings](media/general-settings.png)

3. Select **Delete bot**.

You'll be asked to confirm the deletion of the bot, after which all bot content is immediately deleted.

If your license has expired, you can delete your bot(s) by selecting **Permanently delete your bots**.

## Known issues with creating a bot

When you are creating your bot, you might encounter the following issues.

### Insufficient permissions for the selected environment

In this case, you see this error: "You do not have permissions to any environments. Please get access from an administrator."

You will need to [create a new environment](environments-first-run-experience.md). Use that environment to create your bot.

### The environment I created does not show up in the drop-down menu of Power Virtual Agents

Your environment might not show up in the drop-down menu due to one of the following:
 - The environment doesn't have a database created. To resolve this, go to [admin.powerplatform.com](https://admin.powerplatform.com) to [create a database in your environment](/power-platform/admin/create-database).
 - The environment is created in an unsupported region. Learn more about [supported data locations](data-location.md).

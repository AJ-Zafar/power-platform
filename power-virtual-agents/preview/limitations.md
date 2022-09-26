---
title: "Limitations of the Power Virtual Agents preview (Preview)"
description: "Existing features that aren't supported in the Power Virtual Agents preview."
ms.date: 10/10/2022
ms.topic: overview
author: v-alarioza
ms.author: v-alarioza
manager: shellyha
ms.collection: virtual-agent
searchScope:
  - "Power Virtual Agents"
---

# Limitations (Preview)

[!INCLUDE [Preview disclaimer](../includes/public-preview-disclaimer.md)]

The Power Virtual Agents preview currently doesn't support some of the existing features you're used to in Power Virtual Agents, and available features have some limitations.

## Limitations of current features

- Preview bots are only available in English.
- When sharing a bot, any Power Automate flows used in the conversation logic will not be shared. You'll have to manually share those flows if desired.  
- [Message variations](authoring-create-edit-topics.md#message-variations) do not vary in a single conversation. You'll only see differences between different conversations.
- The **Track between topics** toggle in the [test bot pane](authoring-test-bot.md) doesn't work.
- Quick replies are not read over the [Telephony channel](publication-connect-bot-to-telephony.md).
- While co-authoring a bot with another user is possible, it's not a fully supported feature.
- Global variables that are configured to persist across sessions will not work for all channels and are specifically not supported for the [Telephony channel](publication-connect-bot-to-telephony.md).
- Default values in multiple-choice questions don't work.
- Passing uninitialized variables to [flows](advanced-flow.md) causes an error.

## Upcoming releases

- Skills
- Security options for your bot
- Channels (including Teams and Facebook Messenger)
- Agent transfers
- AI capabilities
- Authoring in Teams app
- Bots setup with application lifecycle management (ALM)
- Importing and exporting bots
- Customization of system fallback topic

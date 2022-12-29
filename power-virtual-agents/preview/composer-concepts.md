---
title: Key concepts for Bot Framework Composer users (preview)
description: Overview of how key concepts in Microsoft Bot Framework Composer translate to concepts in Power Virtual Agents preview.
ms.date: 12/07/2022
ms.topic: conceptual
author: v-alarioza
ms.author: v-alarioza
manager: shellyha
ms.custom: fundamentals, ceX, intro-internal, bap-template
ms.collection: virtual-agent
searchScope:
  - "Power Virtual Agents"
ms.service: power-virtual-agents
---

# Key concepts for Bot Framework Composer users (preview)

[!INCLUDE [Preview disclaimer](includes/public-preview-disclaimer.md)]

If you're used to designing bots in Bot Framework Composer, you'll find that some things are different in the Power Virtual Agents preview, and some things are similar. The following table lists some key concepts in Composer and where to find similar concepts in Power Virtual Agents preview.

<!-- best viewed without wordwrap -->
| Composer concept               | Power Virtual Agents concept                        | Description                                                                                                                                                                                                                                        |
| :----------------------------- | :-------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dialogs and triggers           | [Topics][]                                          | Use topics to organize conversation flow or paths. A topic has a set of _trigger phrases_ that indicate when the bot should start the topic and a set of _nodes_ that describe the topic's conversation path.                                      |
| Intents                        | [Trigger phrases][]                                 | Add trigger phrases to a topic for the phrases, keywords, and questions that a customer is likely to type related to a specific issue. Power Virtual Agents uses natural language understanding to parse what a customer types and find the most appropriate topic. |
| Actions and prompts            | [Nodes][]                                           | Use nodes, such as messages, questions, and conditional branches, on the authoring canvas to create a topic's conversation path.                                                                                                           |
| Bot response variation         | [Response variations][] and [question variations][] | Use response and question variations to add variety to your bot's messages and questions.                                                                                                                                                          |
| Suggested actions              | [Quick replies][]                                   | Use quick replies to provide default reply options to the customer.                                                                                                                                                                                    |
| Entities                       | [Entities][]                                        | Define and use entities to extract semantic information from what a customer types.                                                                                                                                                                   |
| State, storage, and properties | [Variables][]                                       | Use variables to track state.                                                                                                                                                                                                                      |
| Formulas and expressions       | [Power Fx][]                                        | Use Power Fx to create expressions.                                                                                                                                                                                                                |

[Entities]: advanced-entities-slot-filling.md
[Nodes]: authoring-create-edit-topics.md
[Power Fx]: advanced-power-fx.md
[question variations]: authoring-send-message.md#use-message-variations
[Quick replies]: authoring-send-message.md#use-quick-replies
[Response variations]: authoring-send-message.md#use-message-variations
[Topics]: authoring-create-edit-topics.md
[Trigger phrases]: authoring-create-edit-topics.md
[Variables]: authoring-variables.md

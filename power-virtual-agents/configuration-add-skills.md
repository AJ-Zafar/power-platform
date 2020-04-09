---
title: "Extend your bot using Bot Framework Skills"
description: "Skills extend your bot's conversational capabilities by automating a series of actions within a topic. Skills enable the bot to book an appointment, send a confirmation email, manage tasks, and more."
keywords: "extensibility, integration, extend bot, bot framework, skills, custom capabilities"
ms.date: 4/8/2020
ms.service:
  - dynamics-365-ai
ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.custom: "azure, extend"
ms.collection: virtual-agent
---

# Extend your bot using Bot Framework Skills

Power Virtual Agents enables you to extend your bot using [Bot Framework Skills](/azure/bot-service/skills-conceptual?view=azure-bot-service-4.0). If you have already built and deployed bots in your organization (using Bot Framework SDK and pro-code tools) for specific scenarios, you can convert those bots into a Skill and register that Skill in a Power Virtual Agents bot.

This article is intended for system administrators or IT professionals who are familiar with [Bot Framework Skills](/azure/bot-service/skills-conceptual?view=azure-bot-service-4.0). After a Skill has been registered with a Power Virtual Agents bot, authors can seamlessly [trigger Skill actions in conversation](advanced-use-skills.md).

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]


## Compare use of Flows and Skills actions
The following table will help determine when to use Skills for a conversation.

|    | **Flow actions** | **Skill actions** |
| -- | -- | -- |
| **Persona** | Bot authors can build reusable Flows to embed into any bot conversation | Developers can create, deploy, and host custom Skills in their own environment |
| **Conversation** | Use Flows for simple, single-turn operations. For example, place an order, or get order status. | Use Skills for complex, multi-turn operations. For example, schedule a meeting or book a flight. |
| **Response** | Use Flows to emit a bot response. For example, show a personalized message or inline images. | Use Skills to emit any supported bot response. For example, show an adaptive card or send random responses. |
| **Actions** | Use Flows to trigger server-side single-turn actions. For example, call an HTTP API or trigger a custom connector. | Use Skills to trigger server-side and client-side events and actions. For example, navigate to a page upon bot response. |


## Configure a Skill for use in Power Virtual Agents
First, [create a Power Virtual Agents bot](authoring-first-bot.md) and [create and deploy Skill using pro-code tools](https://go.microsoft.com/fwlink/?linkid=2110533) into your organization.

>[!NOTE]
>Power Virtual Agents only supports Skills built using [Bot Framework SDK version 4.7](/azure/bot-service/skills-conceptual?view=azure-bot-service-4.0) or above.

Before registering the Skill, provide the bot's ID to your Skills developer to authorize the bot to call actions in the Skill. [Learn more about Skill allowlist](https://go.microsoft.com/fwlink/?linkid=2123148).

**Add bot to Skill's allow list:**

1. In the [Power Virtual Agents portal](https://powerva.microsoft.com), on the side navigation pane, expand the **Manage** menu and select **Skills**.

   ![Select Manage, then Skills](media/skills-menu.png)

1. At the top of the Skills page, select **Provide ID for allow list**.
 
   ![Select Provide id for allow list button](media/skills-provide-id.png)

1. A window will show with your unique ID. Copy this ID and provide it to your Skills developer.

   ![Windows showing unique ID](media/skills-provide-id-modal.png)


**Enter the Skill manifest URL to add a Skill to your bot:**

1. In the [Power Virtual Agents portal](https://powerva.microsoft.com), on the side navigation pane, expand the **Manage** menu and select **Skills**.

   ![Select Manage, then Skills](media/skills-menu.png)

1. At the top of the Skills page, select **Add skill**.
 
   ![Select Add skill button](media/skills-provide-id.png)

1. Enter the URL to the Skill manifest. A Skill's manifest contains the information that your bot will need to trigger actions within a Skill.

1. Select **Next** to begin the [validation process](#troubleshooting-errors-during-skill-registration). When successful, your Skill is added to your bot. You can now [use this Skill in your topics](advanced-use-skills.md). 

## Compliance considerations

To protect user privacy, we require Skills to be registered as an app in the signed-in user's Azure Active Directory tenant.

### Troubleshooting errors during Skill registration

A series of validation checks are made against the URL. These checks ensure compliance, governance, and usability of the Skill being added to your bot. You will need to fix these errors prior to registering a skill.

Error message | Troubleshoot / Mitigation
---|---
We ran into problems getting the skill manifest.<br/>(`MANIFEST_FETCH_FAILED`)| Try opening your manifest URL in a web browser. If the URL renders the page within 10 seconds, re-register your Skill.
The manifest is incompatible. <br/>(`MANIFEST_MALFORMED`) | (a) Check if the manifest is a valid JSON file.<br/>(b) Check if the manifest contains required properties <br/>For example, (`name`, `msaAppId`, single `endpoint`, `activities`/`id`, `activities`/`description`, `activities`/`type` (only `event` or `message` supported)).
There is a mismatch in your endpoints <br/>(`MANIFEST_ENDPOINT_ORIGIN_MISMATCH`) | Check if your Skill endpoint matches your Azure AD application registration's `Publisher domain` (preferred) or `Home page URL` field. [Learn more about setting the home page for endpoints](https://go.microsoft.com/fwlink/?linkid=2123145).
To add a skill, it must first be registered <br/>(`APPID_NOT_IN_TENANT`) | Check if your skill's application ID is registered in your organization's Azure AD tenant. |
The link isn't valid; The link must begin with https:// <br/>(`URL_MALFORMED`, `URL_NOT_HTTPS`) | Re-enter the link as a secure URL. |
The manifest is too large; <br/>(`MANIFEST_TOO_LARGE`)| Check size of the manifest. It must be less than or equal to 500KB. |
This Skill has already been added to your bot. <br/>(`MANIFEST_ALREADY_IMPORTED`)| Delete the Skill and try registering again. |
The Skill is limited to 25 actions. <br/>(`LIMITS_TOO_MANY_ACTIONS`)|There are too many Skill actions defined in Skill manifest. Remove actions and try again. |
Actions are limited to 25 inputs. <br/>(`LIMITS_TOO_MANY_INPUTS`)|There are too many Skill action input parameters. Remove parameters and try again. |
Actions are limited to 25 outputs. <br/>(`LIMITS_TOO_MANY_OUTPUTS`)|There are too many Skill action output parameters. Remove parameters and try again. |
Your bot can have a maximum of 25 Skills. <br/>(`LIMITS_TOO_MANY_SKILLS`)| There are too many Skills added into a bot. Remove an existing Skill and try again. |
It looks like something went wrong.<br/>(`AADERROR_OTHER`)|There was a transient error while validating your Skill. Retry.|
Something went wrong while checking your Skill. <br/>(`ENDPOINT_HEALTHCHECK_FAILED`, `HEALTH_PING_FAILED`) | Check if your Skill endpoint is online and responding to messages.|
This skill has not allow-listed your bot <br/>(`ENDPOINT_HEALTHCHECK_UNAUTHORIZED`) | Check if your bot has been added to the Skills allowlist. [Learn more about adding a Skill to the allowlist](https://go.microsoft.com/fwlink/?linkid=2123431). |


---
title: "Work with chatbot variables - Preview"
description: "Use variables to use custom and prebuilt entities to allow for customized conversations in your bot."
keywords: "PVA"
ms.date: 11/02/2021
ms.service: power-virtual-agents
ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.custom: authoring, ceX
ms.collection: virtual-agent
---

# Use variables (Preview)

[!INCLUDE [cc-beta-prerelease-disclaimer](includes/cc-beta-prerelease-disclaimer.md)]

Select the version of Power Virtual Agents you're using here:

> [!div class="op_single_selector"]
>
> - [Power Virtual Agents web app](authoring-variables.md)
> - [Power Virtual Agents app in Microsoft Teams](teams/authoring-variables-teams.md)

> [!IMPORTANT]
> This is a preview feature.
>
> Preview features aren't meant for production use and may have restricted functionality. These features are available before an official release so that customers can get early access and [provide feedback](https://powerusers.microsoft.com/t5/Forums/ct-p/pva_forums).

Variables let you save responses from your customers in a conversation with your bot so that you can reuse them later in the conversation.

For example, you can save a customer's name in a variable called `UserName`. The bot can then address the customer by name as the conversation continues.

You can use variables to create logical expressions that dynamically route the customer down different conversation paths. You can also feed variables to [Power Automate](advanced-flow.md) and [Bot Framework skills](/azure/bot-service/bot-builder-skills-overview?view=azure-bot-service-4.0&preserve-view=true) as input parameters, and save the output results from those actions.  

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]

## Entity and variable type

Power Virtual Agents uses [entities](advanced-entities-slot-filling.md) to understand and identify a specific type of information from a user's responses. When saving the identified information to a variable, a variable type will be associated with it. The variable type is analogous with the entity.

Each entity or variable type maps to a base type, as listed in the following table.

The base type defines the operators that can be used for the corresponding variable when constructing a logical expression. It also defines whether a variable can be used to feed to an input parameter in a [flow](advanced-flow.md) or [Bot Framework skill](/azure/bot-service/bot-builder-skills-overview?view=azure-bot-service-4.0&preserve-view=true) as an input parameter.

For example, a **boolean** base type maps to an operator "is equal to" with possible values being true or false, while a **number** base type will give you a series of numeric operators such as "is equal to", "is greater than", or "is greater than or equal to", and so on.

 | Entity                  | Variable Base Type |
 | ----------------------- | ------------------ |
 | Multiple choice options | String             |
 | User's entire response  | String             |
 | Age                     | Number             |
 | Boolean                 | Boolean            |
 | City                    | String             |
 | Color                   | String             |
 | Continent               | String             |
 | Country or region       | String             |
 | Date and time           | String             |
 | Duration                | String             |
 | Email                   | String             |
 | Event                   | String             |
 | Language                | String             |
 | Money                   | String             |
 | Number                  | String             |
 | Ordinal                 | String             |
 | Organization            | String             |
 | Percentage              | Number             |
 | Person name             | String             |
 | Phone number            | String             |
 | Point of interest       | String             |
 | Speed                   | Number             |
 | State                   | String             |
 | Street address          | String             |
 | Temperature             | Number             |
 | URL                     | String             |
 | Weight                  | Number             |
 | Zip code                | String             |
 | Custom entity           | String             |

## Create a variable

In the bot authoring canvas, add a question node by choosing **Ask a question**. A variable will be automatically created within the node.

**Create a variable:**

1. Go to the [**Topics page**](./authoring-create-edit-topics.md) for the bot you want to edit.

1. Open the authoring canvas for the topic you want to add a variable to.

1. Click the plus (**+**) icon and select **Ask a question**.

   :::image type="content" source="media/authoring-variables/handoff-add-node.png" alt-text="Screenshot of adding a node." border="false":::

1. You'll notice a variable that contains the user's response is automatically created.

   :::image type="content" source="media/authoring-variables/Automatically_created_variable_(draft).PNG" alt-text="Create a variable." border="false":::

## Pick an entity to use

By default, the question node will be created using **Multiple choice options**, but you can also pick a different prebuilt or custom entity by choosing what to identify from the node. For example, you can choose **Person name** to use that entity.

:::image type="content" source="media/authoring-variables/Pick_an_entity_(draft).PNG" alt-text="Select an entity." border="false":::

## Rename a variable

When a variable is automatically created, a default name will be given. You rename a variable by selecting the variable name, entering a new name, and selecting **Done** to save the new name.

:::image type="content" source="media/authoring-variables/Rename_a_variable_(draft).PNG" alt-text="Rename a variable." border="false":::

## Use variables in action nodes

When using a variable in an action node, if a variable's base type matches with a parameter type specified for a flow or Bot Framework skill, it will show up as a candidate to be used to feed to this parameter. The output from the action nodes will generate new variables as well.  

:::image type="content" source="media/authoring-variables/User_a_variable_in_Skills(draft).PNG" alt-text="Use entity in an action node." border="false":::

## Use literal values in action nodes

Alternatively, you can type a literal value into an action node variable input field instead of selecting a variable from the menu. When passing a literal this way, you can type its value directly into the input field.

:::image type="content" source="media/authoring-variables/LiteralActionInput.png" alt-text="Screenshot showing the use of literal values for action inputs.":::

## Related links

- [Reuse variables across topics](authoring-variables-bot.md)
- [Customize the look and feel of the bot](customize-default-canvas.md)

[!INCLUDE[footer-include](includes/footer-banner.md)]

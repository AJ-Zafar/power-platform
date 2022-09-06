---
ROBOTS: NOINDEX,NOFOLLOW
title: "Work with chatbot variables"
description: "Use variables with custom and prebuilt entities to created customized bot conversations."
keywords: "PVA"
ms.date: 05/10/2022
ms.service: power-virtual-agents
ms.topic: article
author: iaanw
ms.author: iawilt
ms.reviewer: clmori
manager: shellyha
ms.custom: authoring, ceX
ms.collection: virtual-agent
---

# Use variables

[!INCLUDE [Build 2022](includes/build-22-disclaimer.md)]

Save customers' responses in a bot conversation to variables and reuse them later in the conversation. Or, use variables to create logical expressions that dynamically route the customer down different conversation paths.

For example, save a customer's name in a variable called `UserName`, and the bot can address the customer by name as the conversation continues.

Variables can also be passed into, and returned from, [other topics](#receive-values-from-other-topics), [Power Automate](advanced-flow.md) and [Bot Framework skills](/azure/bot-service/bot-builder-skills-overview).  

## Variable types

A variable is associated with a **type**. The type determines what values the variable can contain and the operators that you can use when you use the variable as part of a [condition](authoring-using-conditions.md).

<!-- best viewed without wordwrap -->
| Type     | Description                                                                                                           |
| -------- | --------------------------------------------------------------------------------------------------------------------- |
| String   | A sequence of characters used to represent text.                                                                      |
| Boolean  | A logical value that can only be `true` or `false`.                                                                   |
| Number   | Any real number.                                                                                                      |
| Table    | A list of values, but all values must be the same type.                                                 |
| Record   | A collection of name-value pairs where values can be any type.                                                        |
| DateTime | A date, time, day of the week, or month relative to a point in time.                                                  |
| Choice   | A list of string values that have associated synonyms.                                                                |
| Blank    | A placeholder for "no value" or "unknown value". See [Blanks in Power Fx](/power-platform/power-fx/data-types#blank). |

A variable's type is set based on the first time is has a value assigned.

Once a variable has been assigned a value, the type for that variable is fixed and it can't be assigned values of any other type. For example, a variable given the starting value of `1` is assigned the type **Number**. Attempting to assign it to a **String** value of `"apples"` will result in an error.

When testing a bot, a variable may appear temporarily as the type **unknown**. An **unknown** variable hasn't been assigned a value yet.

## Entities

Power Virtual Agents uses [entities](advanced-entities-slot-filling.md) to identify a specific type of information from a user's responses. Each entity maps to a **base type**, as listed in the following table.

| Entity                  | Variable Base Type |
| ----------------------- | ------------------ |
| Multiple-choice options | Choice             |
| User's entire response  | String             |
| Age                     | Number             |
| Boolean                 | Boolean            |
| City                    | String             |
| Color                   | String             |
| Continent               | String             |
| Country or region       | String             |
| Date and time           | DateTime           |
| Email                   | String             |
| Event                   | String             |
| Integer                 | Integer            |
| Language                | String             |
| Money                   | Number             |
| Number                  | Number             |
| Ordinal                 | Number             |
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
| Custom entity           | Choice             |

## Create a variable

Variables can be created in any node that prompts you to select a variable as an output, such as the [**Question**](authoring-ask-question.md) node, a [Power Automate Flow](advanced-flow.md) or [another topic](/authoring-variables.md#Receive-values-from-other-topics). 

These nodes will automatically create new variables, with the appropriate type, for each of their output variable slots.

You can use the [variable properties pane](#variable-properties-pane) to rename variables.

You can also create a new variable and assign a value to it immediately using the **Set Variable Value** node.

1. Select **+** to add a node and then select **Set a variable value**.

2. In the **Set variable** box, select the **>** arrow.

    :::image type="content" source="media/authoring-variables/create-new-variable-button.png" alt-text="Screenshot of selecting a variable in the Set Variable Value node.":::

3. In the flyout menu that appears, select **Create a new variable**.

    :::image type="content" source="media/authoring-variables/create-variable.png" alt-text="Screenshot of Create a new variable button.":::

A new variable will be created. The new variable's type will be **unknown** until you [assign a value](#set-a-variable) to it.

## Set a variable

Typically you'll use a [question node](authoring-create-edit-topics.md#ask-a-question) to save user input to a variable, but there may be situations where you want to set the value manually.

1. To assign a value to a [variable](authoring-variables.md), select **+** to add a node and then select **Set a variable value**.

1. For **Set variable**, choose or create a [new variable](#create-a-variable).

1. For **To value**, assign a value using one of the following 3 options.

   - Use a [literal value](#use-literal-values)
   - Choose an existing variable of the same type from the variable picker. This will set your variable to the same value as the variable you choose.
   - Use a [Power Fx formula](advanced-power-fx.md). This is useful for more complex types, where literal values cannot be used, such as Table and Record types.

## Variable initialization

TODO...

## Use literal values

Instead of selecting a variable from the menu, you can type a literal value into any variable input field.

:::image type="content" source="media/authoring-variables/set-variable-to-literal.png" alt-text="Screenshot showing the use of literal values for action inputs.":::

:::image type="content" source="media/authoring-variables/set-redirect-variable-to-literal.png" alt-text="Screenshot of the authoring canvas showing literal input on an input variable in a redirect node.":::

The node will attempt to interpret literal values entered as String, Number or a Boolean type. 

For example, entering _123_ will be interpreted as Number. If you wanted this interpreted as a string value instead, you could wrap this value in double quotes, like _"123"_. 

For some scenarios, or where you're using more complex types, To set a specific type, use the appropriate [Power Fx formula](advanced-power-fx.md) as per the following table:

## Variables pane

To open the variables pane, in the topic's menu bar, select **Variables**.

:::image type="content" source="media/authoring-variables/open-variable-button.png" alt-text="Screenshot of the variables button.":::

For each of the variables in the topic, select whether you want the values to be passed, received, or both between topics.

:::image type="content" source="media/authoring-variables/variables-pane.png" alt-text="Screenshot of the variables pane.":::

## Variable properties pane

To open the variable properties pane, select a variable in the [variables pane](#variables-pane).

You can also open the variable properties pane by selecting a variable in any node.

:::image type="content" source="media/authoring-variables/select-a-variable.png" alt-text="Screenshot of selecting a variable to open the variable properties pane.":::

In the variable properties pane you can rename a variable, see where a variable is used, or convert a variable to a [global variable](authoring-variables-bot.md).

:::image type="content" source="media/authoring-variables/variable-properties.png" alt-text="Screenshot of the properties pane.":::

## Passing variables between topics

When you redirect to other topics, you can pass values into variables in the destination topic or get variables back from it. Passing variables between topics is especially useful when you already have information that the topic needs. Your users will appreciate not having to answer the question again. It's also helpful when you refactor and separate your topics into reusable components and you want to pass variables across the topics.

### Receive values from other topics

When a topic defines a variable (for example, by a question node), the bot asks the user the question to fill in the variable’s value. If the bot has already acquired the value, there's no reason to ask the question again. In these cases, you can define the variable as **Receive values from other topics**. When another topic redirects to this one, it can pass a variable (or [literal values](#use-literal-values)) into this variable and skip the question. The experience for the user talking to the bot is seamless.

To receive values from other topics, set the variable's property:

1. In the **Question** node, select the variable that you want to receive values from other topics.

1. In the **Variables properties** pane,  under **Topic (limited scope)** select **Receive values from other topics**.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-properties-receive-input.png" alt-text="Screenshot of the authoring canvas showing the Variable properties pane with Receive values from other topics selected.":::

1. Save the topic.

2. Go to a topic that you want to redirect to. Follow the steps in [Redirect to another topic](authoring-moving-between-topics.md#redirecting-to-another-topic) to redirect to the correct topic.

3. Select **+ Add input for destination topic**.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-step1.png" alt-text="Screenshot of the authoring canvas showing adding input for the destination topic.":::

4. Select a variable in the redirected topic that you want to pass the variable to.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-step2.png" alt-text="Screenshot of the authoring canvas showing selecting the variable in the redirected topic.":::

5. Under **Enter or select a value**, select the variable in the current topic that you want to pass to the redirected topic.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-step3.png" alt-text="Screenshot of the authoring canvas showing selecting the variable from the list of options.":::

    The variable is shown in the redirected node.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-step4.png" alt-text="Screenshot of the authoring canvas showing the variable being passed into the redirected node.":::

### Return values to original topics

When a topic asks a question or obtains a variable from an action in some other way, the variable can be returned to the original topic that redirected to it.

In this case, the variable also becomes part of the original topic and can be used like any other variable. This helps you construct the topic, so that information the bot obtains is available across topics reducing the need for global variables.

To return a variable to the original topic, set the variable's property:

1. In the **Question** node, select the variable that you want to receive values from other topics.

1. In the **Variables properties** pane, under **Topic (limited scope)** select **Return values to original topics**.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-properties-return-value.png" alt-text="Screenshot of the authoring canvas showing the Variable properties pane with Return values to original topic selected.":::

1. Save the topic.

1. Go to a topic that you want to redirect to. Follow the steps in [Redirect to another topic](authoring-moving-between-topics.md#redirecting-to-another-topic) to redirect to the correct topic.

    The variable that's being returned to the topic is shown in the redirected topic. Use the returned variable in your topic.

    :::image type="content" source="media/authoring-variables/authoring-subtopic-pass-variable-pass-receive.png" alt-text="Screenshot of the authoring canvas showing a redirected topic with both values input and returned.":::

## Variable scope

By default, a variable can only be referenced within the topic where it is created and is said to be scoped to that topic. 

Variables can also existing within two other scopes.

- [Global](authoring-variables-bot.md) - Global scoped variables are created by the bot author, but are available for use across all topics. You can change the scope of any topic variable to make it a global variable.
- [System](#system-variables) - System variables are created automatically and provide additional contextual information about the conversation or the user. They are usually available in all topics.

## Global variables

See [Reuse variables across topics](authoring-variables-bot.md).

## System variables

There are a number of built-in system variables that provide additional information about a conversation. These variables are shown within the variable picker on the **System** tab.

:::image type="content" source="media/authoring-variables/authoring-variables-system-variable-picker.png" alt-text="Screenshot of Create a new variable button.":::

If you want to use system variables in a Power Fx formula, you must add `System.` before the name. For example, you'd need to use `System.Conversation.Id` instead of `Conversation.Id` (as shown at the top of the system tab in the variable picker above).

Some system variables are hidden from the variable picker and must be accessed with a [Power Fx formula](advanced-power-fx.md).

<!-- best viewed without wordwrap -->
| Name                                 | Type   | Hidden | Definition                                                      |
| ------------------------------------ | ------ | ------ | --------------------------------------------------------------- |
| Conversation.Id                      | string |        | Unique ID for the current conversation.                         |
| Conversation.TopicInitialUserMessage | string |        | User message which triggered the current topic.                 |
| LastActivity.Id                      | string |        | ID of the previously sent [activity][].                         |
| Activity.Channel                     | choice |        | Channel ID of the current conversation.                         |
| Activity.ChannelId                   | string | ✔      | Channel ID of the current conversation, as a string.            |
| Channel.DisplayName                  | string | ✔      | Display the name of the channel.                                |
| Activity.ChannelData                 | any    | ✔      | An object that contains channel-specific content.               |
| Activity.Value                       | any    | ✔      | Open-ended value.                                               |
| Activity.Type                        | choice |        | Type of [activity][].                                           |
| Activity.TypeId                      | string | ✔      | Type of [activity][], as a string.                              |
| Activity.Name                        | string |        | Name of the event.                                              |
| Activity.From.Id                     | string | ✔      | Channel-specific unique ID for the sender.                      |
| Activity.From.Name                   | string | ✔      | Channel-specific user-friendly name of the sender. |
| LastMessage.Text                   | string | ✔      | Last message text sent by the user. |
| Activity.Text                        | string |        | Message text sent by the user on the current turn.                                  |

[activity]: /azure/bot-service/bot-activity-handler-concept

## Related links

- [Reuse variables across topics](authoring-variables-bot.md)
- [Customize the look and feel of the bot](customize-default-canvas.md)

[!INCLUDE[footer-include](includes/footer-banner.md)]

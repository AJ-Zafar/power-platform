---
title: "Work with variables in Power Virtual Agents"
description: "Use variables to utilize custom and prebuilt entities to allow for customized conversations in your bot."
keywords: ""
ms.date: 4/15/2020
ms.service:
  - dynamics-365-ai
ms.topic: article
author: iaanw
ms.author: iawilt
manager: shellyha
ms.custom: authoring
ms.collection: virtual-agent
---

# Use variables

[!INCLUDE [cc-beta-prerelease-disclaimer](includes/cc-beta-prerelease-disclaimer.md)]

Variables let you save responses from your customers in a conversation with your bot so that you can reuse them later in the conversation. 

For example, you can save a customer's name in a variable called `UserName`. The bot can then address the customer by name as the conversation continues.

You can use variables to create logical expressions that dynamically route the customer down different conversation paths. You can also feed variables to [Power Automate](how-to-flow.md) and [Bot Framework Skills](/azure/bot-service/bot-builder-skills-overview?view=azure-bot-service-4.0) as input parameters, and save the output results from those actions.  

## Prerequisites

- [!INCLUDE [Medical and emergency usage](includes/pva-usage-limitations.md)]


## Entity and variable type
Power Virtual Agents uses *[entities](advanced-entities-slot-filling.md)* to understand and identify a specific type of information from a user's responses. When saving the identified information to a variable, a *variable type* will be associated with it. The variable type is essentially analogous with the *entity*. 

Each entity or variable type maps to a base type. These are listed in the following table. 

The base type defines the operators that can be used for the corresponding variable when constructing a logical expression. It also defines whether a variable can be used to feed to an input parameter in a [flow](advanced-flow.md) or [Bot Framework Skill](/azure/bot-service/bot-builder-skills-overview?view=azure-bot-service-4.0) as an input parameter. 

For example, a **boolean** base type maps to an operator "is equal to" with possible values being true or false, while a **number** base type will give you a series of numeric operators such as "is equal to", "is greater than", or "is greater than or equal to", and so on.

 Entity | Variable Base Type
 ---|---
 Multiple choice options | string
 User's entire response | string
 Age | number
 Boolean | boolean
 City | string
 Color | string
 Continent | string
 Country or region | string
 Date and time | string
 Duration | string
 Email | string
 Event | string
 Language | string
 Money | string
 Number | string
 Ordinal | string
 Organization | string
 Percentage | number
 Person name | string
 Phone number | string
 Point of interest | string
 Speed | number
 State | string
 Street address | string
 Temperature | number
 URL | string
 Weight | number
 Zip code | string
 Custom entity | string

## Create a variable
In the bot authoring canvas, add a question node by choosing "Ask a question". A variable will be automatically created within the node.

**Create a variable:**

1. Go to the [**Topics page**](getting-started-create-topics.md) for the bot you want to edit.

1. Open the authoring canvas for the topic you want to add a variable to.

1. Click the plus (**+**) icon and select **Ask a question**. 

   ![Screenshot of adding a node](media/handoff-add-node.png)

1. You'll notice a variable that contains the user's response is automatically created.


<!--from editor: I don't see any reason to include numbers in the following images' alt-text. I recommend removing the numbers. -->


   ![Image 1 - Create a variable](media/Automatically_created_variable_(draft).PNG)

## Pick an entity to use
By default, the question node will be created using *Multiple choice options*, but you can also pick a different prebuilt or custom entity by choosing what to identify from the node. For example, you can choose **Person name** to use that entity. 

![Image 2 - Select an entity](media/Pick_an_entity_(draft).PNG)

## Rename a variable
When a variable is automatically created, a default name will be given. You rename a variable by selecting the variable name, entering a new name, and selecting **Done** to save the new name.

![Image 3 - Rename a variable](media/Rename_a_variable_(draft).PNG)

## Use variables in action nodes
When using a variable in an action node, if a variable's base type matches with a parameter type specified for a flow or Bot Framework Skill, it will show up as a candidate to be used to feed to this parameter. The output from the action nodes will generate new variables as well.  

![Image 4 - Use entity in an action node](media/User_a_variable_in_Skills(draft).PNG)



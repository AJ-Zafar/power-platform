---
title: "Power Virtual Agents preview quickstart (preview)"
description: "Discover the new features introduced in Power Virtual Agents preview."
ms.date: 10/10/2022
ms.topic: article
author: v-alarioza
ms.author: v-alarioza
manager: shellyha
ms.collection: virtual-agent
---

# Quickstart (preview)

[!INCLUDE [Preview disclaimer](includes/public-preview-disclaimer.md)]

This quickstart walks you through making a bot that uses new features and improvements introduced in the Power Virtual Agents preview. We'll create a bot that helps users make a reservation at a fictional restaurant.

>
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4XQgu]
>

## Prerequisites

- [Create and edit topics](authoring-create-edit-topics.md)

## Create a bot

Power Virtual Agents now has an app-level home page that isn't specific to any bot. On this page you can create a new bot, view a list of recent bots that you've accessed, and learning resources like videos, documentation resources, and learning paths.

:::image type="content" source="media/quickstart/new-bot1.png" alt-text="Screenshot of the app-level home page.":::

1. In the side navigation, select **Create**, or select **Home** and then select **Create a bot**.

1. Select **Try the unified canvas (preview)** to create a preview bot.

1. An opt-in confirmation will be shown the first time you attempt to create or view a preview bot. Select **Continue** to continue, or select **No thanks** if you change your mind.

    :::image type="content" source="media/quickstart/preview-opt-in.png" alt-text="Screenshot of the preview opt-in message.":::

1. Name the bot `Reservation Bot` and select **Create**.

    :::image type="content" source="media/quickstart/new-bot2.png" alt-text="Screenshot of the create a chatbot dialog.":::

> [!IMPORTANT]
> Bots can only be created in English in the Power Virtual Agents preview.

## Customize the greeting topic

1. In the side navigation, select **Topics** and then select the **Greeting** topic.

1. In the existing **Message** node, select the **Delete** button in the ellipsis menu.

1. To add a new **Message** node, select **Send a message** in the add node menu. Type the following greetings as [message variations](authoring-send-message.md#use-message-variations):
    - `Good day!`
    - `Hi there!`
    - `Hi!`

1. [Add an image card](authoring-send-message.md#add-an-image) and provide an image of the restaurant to help the user visually confirm that they're booking at the correct location.

    :::image type="content" source="media/quickstart/image-card.png" alt-text="Screenshot of image card added to message node.":::

1. Add a second **Message** node and add the message `We're open 9am to 5pm Monday through Friday, and 10am through 8pm on the weekends. Please note, reservations can only be made for the next 7 days.`

1. Change the edit mode to **Speech**.

   The speech mode allows you to add a specific message for voice-enabled channels and enable the use of [SSML tags](authoring-send-message.md#use-ssml-to-customize-speech-responses).

    :::image type="content" source="media/quickstart/message-speech-mode.png" alt-text="Screenshot of speech mode toggle.":::

1. Add the message `We're open 9am to 5pm Monday through Friday, and 10am through 8pm on the weekends. <emphasis level="strong">Please note</emphasis><break strength="medium" />, reservations can only be made for the next 7 days.`

   When the bot speaks the message over a phone call, it will emphasize "Please note" and pause for a moment before continuing.

1. Add a third **Message** node and type the message `If you'd like, I can help you make a reservation.` to provide a call to action for the user.

1. Add a [quick reply](authoring-send-message.md#use-quick-replies) with the message `make a reservation`.

   A quick reply gives the user the option to select "make a reservation" instead of having to type it out.

   :::image type="content" source="media/quickstart/quick-reply.png" alt-text="Screenshot of the reservation quick reply.":::

1. Select **Save**.

## Add a reservation topic

1. In the side navigation, select **Topics** and then **New topic**.

1. Add the following trigger phrases:
    - `Reserve a table`
    - `Make a reservation`

1. Add a **Question** node and enter the message `What is the desired time and date of your reservation?`

1. For **Identify**, choose **Date and time**. This [entity](advanced-entities-slot-filling.md) enables your bot to extract a date and time from the user's response.

1. For **Save response as**, [create a new variable](authoring-variables.md) named `reservationDateTime`.

1. [Add a **ConditionItem** node](authoring-using-conditions.md) and [change it to a formula](advanced-power-fx.md#use-power-fx-as-a-condition).

1. Enter the [Power Fx formula](advanced-power-fx.md) `Topic.reservationDateTime < Today() || DateDiff(Today(), Topic.reservationDateTime) > 7`. This formula will evaluate to true if the date the user provided is in the past or more than seven days away.

    :::image type="content" source="media/quickstart/condition-formula.png" alt-text="Screenshot of Power Fx formula in a condition node.":::

1. Under the **ConditionItem** node, add a **Message** node. This message will remind the user that reservations can only be made in the next seven days. Enter the message `Sorry, I can only make reservations for the next seven days.`

1. Under the **All Other Conditions** node, add a **Message** node. This message will confirm the user's reservation.
    1. Enter `Your reservation has been made for`
    1. Select **Insert variable** and choose **reservationDateTime** from the list
    1. Enter `. We look forward to seeing you!`

    > [!TIP]
    > You can also enter the full message by using braces around the variable `Your reservation has been made for {Topic.reservationDateTime}. We look forward to seeing you!`.

    :::image type="content" source="media/quickstart/variable-reference.png" alt-text="Screenshot of variable in message node.":::

    When the bot responds with this message, the variable reference will be replaced with the value of the variable.

    :::image type="content" source="media/quickstart/variable-replaced.png" alt-text="Screenshot of the variable's value shown in a message.":::

1. Add a **Redirect** node where the two condition branches meet and choose the **End of conversation** topic.
The **End of conversation** topic is a pre-built topic designed to check if the user is satisfied and asks them to rate their experience.

1. Name the topic **Reservation** and select **Save**.

## Next steps

1. [Publish your bot](publication-fundamentals-publish-channels.md).

1. Test your bot using the [demo website](publication-connect-bot-to-web-channels.md), or call the phone number configured in Telephony.

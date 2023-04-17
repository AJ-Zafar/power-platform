---
title: "Deflection Overview"
description: "Best practices to improve deflection rate in a Power Virtual Agents chatbot"
author: athinesh

ms.date: 1/20/2023
ms.subservice: guidance
ms.topic: conceptual
ms.custom: guidance
ms.author: athinesh
ms.collection: virtual-agent
---
# Deflection overview

Return on investment (ROI) and improved customer satisfaction (CSAT) are top priorities for the organizations that implement Power Virtual Agents chatbots. 

Optimizing the bot deflection rates is one of the top focus areas for organizations to achieve their business goals around ROI, CSAT etc. and improve the overall bot performance. There are major indicators provided out-of-box in Power Virtual Agents to improve bot performance, such as resolution rate, escalation rate, and CSAT. 

While the metrics continue to evolve, there are several things you can do as a bot builder to improve the deflection rate of your bot. In these articles, we will cover the importance of deflection in conversational AI and general techniques/considerations that are universal for optimizing deflection for chatbots.  

> [!TIP]
> In the context of conversational AI, **deflection** is an indicator representing the percentage of requests that are completed in a self-service fashion that would otherwise be handled by live agents. In other words, it refers to the number of items a team avoids having to deal with as a result of automation.

## Why deflection optimization?

> [!div class="checklist"]
> * **Better customer experience**: more customers or employees can get their issues resolved by the bot instead of waiting for a human agent in chat or phone. This leads to a better customer experience, and higher CSAT scores. While this help reduce wait time, live agents can also focus on more complex, higher-value tasks.
> * **Cost savings**: one of the key ways the ROI of the bot is determined is using the deflection rates. The human agent call support typically costs around $ 5-10 in the contact center industry. However, a bot session that resolves a customer request costs around 50 cents. This means that higher deflection rates lead to higher cost savings.

## Understanding the key components of Power Virtual Agents analytics to improve deflection
A basic understanding of available analytics is required to be able to determine what deflection means for your organization. Below is the description of the key metrics from Power Virtual Agents:

|Description                     |Details                           |
|--------------------------------|----------------------------------|
| **Total Sessions**  | The total number of *analytics sessions* within the specified time period. <br> A conversation with a chatbot can generate one or multiple analytics sessions, each with their own engagement status and outcome. An analytics sessions begins when a user has new questions after an initial conversation completed (e.g. reached the End Of Conversation topic). |
|  **Engagement Rate** | The percentage of total sessions that are engaged sessions. <br> An engaged session is an analytics session in which a custom topic (as opposed to a system topic like "Conversation Start") is triggered, or the session ends in escalation. Engaged sessions can have one of three outcomes—they are either resolved, escalated, or abandoned. |
|  **Resolution Rate**  | The percentage of engaged sessions that are resolved. <br> A resolved session is an engaged session in which the user receives an End Of Conversation question that asks "*Did that answer your question?*" and the user either does not respond or responds "*Yes*". |
|  **Escalation Rate**  | The percentage of engaged sessions that are escalated. <br> An escalated session is an engaged session that is escalated to a human agent. |
|  **Abandon Rate** | The percentage of engaged sessions that are abandoned. <br> An abandoned session is an engaged session that is neither resolved nor escalated after one hour from the beginning of the session. |
|  **CSAT**  | The graphical view of the average of customer satisfaction (CSAT) scores for sessions in which customers respond to an End of Conversation request to take the survey.  |

These metrics need to be continuously improved to optimize the bot ROI. However, each organization may have their own definition of what deflection rate means to them. For example, an organization may consider Abandon rate along with Escalation rate as part of deflection calculation while another organization may look purely at Escalation rate. 

Despite having different definitions for deflection rate, the above listed metrics will still provide the foundation to calculate deflection.
Based on our experience with various customers, we have seen that in the context of deflection, Resolution rate and Escalation rate play a major role. Increasing the Resolution rate and reducing the Escalation rate typically has a direct impact on the overall bot deflection metrics.

## Key techniques
![deflection playbook techniques](./media/introduction/df-key-techniques.png)

> [!div class="nextstepaction"]
> [Topic escalation analysis](deflection-topic-escalation-analysis.md)
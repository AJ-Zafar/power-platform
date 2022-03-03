---
title: "CoE Starter Kit explained: building blocks and add-ons"
description: "The Power Platform CoE Starter Kit is shipped in multiple components. Learn about the building blocks and add-ons designed to help you innovate and improve."
author: manuelap-msft
manager: devkeydet

ms.component: pa-admin
ms.topic: conceptual
ms.date: 12/14/2020
ms.subservice: guidance
ms.author: mapichle
ms.reviewer: jimholtz
search.audienceType: 
  - admin
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
---
# CoE Starter Kit modules

The Center of Excellence (CoE) Starter Kit is shipped in multiple components:

## Building blocks

### Center of Excellence – core components

These components provide what you need to get started with setting up a CoE. They sync all your resources into tables and build admin apps on top of that to help you get more visibility of the apps, flows, and makers that exist in your environment. Additionally, apps like DLP Editor and Set App Permissions help with daily admin tasks.  

The CoE core components solution contains assets that are only relevant to admins. More information: [Set up core components](setup-core-components.md) and [Use core components](core-components.md)

### Center of Excellence – governance components

After you've become familiar with your environment and resources, you might start thinking about audit and compliance processes for your apps. You might want to gather additional information about your apps from your makers, or audit specific connectors or app usage. The apps and flows that are part of this solution help you get started.  

The governance components solution contains assets that are relevant to admins and makers. More information: [Set up governance components](setup-governance-components.md) and [Use governance components](governance-components.md)

### Center of Excellence – nurture components

An essential part of establishing a CoE is nurturing your makers and creating an internal community. You'll want to share best practices and templates, and onboard new makers. The assets that are part of this solution can help develop a strategy for this motion.  

The nurture components solution contains assets that are relevant to everyone in the organization: admins, makers, and users of apps and flows. More information: [Set up nurture components](setup-nurture-components.md) and [Use nurture components](nurture-components.md)

We recommend becoming familiar with the CoE core components solution before you add governance, nurture, or theming components.

## Standalone add-ons

### Center of Excellence – theming components

A frequent ask from organizations in which makers create canvas apps is the ability to apply themes&mdash;specifically, the ability to create apps that match the organizational brand. The assets in this solution will help you create, manage, and share themes.

The theming components solution contains assets that are relevant to makers and designers. More information: [Set up theming components](setup-theming.md) and [Use theming components](theming-components.md)

### Center of Excellence – application lifecycle management components  

The application lifecycle management (ALM) components are intended to provide Power Platform makers guidance on creating healthy ALM practices for their solutions as part of their overall DevOps strategy.

- **ALM Accelerator for Power Platform (AA4PP)** - Contains assets that are relevant to makers, advanced makers, maker teams, and admins. The ALM Accelerator for Power Platform provides an App and Azure Pipelines to enable Makers to source control their solutions and promote their solutions from one environment to another. Promotion of solutions can be configured to ensure the correct approvals take place at every step in the process. The user experience in the AA4PP App is configurable to allow for targeting specific user personas and roles. More information: [Set up ALM Accelerator for Power Platform components](setup-almacceleratorpowerplatform-cli.md) and [Use ALM Accelerator for Power Platform components](almacceleratorpowerplatform-components.md)

- **ALM Accelerator for Makers (AA4M)** - Contains a reference app and GitHub workflows to demonstrate allow makers to source control and deploy their solutions in a limited fashion. For more robust deployment configurations including components that need to be configured as part of or after the solution deployment, it's recommended to use the ALM Accelerator for Power Platform. The GitHub integration in the ALM Accelerator for Makers will eventually be replaced by functionality in the ALM Accelerator for Power Platform, but remains as part of the CoE Starter Kit for reference. More information: [Set up ALM Accelerator for Makers components](setup-almaccelerator.md) and [Use ALM Accelerator for Makers components](almaccelerator-components.md)

### Center of Excellence – Innovation Backlog components

The Innovation Backlog solution contains assets that are relevant to everyone in the organization. More information: [Set up Innovation Backlog components](setup-innovationbacklog.md), [What's in the Innovation Backlog components](innovationbacklog-components.md), and [Use the Innovation Backlog app](use-innovationbacklog.md)

[!INCLUDE[footer-include](../../includes/footer-banner.md)]

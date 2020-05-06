---
title: "Managed properties in Power Apps | Microsoft Docs"
description: "Learn about managed properties in Power Apps"
keywords: 
author: Mattp123
ms.author: matp
manager: kvivek
ms.custom: ""
ms.date: 05/05/2020
ms.reviewer: ""
ms.service: powerapps
ms.topic: "article"
search.audienceType: 
  - maker
search.app: 
  - PowerApps
  - D365CE
---

# Managed properties 
You can use managed properties to control which of your managed solution components can be customized. If you're creating solutions for other organizations, you should allow them to customize solution components where it makes sense for their unique requirements. However, you have to be able to predictably support and maintain your solution, so you should never allow any customization of critical components that provide the core functionality of your solution.

If you're creating solutions for your own organization, we recommend that you don't allow any customization of the components in your managed solutions at all. 

Managed properties are intended to protect your solution from modifications that
might cause it to break. Managed properties don't provide digital rights
management (DRM), or capabilities to license your solution or control who may
import it.

You apply managed properties when the solution is unmanaged in the unmanaged
layer of your development environment. The managed properties will take effect
after you package the managed solution and install it in a different
environment. After the managed solution is imported, the managed properties
can't be updated except by an update of the solution by the original publisher.

Most solution components have a **Managed properties** menu item available in the list of solution components. When you import the managed solution that
contains the components, you can view&mdash;but not change&mdash;their managed properties.

More information: [View and edit entity managed properties](/powerapps/maker/common-data-service/set-managed-properties-metadata) 

### See also
[Use segmented solutions](segmented-solutions-alm.md)
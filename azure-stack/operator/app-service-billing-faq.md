---
title: Azure App Service on Azure Stack Billing Overview and FAQ | Microsoft Docs
description: Details on how Azure App Service on Azure Stack is metered and charged for.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: femila
editor: ''

ms.assetid:
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 05/23/2019

---

# Azure App Service on Azure Stack billing overview and FAQ

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

The guidance in this document illustrates how cloud operators are billed for offering Azure App Service on Azure Stack and how they can in turn bill their tenants for their usage of the service.

## Billing overview

Azure Stack cloud operators choose to deploy the Azure App Service on Azure Stack onto their Azure Stack stamp in order to offer the tenant capabilities of Azure App Service and Azure Functions to their customers.  The Azure App Service resource provider consists of multiple types of roles that can be divided between infrastructure and worker tiers.

Infrastructure roles are not billed as they are required for the core operation of the service.  Infrastructure roles can be scaled out as required to support the demands of the cloud operator's tenants.  The infrastructure roles are as follows:

- Controllers
- Management roles
- Publishers
- Front ends

Worker tiers consist of two main types: shared and dedicated. Worker usage is billed to the cloud operator according to the following criteria.

## Shared Workers

Shared workers are multi-tenant and host free and shared App Service plans and consumption-based Azure Functions for many tenants. Shared workers emit usage meters when marked as ready in the Azure App Service resource provider.

## Dedicated workers

Dedicated workers are tied to the App Service plans which tenant(s) create. For example, in the S1 SKU, by default tenants can scale to 10 instances by default. Therefore, when a tenant creates an S1 App Service plan, Azure App Service allocates one of the instances in the small worker tier scale set to that tenant's App Service plan. Subsequently, the assigned worker is no longer available to be assigned to any other tenants.  If the tenant chooses to scale the App Service plan to 10 instances, then a further 9 workers are removed from the available pool and are assigned to the tenant's App Service plan.

Meters are emitted for dedicated workers when they are:

- Marked as ready in the Azure App Service resource provider.
- Assigned to an App Service plan.

Therefore, this billing model enables cloud operators to provision a pool of dedicated workers ready for customers to use without paying for the workers until they are effectively reserved by their tenant's app service plan. For example, if you have 20 workers in the Small Worker tier and then you have 5 customers create two S1 App Service plans each, and they each scale the App Service plan up to two instances, then you will have no workers available. As a result, there will also be no capacity for any of your customers or new customers to scale out or create new App Service plans. Cloud operators can view the current number of available workers per worker tier by looking at the worker tiers in the Azure App Service configuration on Azure Stack administration.

![App Service Worker Tiers][1]

## See customer usage using the Azure Stack usage service

Cloud operators can query the [Azure Stack tenant usage API](azure-stack-tenant-resource-usage-api.md) to retrieve usage information for their customers and you can find all of the individual meters that App Service emits to describe tenant usage in the [Usage FAQ](azure-stack-usage-related-faq.md). These meters can then be used to calculate the usage per customer subscription that can then be used to calculate charges.

## Frequently asked questions

### How do I license the SQL Server and file server infrastructure required in the pre-requisites

Licensing for SQL and file server infrastructure, required by the Azure App Service resource provider, is covered in the Azure App Service on Azure Stack - [Before you get started](azure-stack-app-service-before-you-get-started.md#licensing-concerns-for-required-file-server-and-sql-server) article.

### The Usage FAQ lists the tenant meters but not the prices for those meters, where can I find them

Cloud operators are free to apply their own pricing model to their end customers. The usage service provides the usage metering and the cloud operator must then use the meter quantity to charge their customers based on the pricing model they determine. Having this ability to set pricing enables operators to differentiate from other Azure Stack operators.

## Next steps

- [Azure Stack Tenant Usage API](azure-stack-tenant-resource-usage-api.md)

<!--Image references-->
[1]: ./media/app-service-billing-faq/app-service-worker-tiers.png
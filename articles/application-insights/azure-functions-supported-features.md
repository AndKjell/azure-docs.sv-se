---
title: Funktioner som stöds av Azure Application Insights – Azure Functions | Microsoft Docs
description: Application Insights funktioner som stöds för Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 0f4eaaefb7d2080218e19574621a4962e61057c3
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394318"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Funktioner som stöds av Application Insights för Azure Functions

Azure Functions erbjuder [inbyggd integrering](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) med Application Insights, som är tillgängligt via gränssnittet ILogger. Nedan visas en lista över funktioner som för närvarande stöds. Granska Azure Functions-guide för [komma igång](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Funktioner som stöds

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights SDK för .NET**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Automatisk insamling av**        |                 |                   |               
| &bull; Begäranden                     | Ja             | Ja               | 
| &bull; Undantag                   | Ja             | Ja               | 
| &bull; Beroenden                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; serviceBus|                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; eventHub  |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Ja               | 
| | | | 
| **Funktioner som stöds**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Ja             | Ja               | 
| &bull; Sampling                     | Ja             | Ja               | 
| &bull; Pulsslag                   |                 | Ja               | 
| | | | 
| **Korrelation**                       |                   |                   |               
| &bull; serviceBus                     |                   | Ja               | 
| &bull; eventHub                       |                   | Ja               | 
| | | | 
| **Konfigurerbara**                      |                   |                   |           
| &bull;Konfigureras fullt ut.<br/>Se [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) anvisningar.<br/>Se [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) för alla alternativ.               |                   | Ja                   | 


## <a name="sampling"></a>Samling

Azure Functions kan Sampling som standard i konfigurationen. Mer information finns i [konfigurera Sampling](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

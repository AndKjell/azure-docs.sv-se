---
title: Workspace() uttryck i Azure Log Analytics-fråga | Microsoft Docs
description: Arbetsytan uttryck används i en Log Analytics-fråga för att hämta data från en viss arbetsyta i samma resursgrupp, en annan resursgrupp eller en annan prenumeration.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 70ee244599f926f4d6c4a36f6065a1f743de2466
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971832"
---
# <a name="workspace-expression-in-log-analytics-query"></a>Workspace() uttryck i Log Analytics-fråga

Den `workspace` uttryck används i en Log Analytics-fråga för att hämta data från en viss arbetsyta i samma resursgrupp, en annan resursgrupp eller en annan prenumeration. Detta är användbart att inkludera loggdata i en Application Insights-fråga och för att köra frågor mot data över flera arbetsytor i en loggfråga.


## <a name="syntax"></a>Syntax

`workspace(`*identifierare*`)`

## <a name="arguments"></a>Argument

- *Identifieraren*: identifierar arbetsytan med något av format i tabellen nedan.

| Identifierare | Beskrivning | Exempel
|:---|:---|:---|
| Resursnamn | Mänskliga läsbart namn på arbetsyta (AKA ”komponentnamn”) | Workspace("contosoretail") |
| Kvalificerat namn | Fullständigt namn på arbetsytan i formatet: ”subscriptionName/resourceGroup/componentName” | Workspace('Contoso/ContosoResource/ContosoWorkspace') |
| ID | GUID för arbetsytan | Workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure-resurs-ID | Identifierare för Azure-resursen | Workspace("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/Workspaces/contosoretail") |


## <a name="notes"></a>Anteckningar

* Du måste ha läsbehörighet till arbetsytan.
* Ett relaterade uttryck är `app` som gör det möjligt att söka i Application Insights-program.

## <a name="examples"></a>Exempel

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Nästa steg

- Se den [app uttryck](workspace-expression.md) att referera till Application Insights-app.
- Läs om hur [Log Analytics-data](../../log-analytics/log-analytics-log-search.md) lagras.
---
title: Automatisera Azure Stack-verifiering med PowerShell | Microsoft Docs
description: Du kan automatisera Azure Stack-verifiering med PowerShell.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 1cb4b1a7cfd72ea302676244a53af58e77215aa9
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235454"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Automatisera Azure Stack-verifiering med PowerShell 

Verifiering som en tjänst (VaaS) gör möjligheten att automatisera starta tester med hjälp av den **LaunchVaaSTests.ps1** skript.

Du kan använda PowerShell för följande arbetsflöde:

- Testet arbetsflöde

Det här skriptet omfattar fyra element i ett arbetsflöde:

- Installerar nödvändiga komponenter.
- Installerar och startar den lokala agenten.
- Startar en viss kategori av testerna som integrering, funktionell tillförlitlighet.
- Rapporter varje test skicka resultatet för att övervaka och rapportera filen generation.

## <a name="launch-the-test-pass-workflow"></a>Starta arbetsflödet test-pass

1. Öppna en upphöjd PowerShell-prompt.

2. Kör följande skript för att ladda ned skriptet automation:

    ````PowerShell  
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://vaastestpacksprodeastus.blob.core.windows.net/packages/Microsoft.VaaS.Scripts.3.0.0.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ````

3. Kör följande skript med dina värden:

    ````PowerShell  
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>"  -AsPlainText -Force)
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<ServiceAdminUser>", (ConvertTo-SecureString "<ServiceAdminPassword>" -AsPlainText -Force)
    $TenantAdminCreds = New-Object System.Management.Automation.PSCredential "<TenantAdminUser>", (ConvertTo-SecureString "<TenantAdminPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
        -VaaSAccountTenantId <VaaSAccountTenantId> `
        -VaaSSolutionName <VaaSSolutionName> `
        -VaaSTestPassName <VaaSTestPassName> `
        -VaaSTestCategories Integration,Functional `
        -MaxScriptWaitTimeInHours 12 `
        -ServiceAdminCreds $ServiceAdminCreds `
    ````

    **Parametrar**

    | Parameter | Beskrivning |
    | --- | --- |
    | VaaSUserld | Ditt VaaS användar-ID. | 
    | VaaSUserPassword | Lösenordet VaaS. |
    | ServiceAdminUser | Ditt Azure Stack-admin-tjänstkonto.  |
    | ServiceAdminPassword | Lösenordet för tjänsten på Azure Stack.  |
    | TenantAdminUser | Administratören för den primära klienten.  |
    | TenantAdminPassword | Lösenordet för den primära klienten.  |
    | FunctionalCategory| Integrering, fungerar, eller. Tillförlitlighet. Om du använder flera värden, Avgränsa varje värde med ett kommatecken.  |

4. Granska resultatet av testet. Läs om hur läser testresultaten [övervaka tester](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Nästa steg

 - Mer information om [Azure Stack-verifiering som en tjänst](https://docs.microsoft.com/azure/azure-stack/partner).
 - Mer information om PowerShell i Azure Stack finns i [Azure Stack-modulen](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) referens.
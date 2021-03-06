---
title: Ersätta en fysisk disk i Azure Stack | Microsoft Docs
description: Beskriver processen för hur du ersätter en fysisk disk i Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: 7ce501be5458282273e51a5b2bc18482592d2333
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44376960"
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Ersätta en fysisk disk i Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

Den här artikeln beskrivs den allmänna processen för att ersätta en fysisk disk i Azure Stack. Om en fysisk disk kraschar, bör du ersätta det så snart som möjligt.

Du kan använda den här proceduren för integrerade system och development kit distributioner som har diskar som växlas.

Faktiska diskbyte stegen varierar baserat på din maskinvaruleverantör för OEM-tillverkare (original equipment manufacturer). Se dokumentationen från leverantören fältet replaceable enhet (FRU) för detaljerade anvisningar som är specifika för ditt system. 

## <a name="review-disk-alert-information"></a>Granska disk aviseringsinformation
När en disk misslyckas, kan du få ett meddelande som talar om att anslutningen har kopplats till en fysisk disk. 

 ![Avisering som visar anslutningen kopplades till fysisk disk](media/azure-stack-replace-disk/DiskAlert.png)

Om du öppnar aviseringen innehåller varningsbeskrivningen noden skala enhet och plats exakta fysiska platsen för den disk som du måste ersätta. Ytterligare Azure Stack hjälper dig att identifiera den skadade disken med hjälp av LED indikator funktionerna.

 ## <a name="replace-the-disk"></a>Ersätt disken

Följ instruktionerna för din OEM maskinvaruleverantörens FRU för faktiska diskbyte.

> [!note]
> Ersätt diskar för en skala enhet nod i taget. Vänta tills de virtuell disk reparera ska slutföras innan du fortsätter till nästa nod i skala enhet

Om du vill förhindra användning av en disk som inte stöds i ett integrerat system, blockerar systemet diskar som inte stöds av leverantören. Om du försöker använda en som inte stöds disk, om en ny avisering att en disk har har placerats i karantän på grund av en modell som inte stöds eller inbyggd programvara.

När du ersätter disken identifieras den nya disken automatiskt i Azure Stack och startar reparationsprocessen virtuell disk.  
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>Kontrollera status för virtuell disk reparation
 
 När du ersätter disken kan du övervaka hälsostatus för virtuell disk och reparera jobbförlopp genom att använda privilegierad slutpunkt. Följ dessa steg från alla datorer som har en nätverksanslutning till privilegierad slutpunkt.

1. Öppna en Windows PowerShell-session och Anslut till privilegierad slutpunkt.
    ````PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```` 
  
2. Kör följande kommando för att visa hälsotillstånd för virtuell disk:
    ````PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ````
   ![PowerShell-utdata från kommandot Get-VirtualDisk](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Kör följande kommando för att visa aktuell status för jobbet av lagring:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ````
      ![PowerShell-utdata från kommandot Get-StorageJob](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Felsöka virtuell disk reparation

Om den virtuella disken reparera visas jobb har fastnat, kör följande kommando för att starta om jobbet:
  ````PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ```` 

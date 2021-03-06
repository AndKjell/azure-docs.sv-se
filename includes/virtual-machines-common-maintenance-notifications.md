---
title: ta med fil
description: ta med fil
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/02/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: efedb2f48748264fb936fe82a1dbb3cf4403cc5e
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/16/2018
ms.locfileid: "40166955"
---
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Visa virtuella datorer som har schemalagts för underhåll i portalen

Du kan se listan över virtuella datorer som påverkas av den kommande underhållet när planerade underhållet har schemalagts. 

Du kan använda Azure portal och leta efter virtuella datorer som är schemalagda för underhåll.

1. Logga in på [Azure Portal](https://portal.azure.com).

2. I det vänstra navigeringsfönstret klickar du på **virtuella datorer**.

3. I virtuella datorer-fönstret klickar du på den **kolumner** knappen för att öppna listan över tillgängliga kolumner.

4. Välj och Lägg till följande kolumner:

   **Underhåll**: Visar status för underhåll för den virtuella datorn. Följande är möjliga värden:
      
      | Värde | Beskrivning |
      |-------|-------------|
      | Börja nu | Virtuellt datorn i självbetjäningsunderhållsperioden som gör att du kan starta underhållet själv. Se nedan om hur du startar Underhåll på den virtuella datorn. | 
      | Har schemalagts | Den virtuella datorn är schemalagd för underhåll utan något alternativ för dig att starta underhållet. Du kan lära dig om underhållsperioden genom att välja Underhåll – schemalagd period i den här vyn eller genom att klicka på den virtuella datorn. | 
      | Redan uppdaterat | Den virtuella datorn har uppdaterats redan och ingen ytterligare åtgärd krävs just nu. | 
      | Försök igen senare | Du har initierat underhållet utan framgång. Du kommer att kunna använda alternativet självbetjäningsunderhållet vid ett senare tillfälle. | 
      | Försök nu | Du kan göra om en tidigare misslyckad lokal kundinitierat underhåll. | 
      | - | Den virtuella datorn är inte en del av en planerade underhållet senare. |
      

   **Underhåll – självbetjäning period**: Visar på vilket tidsfönster då du själv kan starta Underhåll på dina virtuella datorer.
   
   **Underhåll – schemalagd period**: visar tidsfönstret när Azure ska behålla den virtuella datorn för att kunna utföra underhåll. 



## <a name="notification-and-alerts-in-the-portal"></a>Meddelanden och aviseringar i portalen

Azure kommunicerar ett schema för planerat underhåll genom att skicka ett e-postmeddelande till gruppen prenumeration ägare och Medägare. Du kan lägga till ytterligare mottagare och kanaler denna kommunikation genom att skapa aviseringar för Azure-aktivitet. Mer information finns i [övervaka prenumerationsaktivitet med Azure-aktivitetsloggen] (.. / articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

1. Logga in på [Azure Portal](https://portal.azure.com).
2. I menyn till vänster väljer **övervakaren**. 
3. I den **övervaka – aviseringar (klassisk)** fönstret klickar du på **+ Lägg till aktivitetsloggavisering**.
5. Fyll i den **Lägg till aktivitetsloggavisering** sidan och se till att ange följande **kriterier**:
   - **Händelsekategori**: Tjänstehälsa
   - **Tjänster**: Virtual Machine Scale Sets och virtuella datorer
   - **Typ**: planerat underhåll 
    
Mer information om hur du konfigurerar aviseringar för aktivitetsloggar finns [skapa aviseringar för aktivitetsloggen](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md).
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Starta Underhåll på den virtuella datorn från portalen

Medan de tittar på de Virtuella datorinformationen, kommer du att kunna se mer underhållsrelaterade information.  
Högst upp i den detaljerade vyn för virtuell dator läggs en ny avisering menyflik om den virtuella datorn ingår i en planerade underhållet senare. Dessutom läggs ett nytt alternativ för att starta Underhåll när det är möjligt. 


Klicka på Underhåll meddelandet om du vill se underhållssidan med mer information om planerat underhåll. Därifrån kan du kommer att kunna **starta Underhåll** på den virtuella datorn.

När du startar underhåll, den virtuella datorn kommer att finnas kvar och status för underhåll kommer att uppdateras för att återspegla resultatet inom några minuter.

Om du missade fönstret självbetjäning kan att du fortfarande kunna se fönstret när den virtuella datorn kommer att finnas kvar av Azure. 

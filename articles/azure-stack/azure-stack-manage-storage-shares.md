---
title: Hantera kapacitet i Azure Stack | Microsoft Docs
description: Övervaka och hantera tillgängligt lagringsutrymme för Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: d979692ba32c596058d28c88f32e7e5872ab7ef9
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452334"
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Hantera lagringskapacitet för Azure Stack 

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

Informationen i den här artikeln hjälper operatorn Övervakare för Azure Stack-molnet och hantera lagringskapaciteten för deras Azure Stack-distribution. Azure Stack-lagringsinfrastruktur allokerar en delmängd av den totala lagringskapaciteten på Azure Stack-distributioner som ska användas för **lagringstjänster**. Lagringstjänsterna lagra data för en klient på resurser på volymer som motsvarar noderna i distributionen.

Som en cloud-operator har du en begränsad mängd lagringsutrymme du arbetar med. Mängden lagringsutrymme som definieras av du implementerar lösningen. Din lösning tillhandahålls genom din OEM-leverantör när du använder en lösning för flera noder eller genom att maskinvaran som du installerar Azure Stack Development Kit.

Eftersom Azure Stack inte har stöd för utökning av lagringskapacitet, är det viktigt att [övervakaren](#monitor-shares) det tillgängliga lagringsutrymmet till att säkerställa effektiv drift bevaras.  

När den återstående kapaciteten för en resurs blir begränsad, planerar att [hantera utrymmet](#manage-available-space) att förhindra att resurserna körs utanför kapacitet.

Alternativ för att hantera kapacitet är:
- Frigöra kapacitet
- Migrera en behållare

När en resurs är 100% optimeras lagringen tjänsten inte längre funktioner för resursen. Kontakta Microsoft support om du vill ha hjälp med att återställa åtgärder för resursen.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Förstå volymer och resurser, behållare och diskar
### <a name="volumes-and-shares"></a>Volymer och resurser
Den *lagringstjänst* partitionerar tillgängligt lagringsutrymme i separat och lika med volymer som har allokerats för att lagra data för klientorganisationen. Antalet volymer är lika med antalet noder i Azure Stack-distributionen:

- Det finns fyra volymer på en distribution med fyra noder. Varje volym har en enda resurs. På en distribution med flera noder minskas antal resurser inte om en nod har tagits bort eller inte fungerar korrekt.
- Om du använder Azure Stack Development Kit, finns det en enskild volym med en enda resurs.

Eftersom storage service-resurser är för exklusiv användning av storage-tjänster, måste du inte direkt ändra, lägga till eller ta bort filer på filresurser. Storage-tjänster bör arbeta med filer som lagras i dessa volymer.

Resurser på volymer håller klientdata. Klientdata innehåller sidblobar, blockblobbar, lägga till blobbar, tabeller, köer, databaser och relaterade metadatalagring. Eftersom storage-objekt (BLOB-objekt, osv.) finns individuellt inom en enda resurs, får inte den maximala storleken för varje objekt överskrida storleken på en resurs. Den maximala storleken för nya objekt är beroende av den kapacitet som finns kvar i en resurs som outnyttjat utrymme när det nya objektet skapas.

När en resurs har för lite ledigt utrymme och åtgärder för att [frigöra](#reclaim-capacity) utrymme är inte lyckades eller tillgänglig, operatorn Azure Stack-molnet kan [migrera](#migrate-a-container-between) blob-behållare från en resurs till en annan.

- Mer information om behållare och blobbar finns i [Blob-lagring](azure-stack-key-features.md#blob-storage) i nyckel-funktioner och koncept i Azure Stack.
- Information om hur klientanvändare fungerar med blob storage i Azure Stack finns i [Azure Stack-lagringstjänster](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Containrar
Klientanvändare skapa behållare som sedan används för att lagra blob-data. När användaren väljer i vilken behållare att placera BLOB storage-tjänsten använder en algoritm för att fastställa på vilken volym att placera behållaren. Algoritmen väljer vanligtvis volymen med mest tillgängligt utrymme.  

När en blob placeras i en behållare, kan som växa mer utrymme. När du lägger till nya blobbar och befintliga växer blobar, det tillgängliga utrymmet på volymen som innehåller den behållaren krymper.  

Behållare är inte begränsad till en enda resurs. När den kombinerade blob-data i en behållare växer Använd 80% eller mer av det tillgängliga utrymmet, försätts i behållaren *spill* läge. I spill läge allokeras alla nya blobbar som skapas i den behållaren till en annan volym som har tillräckligt med utrymme. Framöver kommer kan en behållare i spill läge ha blobbar som är fördelade på flera volymer.

När 80% och 90% av det tillgängliga utrymmet på en volym används, genererar systemet aviseringar i Azure Stack-administratörsportalen. Molnoperatörer bör granska tillgängliga lagringskapaciteten och Kom ihåg att balansera om innehållet. Storage-tjänsten slutar att arbete när en disk är 100% används och inga fler aviseringar aktiveras.

### <a name="disks"></a>Diskar
VM-diskar läggs till behållare av klienter, inklusive en operativsystemdisk. Virtuella datorer kan också ha en eller flera datadiskar. Båda typerna av diskar lagras som sidblobar. För att klienter är att placera varje disk i en separat behållare för att förbättra prestanda för den virtuella datorn.
- Varje behållare som innehåller en disk (sidblob) från en virtuell dator betraktas som en behållare som är anslutna till den virtuella datorn som äger disken.
- En behållare som inte innehåller en disk från en virtuell dator betraktas som en kostnadsfri behållare.

Alternativ för att frigöra utrymme på en ansluten behållare [begränsas](#move-vm-disks).
> [!TIP]  
> Molnoperatörer hanterar inte diskar som är kopplade till virtuella datorer (VM) som klienter kan lägga till en behållare. När du planerar att hantera utrymmet på storage-resurser, kan det vara för att förstå hur diskar som är relaterade till behållare och filresurser.

## <a name="monitor-shares"></a>Övervaka resurser
Använd PowerShell eller admin portal för att övervaka resurser så att du kan förstå när utrymmet är begränsad. När du använder portalen kan få du aviseringar om resurser som är ont om utrymme.    

### <a name="use-powershell"></a>Använd PowerShell
Som en cloud-operatör kan du övervaka lagringskapaciteten för en resurs med PowerShell **Get-AzsStorageShare** cmdlet. Cmdleten Get-AzsStorageShare returnerar det totala allokerade och ledigt utrymmet i byte på var och en av resurserna.   
![Exempel: Returnera ledigt utrymme för resurser](media/azure-stack-manage-storage-shares/free-space.png)

- **Total kapacitet** är det totala utrymmet i byte som är tillgängliga för resursen. Här används för data och metadata som underhålls av storage-tjänster.
- **Kapacitet som används av** är mängden data i byte som används av de alla utrymmena de filer som lagrar data för klientorganisationen och tillhörande metadata.

### <a name="use-the-administrator-portal"></a>Använda administratörsportalen
Du kan använda administrationsportalen visa lagringskapaciteten för alla resurser som en cloud-operator.

1. Logga in på den [administrationsportalen](https://adminportal.local.azurestack.external).
2. Välj **alla tjänster** > **Storage** att öppna fillistan för resurs där du kan visa information om syntax. 

  ![Exempel: Lagringsfilresurser](media/azure-stack-manage-storage-shares/storage-file-shares.png)

  - **Totalt antal** är det totala utrymmet i byte som är tillgängliga för resursen. Här används för data och metadata som underhålls av storage-tjänster.
  - **ANVÄNDS** är mängden data i byte som används av de alla utrymmena de filer som lagrar data för klientorganisationen och tillhörande metadata.

### <a name="storage-space-alerts"></a>Storage utrymme aviseringar
När du använder admin portal kan få du aviseringar om resurser som är ont om utrymme.

> [!IMPORTANT]
> Behåll resurser från att nå fullständig användning som en cloud-operator. När en resurs är 100% optimeras lagringen tjänsten inte längre funktioner för resursen. Om du vill återställa ledigt utrymme och återställning på en resurs som är 100% används, måste du kontakta Microsoft support.

**Varning**: när en filresurs är över 80% används, får du en *varning* avisering i admin portal: ![exempel: varning](media/azure-stack-manage-storage-shares/alert-warning.png)


**Kritiska**: när en filresurs är över 90% används, får du en *kritiska* avisering i admin portal: ![exempel: kritisk avisering](media/azure-stack-manage-storage-shares/alert-critical.png)

**Visa information om**: I admin portal öppnar du informationen för en avisering för att visa alternativ för minskning: ![exempel: Visa aviseringsinformation](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Hantera tillgängligt utrymme
När det är nödvändigt att ledigt utrymme på en resurs kan du använda metoderna minst inkräktande först. Prova till exempel att frigöra utrymme innan du väljer att migrera en behållare.  

### <a name="reclaim-capacity"></a>Frigöra kapacitet
*Det här alternativet gäller både flernodiga distributioner och Azure Stack Development Kit.*

Du kan frigöra kapacitet som används av innehavarens konton som har tagits bort. Den här kapaciteten är automatiskt frigöras när data [kvarhållningsperioden](azure-stack-manage-storage-accounts.md#set-the-retention-period) uppnås, eller du kan utföra åtgärder för att frigöra det. omedelbart.

Mer information finns i [frigöra kapacitet](azure-stack-manage-storage-accounts.md#reclaim) i hantera lagringsresurser.

### <a name="migrate-a-container-between-volumes"></a>Migrera en behållare mellan volymer
*Det här alternativet gäller endast för distributioner med flera noder.*

På grund av klient användningsmönster använda några klient resurser mer utrymme än andra. Resultatet kan vara en resurs som börjar ta slut på utrymme innan andra resurser som är relativt oanvända.

Du kan försöka att frigöra utrymme på en överansträngda resurs genom att migrera manuellt vissa blob-behållare till en annan resurs. Du kan migrera flera mindre behållare till en enda resurs som har kapacitet att innehålla alla. Du kan använda migrering för att flytta *kostnadsfria* behållare. Kostnadsfria behållare är behållare som inte innehåller en disk för en virtuell dator.   

Migrering konsoliderar alla en behållare blob på den nya resursen.

- Om en behållare har försatts i spill läge och har placerats blobar på ytterligare volymer, kommer den nya resursen måste ha tillräckligt med kapacitet att innehålla alla blobar för den behållare som du migrerar. Detta omfattar de blobar som finns i ytterligare resurser.

- PowerShell-cmdleten *Get-AzsStorageContainer* identifierar bara utrymme som används i den ursprungliga volymen för en behållare. Cmdleten identifierar inte utrymme som används av blobar på ytterligare volymer. I full storlek i en behållare kan därför inte tydligt. Det går att slå samman en behållare på en ny resurs kan skicka den nya resursen till ett spill villkor där data till ytterligare resurser placeras. Därför kan behöva du balansera om resurser igen.

- Om du saknar behörighet till en resursgrupp och det går inte att använda PowerShell för att fråga efter ytterligare volymer för spill data, tillsammans med ägaren av dessa resursgrupper och behållare att förstå den totala storleken på data att migrera innan du migrerar data.  

> [!IMPORTANT]
> Migrering av BLOB-objekt för en behållare är en offline-åtgärd som kräver användning av PowerShell. Tills migreringen är klar, alla blobar för den behållare som du migrerar vara offline och kan inte användas. Du bör också undvika att uppgradera Azure Stack tills alla pågående migreringen är klar.

#### <a name="to-migrate-containers-using-powershell"></a>Att migrera behållare med hjälp av PowerShell
1. Bekräfta att du har [Azure PowerShell installerad och konfigurerad](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Mer information finns i [Använda Azure PowerShell med Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767).
2.  Granska behållare för att förstå vilka data som finns på den resurs som du planerar att migrera. Använd för att identifiera de bästa kandidat behållarna för migrering i en volym på **Get-AzsStorageContainer** cmdlet:

    ````PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    $shares = Get-AzsStorageShare -FarmName $farm_name
    $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -FarmName $farm_name
    ````
    Granska $containers:

    ````PowerShell
    $containers
    ````

    ![Exempel: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3.  Identifiera de bästa mål-resurserna för att lagra den behållare som du migrerar:

    ````PowerShell
    $destinationshares = Get-AzsStorageShare -SourceShareName
    $shares[0].ShareName -Intent ContainerMigration
    ````

    Granska $destinationshares:

    ````PowerShell 
    $destinationshares
    ````

    ![Exempel: $destination resurser](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Starta migreringen för en behållare. Migreringen är asynkrona. Om du startar migreringen av ytterligare behållare innan den första migreringen är klar, kan du använda jobb-id för att spåra status för var och en.

  ````PowerShell
  $job_id = Start-AzsStorageContainerMigration -StorageAccountName $containers[0].Accountname -ContainerName $containers[0].Containername -ShareName $containers[0].Sharename -DestinationShareUncPath $destinationshares[0].UncPath -FarmName $farm_name
  ````

  Granska $jobId. I följande exempel ersätter *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* med jobb-id som du vill undersöka:

  ````PowerShell
  $jobId
  d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
  ````

5. Du kan använda jobb-id för att kontrollera statusen för migreringsjobbet. När behållaren migreringen är klar, **MigrationStatus** är inställd på **Slutför**.

  ````PowerShell 
  Get-AzsStorageContainerMigrationStatus -JobId $job_id -FarmName $farm_name
  ````

  ![Exempel: Migreringsstatus](media/azure-stack-manage-storage-shares/migration-status1.png)

6.  Du kan avbryta ett pågående migreringsjobb. Har avbrutits migrering som jobb ska bearbetas asynkront. Du kan spåra uppsägningen av prenumerationen med hjälp av $jobid:

  ````PowerShell
  Stop-AzsStorageContainerMigration -JobId $job_id -FarmName $farm_name
  ````

  ![Exempel: Återställningsstatus](media/azure-stack-manage-storage-shares/rollback.png)

7. Du kan köra en kommandot tillsammans från steg 6 igen, tills status bekräftar migreringsjobbet är **avbruten**:  

    ![Exempel: Avbruten status](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>Flytta VM-diskar
*Det här alternativet gäller endast för distributioner med flera noder.*

Extrema metoden att hantera adressutrymmen innebär flytt av virtuella diskar. Eftersom det är komplicerat att flytta en anslutna behållare (en som innehåller en VM-disk), kan du kontakta Microsoft Support om du vill utföra den här åtgärden.

## <a name="next-steps"></a>Nästa steg
Läs mer om [erbjuder virtuella datorer till användare](azure-stack-tutorial-tenant-vm.md).

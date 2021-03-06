---
title: Skapa och återställer en säkerhetskopia i BizTalk Services | Microsoft Docs
description: BizTalk Services innehåller säkerhetskopiering och återställning. Lär dig hur du skapar och återställer en säkerhetskopia och Bestäm vad säkerhetskopieras. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 90cf2d0ddbba47a856bf1299a101c5185873b5d8
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214420"
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Säkerhetskopiering och återställning

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services innehåller säkerhetskopierings- och återställningsfunktioner. 

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

> [!NOTE]
> Hybridanslutningar säkerhetskopieras inte, oberoende av vilken utgåva. Du måste återskapa din hybridanslutningar.


## <a name="before-you-begin"></a>Innan du börjar
* Säkerhetskopiering och återställning kanske inte tillgänglig för alla utgåvor. Se [BizTalk Services: diagram över utgåvor](biztalk-editions-feature-chart.md).
* Säkerhetskopiera innehåll kan återställas till samma BizTalk Service eller till ett nytt BizTalk Service. Om du vill återställa den BizTalk Service med samma namn, befintliga BizTalk Service måste tas bort och namnet måste vara tillgängliga. Det kan ta längre tid än vad som ville ha för samma namn ska vara tillgängliga när du har tagit bort en BizTalk Service. Om du inte vill vänta tills samma namn ska vara tillgängliga, återställer du sedan till ett nytt BizTalk Service.
* BizTalk Services kan återställas till samma version eller högre version. Återställer BizTalk Services till en lägre version stöds från när säkerhetskopian skapades, inte.
  
    Till exempel kan en säkerhetskopiering med Basic-versionen återställas till Premium-versionen. En säkerhetskopia som använder Premium-versionen kan inte återställas till Standard-utgåvan.
* EDI-kontrollnummer säkerhetskopieras för att upprätthålla affärskontinuitet av. Meddelanden som bearbetas efter den senaste säkerhetskopieringen, kan återställa säkerhetskopierade innehållet orsaka duplicerade kontrollnummer.
* Om en batch har aktiva meddelanden, bearbetar batchen **innan** kör säkerhetskopiering. När du skapar en säkerhetskopia (som krävs eller schemalagda), lagras aldrig meddelanden i batchar. 
  
    **Om en säkerhetskopia görs med aktiva meddelanden i en grupp, säkerhetskopieras inte dessa meddelanden och går därför förlorade.**
* Valfritt: I BizTalk Services-portalen, sluta alla hanteringsåtgärder.

## <a name="create-a-backup"></a>Skapa en säkerhetskopia
En säkerhetskopia kan utföras när som helst och styrs helt av dig. Du kan skapa en säkerhetskopia med den [REST API för att hantera BizTalk Services i Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

## <a name="restore"></a>Återställ
Om du vill återställa en säkerhetskopia, använda den [REST API för att hantera BizTalk Services i Azure](https://msdn.microsoft.com/library/azure/dn232347.aspx).

### <a name="postrestore"></a>När du återställer en säkerhetskopia
BizTalk Service återställs alltid i en **pausad** tillstånd. I det här tillståndet kan du göra några konfigurationsändringar innan fungerar i den nya miljön, inklusive:

* Om du har skapat BizTalk Service-program med hjälp av Azure BizTalk Services SDK kan du behöva uppdatera autentiseringsuppgifterna för ACS (Access Control) i programmen för att arbeta med den återställda miljön.
* Du återställer en BizTalk Service för att replikera en befintlig BizTalk Service-miljö. I det här fallet om det finns avtal som konfigurerats i den ursprungliga BizTalk Services-portalen och som använder FTP källmappen, kan du behöva uppdatera avtal i den nyligen återställda miljön att använda en annan källa FTP-mapp. Annars kan finnas det två andra avtal som försöker hämta samma meddelande.
* Om du har återställt om du vill ha flera BizTalk Service-miljöer, kontrollera att du riktar rätt miljö i Visual Studio-program, PowerShell-cmdletar, REST API: er eller Trading Partner Management OM API: er.
* Det är en bra idé att konfigurera automatiska säkerhetskopieringar på den nyligen återställda BizTalk Service-miljön.

## <a name="what-gets-backed-up"></a>Vad säkerhetskopieras
När en säkerhetskopia skapas som följande objekt säkerhetskopieras:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Objekt som säkerhetskopieras</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services-portalen</strong></td>
</tr> 
<tr>
<td>Konfiguration och Runtime</td> 
<td>
<ul>
<li>Information om partner och profil</li>
<li>Avtal för partner</li>
<li>Anpassade sammansättningar som distribuerats</li>
<li>Bryggor distribueras</li>
<li>Certifikat</li>
<li>Transformeringar som distribuerats</li>
<li>Pipelines</li>
<li>Mallar som skapats och sparats i BizTalk Services-portalen</li>
<li>X12 ST01 och GS01 mappningar</li>
<li>Kontrollnummer (EDI)</li>
<li>AS2-meddelandet MIC-värdena</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk-tjänst</strong></td>
</tr> 
<tr>
<td>SSL-certifikat</td> 
<td>
<ul>
<li>SSL-certifikat-Data</li>
<li>Lösenord för SSL-certifikat</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk Services-inställningar</td> 
<td>
<ul>
<li>Skalningsantal för enhet</li>
<li>Utgåva</li>
<li>Produktversion</li>
<li>Region/Datacenter</li>
<li>Access Control Service (ACS)-namnområdet och nyckel</li>
<li>Spåra databasanslutningssträng</li>
<li>Arkivering anslutningssträng för Lagringskonto</li>
<li>Övervakning av anslutningssträng för lagringskonto</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Ytterligare objekt</strong></td>
</tr> 
<tr>
<td>Spårning av databasen</td> 
<td>När BizTalk Service skapas, anges spåra databasinformationen, inklusive Azure SQL Database-Server och spåra databasens namn. Spårning av databasen säkerhetskopieras inte automatiskt.
<br/><br/>
<strong>Viktigt</strong><br/>
Om spårning av databasen tas bort och databasen måste återställas, måste det finnas en tidigare säkerhetskopia. Om en säkerhetskopia inte finns, är det inte återställas med spårning av databasen och dess data. I så fall kan du skapa en ny spårning-databas med samma databasnamn. GEO-replikering rekommenderas.</td>
</tr> 
</table>

## <a name="next"></a>Nästa
För att skapa Azure BizTalk Services, går du till [BizTalk Services: etablering](http://go.microsoft.com/fwlink/p/?LinkID=302280). Om du vill börja skapa program går du till [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Se även
* [Säkerhetskopiera BizTalk-tjänst](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Återställa BizTalk-tjänst från en säkerhetskopia](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard och Premium diagram över utgåvor](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: etablering](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Etablering av statusdiagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Begränsning](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Utfärdarens namn och nyckel](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hur gör jag för att börja använda Azure BizTalk Services SDK?](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png


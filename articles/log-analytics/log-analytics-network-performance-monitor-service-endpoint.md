---
title: Network Performance Monitor-lösningen i Azure Log Analytics | Microsoft Docs
description: Använd funktionen för övervakning av tjänstens anslutning i Övervakare av nätverksprestanda för att övervaka nätverksanslutning till valfri slutpunkt som har en öppen TCP-port.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: ''
ms.openlocfilehash: fb84b20630eb63cb53ccb1d13a383ed6287b802b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406627"
---
# <a name="service-connectivity-monitor"></a>Övervakare av tjänstanslutning

Du kan använda funktionen för tjänsten Anslutningsövervakare [Övervakare av nätverksprestanda](log-analytics-network-performance-monitor.md) att övervaka nätverksanslutningar till vilken slutpunkt som har en öppen TCP-port. Sådana slutpunkter är webbplatser, SaaS-program, PaaS-program och SQL-databaser. 

Du kan utföra följande funktioner med övervakning av tjänstens anslutning: 

- Övervaka nätverksanslutningar till dina program och nätverkstjänster från flera kontor och avdelningskontor. Program och nätverkstjänster omfattar Office 365, Dynamics CRM, interna line-of-business-program och SQL-databaser.
- Använd inbyggda tester för att övervaka nätverksanslutningar till Office 365 och Dynamics 365-slutpunkter. 
- Fastställa svarstid, nätverkssvarstid och paketförlust erfarna när du ansluter till slutpunkten.
- Avgöra om dålig programprestanda finns på grund av nätverket eller på grund av vissa problem på program-leverantörens slutet.
- Identifiera aktiva punkter i nätverket som kan orsaka dålig programprestanda genom att visa den svarstid som tillförts av varje hopp på en topologisk karta.


![Övervakare av tjänstanslutning](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Konfiguration 
För att öppna konfigurationen för Övervakare av nätverksprestanda, öppna den [Network Performance Monitor-lösningen](log-analytics-network-performance-monitor.md) och välj **konfigurera**.

![Konfigurera Övervakare av nätverksprestanda](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>Konfigurera Log Analytics-agenter för övervakning
Aktivera följande brandväggsregler på noder som används för att övervaka så att lösningen kan identifiera topologin från dina noder till tjänsteslutpunkt: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Skapa tester för tjänsten anslutning övervakning 

Börja skapa dina tester för att övervaka nätverksanslutningar till Tjänsteslutpunkter.

1. Välj den **Service Anslutningsövervakare** fliken.
2. Välj **Lägg till testa**, och ange namn och beskrivning. 
3. Välj typ av test:<br>

    * Välj **Web** att övervaka anslutningen till en tjänst som svarar på HTTP/S-begäranden, till exempel outlook.office365.com eller bing.com.<br>
    * Välj **nätverk** att övervaka anslutningen till en tjänst som svarar på förfrågningar TCP men inte svarar på HTTP/S-begäranden, till exempel en SQLServer, FTP-servern eller SSH-porten. 
4. Om du inte vill att utföra nätverksmätningar som Nätverksfördröjningen, paketförlust och identifiering av topologi, avmarkera de **utför nätverksmätningar** markerar du kryssrutan. Behåll den markerad för att få största möjliga nytta från funktionen. 
5. I **Target**, ange URL-Adressen/FQDN/IP-adressen som du vill övervaka nätverksanslutningar.
6. I **portnummer**, ange portnumret för Måltjänsten. 
7. I **testa frekvens**, ange ett värde för hur ofta du vill att testet ska köras. 
8. Markera de noder som du vill övervaka nätverksanslutningar till tjänsten. 

    >[!NOTE]
    > För Windows server-baserade noderna använder funktionen för TCP-baserade förfrågningar för att utföra nätverksmätningar. För Windows klientbaserade noder använder funktionen för ICMP-baserade begäranden för att utföra nätverksmätningar. I vissa fall kan blockerar målprogrammet inkommande ICMP-baserade begäranden när noderna är Windows klientbaserade. Lösningen kan inte utföra nätverksmätningar. Vi rekommenderar att du använder Windows server-baserade noder i sådana fall. 

9. Om du inte vill skapa health-händelser för objekt du väljer, rensa **aktivera hälsoövervakning för mål som omfattas av det här testet**. 
10. Välj övervakning villkor. Du kan ange anpassade tröskelvärden för health händelsegenerering genom att ange tröskelvärden. När värdet för villkoret går över det valda tröskelvärdet för den valda nätverk eller undernätverk par, genereras en hälsotillståndshändelse. 
11. Välj **spara** att spara konfigurationen. 

    ![Tjänsten Anslutningsövervakare testkonfigurationer](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Genomgång 

Gå till instrumentpanelsvyn för övervakning av nätverksprestanda. För att få en översikt över hälsotillståndet för de olika testerna som du har skapat kan du titta på den **Service Anslutningsövervakare** sidan. 

![Tjänsten Anslutningsövervakare sidan](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Markera panelen för att visa information om testerna på den **tester** sidan. Du kan visa hälsotillstånd för point-in-time och värdet av tjänstens svarstid, nätverkssvarstid och paketförlust för alla tester i tabellen till vänster. Använda nätverket tillstånd Recorder kontrollen för att visa nätverk ögonblicksbilden vid ett senare tillfälle i förflutna. Välj testet i tabellen som du vill undersöka. Du kan visa historiska trenden för den förlust och fördröjning svar tidsvärden i diagrammen i rutan till höger. Välj den **Test information** länken för att visa prestanda från varje nod.

![Tester för tjänsten anslutning övervakning](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

I den **Testnoder** vy, kan du se nätverksanslutning från varje nod. Välj den nod som har försämrade prestanda. Det här är den nod där programmet observeras körs långsamt.

Avgöra om dålig programprestanda finns på grund av nätverket eller ett problem på leverantören programmet att göra genom att följa sambandet mellan svarstiden för programmet och svarstiden i nätverk. 

* **Problem med programmet:** en topp i svarstiden men konsekvens i Nätverksfördröjningen föreslår att nätverket fungerar bra och problemet kan bero på ett problem på slutet program. 

    ![Tjänsten Anslutningsövervakare programproblem](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

* **Network problemet:** en topp i svarstid som åtföljs av en motsvarande topp i Nätverksfördröjningen tyder på att ökningen av svarstiden kan bero på en ökning av Nätverksfördröjningen. 

    ![Tjänsten Anslutningsövervakare nätverksproblem](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

När du har fastställt att problemet ligger på grund av nätverket, Välj den **topologi** visningslänk att identifiera problematiska hopp på topologisk karta. I följande bild visas ett exempel. Av den totala svarstiden 105 ms mellan noden och programslutpunkt beror 96 ms hopp markerat i rött. När du har identifierat det problematiska hoppet kan du vidta lämpliga åtgärder. 

![Tester för tjänsten anslutning övervakning](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostik 

Om du upptäcker en avvikelse, gör du följande:

* Om den tjänstens svarstid, nätverksförluster och fördröjning visas som NA, kan en eller flera av följande orsaker vara orsaken:

    - Programmet har stoppats.
    - Noden som används för att kontrollera nätverksanslutningen till tjänsten har stoppats.
    - Målet som angetts i testkonfigurationen är felaktig.
    - Noden har inte någon nätverksanslutning.

* Om en giltig tjänstens svarstid visas men nätverksförluster samt svarstid visas som NA, kan en eller flera av följande orsaker vara orsaken:

    - Om noden som används för att kontrollera nätverksanslutningen till tjänsten är en Windows-klientdator, Måltjänsten blockerar ICMP-begäranden, eller en brandvägg blockerar ICMP-begäranden som kommer från noden.
    - Den **utför nätverksmätningar** kryssrutan är tom i testkonfigurationen. 

* Om tjänstens svarstid är NA men nätverksförluster samt svarstid är giltiga, kanske inte target-tjänsten ett webbprogram. Redigera testkonfigurationen och välj testtypen som **nätverk** i stället för **Web**. 

* Om programmet är långsamt, kan du avgöra om dålig programprestanda beror på att nätverket eller ett problem på leverantören programmet att göra.


## <a name="next-steps"></a>Nästa steg
[Söka loggarna](log-analytics-log-searches.md) att visa detaljerad nätverk prestanda dataposter.

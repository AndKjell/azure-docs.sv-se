<properties title="Load Balancing for Azure Infrastructure Services" pageTitle="Lastenausgleich für Azure-Infrastrukturdienste" description="Beschreibt die Funktionen zum Ausführen des Lastenausgleichs mit Traffic Manager und dem Lastenausgleich." metaKeywords="" services="virtual-machines" solutions="" documentationCenter="" authors="cherylmc" videoId="" scriptId="" manager="adinah" />

<tags ms.service="virtual-machines" ms.workload="infrastructure-services" ms.tgt_pltfrm="" ms.devlang="na" ms.topic="article" ms.date="09/17/2014" ms.author="cherylmc" />

#Lastenausgleich für Azure-Infrastrukturdienste#

Für Azure-Infrastrukturdienste kann Lastenausgleich auf zwei Ebenen genutzt werden:

- **DNS-Ebene**: Lastenausgleich für Datenverkehr zu unterschiedlichen Clouddiensten in unterschiedlichen Rechenzentren, zu unterschiedlichen Azure-Websites befindet sich in unterschiedlichen Rechenzentren oder zu externen Endpunkten. Für diese Form des Lastenausgleichs werden der Traffic Manager und die Roundrobin-Methode verwendet.
- **Netzwerkebene**: Lastenausgleich für eingehenden Internetdatenverkehr zu unterschiedlichen virtuellen Computern eines Cloud-Diensts oder Lastenausgleich des Datenverkehrs zwischen virtuellen Computern in einem Clouddienst oder virtuellen Netzwerk. Für diese Aufgaben wird das Azure-Lastenausgleichsmodul verwendet.

##Traffic Manager-Lastenausgleich für Clouddienste und Websites##

Mit Azure Traffic Manager können Sie die Verteilung des Benutzerdatenverkehrs an Endpunkte steuern, z. B. an Clouddienste, Websites externe Websites und andere Traffic Manager-Profile. Die Funktionsweise von Traffic Manager basiert darauf, dass Sie ein intelligentes Richtlinienmodul auf DNS-Abfragen (Domain Name System) von Domänennamen Ihrer Internetressourcen anwenden. Die Clouddienste oder Websites können in verschiedenen Rechenzentren auf der ganzen Welt ausgeführt werden. 

Sie müssen entweder REST oder Windows PowerShell zur Konfiguration externer Endpunkte oder Traffic Manager-Profile als Endpunkte verwenden. 

Azure Traffic Manager verwendet drei Lastenausgleichsmethoden, um den Datenverkehr zu verteilen:

- **Failover**: Verwenden Sie diese Methode, um einen primären Endpunkt für den gesamten Datenverkehr verwenden, aber Sicherungen bereitstellen, für den Fall, dass die primäre nicht mehr verfügbar ist.
- **Leistung**: Verwenden Sie diese Methode, wenn sich die Endpunkte an unterschiedlichen geografischen Standorten befinden und anfordernde Clients den "nächstgelegenen" Endpunkt im Hinblick auf die niedrigste Latenz verwendet werden soll.
- **Round-Robin:**  Verwenden Sie diese Methode, wenn Sie die Last auf eine Reihe von Cloud-Diensten im gleichen Datencenter oder auf Clouddienste oder Websites in verschiedenen Datencentern verteilen möchten.

Weitere Informationen finden Sie unter [zu Traffic Manager-Lastenausgleichsmethoden](../traffic-manager/traffic-manager-load-balancing-methods.md).

Die folgende Abbildung zeigt ein Beispiel für die Roundrobin-Lastenausgleichsmethode, bei der der Datenverkehr zwischen unterschiedlichen Clouddiensten verteilt wird.

![Lastenausgleich](./media/load-balancing-vms/TMSummary.png)

Im Folgenden die grundlegende Vorgehensweise:

1.  Ein Internetclient fragt einen Domänennamen ab, der einem Webdienst entspricht.
2.  DNS leitet die Namensabfrage an den Traffic Manager weiter.
3.  Der Traffic Manager gibt den DNS-Namen des Clouddiensts in der Roundrobinliste zurück. Der DNS-Server des Internetclients löst den Namen in eine IP-Adresse auf und sendet diese an den Internetclient.
4.  Der Internetclient stellt eine Verbindung mit dem ausgewählten Clouddienst her.

## Azure-Lastenausgleich für virtuelle Computer ##

Virtuelle Computer im selben Clouddienst oder virtuellen Netzwerk können über ihre privaten IP-Adressen direkt miteinander kommunizieren. Computer und Dienste außerhalb des Clouddiensts oder virtuellen Netzwerks können nur mit virtuellen Computern in einem Clouddienst oder virtuellen Netzwerk mit einem konfigurierten Endpunkt kommunizieren. Ein Endpunkt ist eine Zuordnung einer öffentlichen IP-Adresse und eines Ports zu der privaten IP-Adresse und dem Port eines virtuellen Computers oder einer Webrolle innerhalb eines Azure-Clouddiensts.

Das Azure-Lastenausgleichsmodul verteilt bestimmte eingehende Datenverkehrsdaten nach dem Zufallsprinzip auf mehrere virtuelle Computer oder Dienste. Diese Konfiguration wird als Gruppe mit Lastenausgleich bezeichnet. Sie können zum Beispiel die Netzwerklast von Webanfragen auf mehrere Webserver oder Webrollen verteilen.

Die folgende Abbildung zeigt einen Endpunkt für Standard-Datenverkehr (unverschlüsselt) mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen und privaten TCP-Port 80 genutzt wird. Diese drei virtuellen Computer bilden einen Satz mit Lastenausgleich.

![loadbalancing](./media/load-balancing-vms/LoadBalancing.png)

Weitere Informationen finden Sie unter [Azure-Lastenausgleich](../articles/load-balancer/load-balancer-overview.md). Die Schritte zum Erstellen eines Satzes mit Lastenausgleich finden Sie unter [eine Gruppe mit Lastenausgleich konfigurieren](../load-balancer/load-balancer-overview.md).

Azure ist auch in der Lage, Lasten innerhalb eines Clouddiensts oder virtuellen Netzwerks auszugleichen. Diese Methode wird als interner Lastenausgleich bezeichnet und kann wie folgt verwendet werden:

- Lastenausgleich zwischen Servern auf unterschiedlichen Ebenen einer Multi-Tier-Anwendung (beispielsweise zwischen der Web- und Datenbankebene).
- Lastenausgleich für Branchenanwendungen (LOB-Anwendungen), die in Azure gehostet werden, ohne dass zusätzliche Hardware oder Software für den Lastenausgleich erforderlich ist. 
- Einschließen von lokalen Servern in die Gruppe der Computer, für deren Datenverkehr Lastenausgleich stattfindet.

Vergleichbar mit dem Azure-Lastenausgleich ist auch ein interner Lastenausgleich möglich, indem eine interne Gruppe mit Lastenausgleich konfiguriert wird. 

Die folgende Abbildung zeigt ein Beispiel eines internen Endpunkts mit Lastenausgleich für eine Branchenanwendung (LOB-Anwendung), die in einem standortübergreifenden virtuellen Netzwerk von drei virtuellen Computern gemeinsam genutzt wird. 

![loadbalancing](./media/load-balancing-vms/LOBServers.png)

Weitere Informationen finden Sie unter [Interner Lastenausgleich](../load-balancer/load-balancer-internal-overview.md). Die Schritte zum Erstellen eines Satzes mit Lastenausgleich finden Sie unter [einen internen Lastenausgleich konfigurieren](../load-balancer/load-balancer-internal-getstarted.md).

<!-- LINKS -->



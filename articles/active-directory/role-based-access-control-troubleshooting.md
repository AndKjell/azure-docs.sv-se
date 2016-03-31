<properties
    pageTitle="Behandlung von Problemen bei der rollenbasierten Zugriffssteuerung"
    description="Arbeiten mit unterschiedlichen Ressourcentypen bei der rollenbasierten Zugriffssteuerung."
    services="azure-portal"
    documentationCenter="na"
    authors="IHenkel"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="inhenk"/>

# Behandlung von Problemen bei der rollenbasierten Zugriffssteuerung

## Einführung

[Rollenbasierte Zugriffskontrolle](../role-based-access-control-configure.md) ist ein leistungsstarkes Feature, mit dem Sie differenzierten Zugriff auf Ressourcen in Azure delegieren kann. Gewähren Sie einem bestimmten Benutzer einfach und sicher Zugriff auf genau die Ressourcen, die er benötigt. In einigen Fällen kann das Ressourcen-Modell für Azure-Ressourcen jedoch kompliziert sein, und es ist nicht immer leicht ersichtlich, wofür Sie Berechtigungen vergeben.

In diesem Dokument erhalten Sie ausführliche Informationen über die Nutzung einiger der Rollen im klassischen Azure-Portal. Es sind drei allgemeine Rollen enthalten, die alle Ressourcentypen abdecken:
* Besitzer
* Mitwirkender
* Leser

Besitzer und Mitwirkende haben Vollzugriff auf alle Verwaltungsfunktionen, mit dem Unterschied, dass Mitwirkende anderen Benutzern oder Gruppen keinen Zugriff gewähren können. Die Leserolle werden wir aufgrund ihrer umfassenden Eigenschaften ausführlicher beschreiben. [Dieser Artikel](../role-based-access-control-configure.md) ausführliche Informationen genau, um Zugriff zu gewähren.

## App Service-Workloads

### Nur Lesezugriff

Wenn Sie selbst nur über Lesezugriff auf eine Web-App verfügen oder einem Benutzer nur Lesezugriff auf eine Web-App gewähren, sind eventuell einige Funktionen für Sie unerwartet deaktiviert. Die folgenden Verwaltungsfunktionen erfordern **Schreiben** Zugriff auf eine Web-app (Mitwirkender oder Besitzer) und nicht zur Verfügung, die in jedem lesen nur Szenario.

1. Befehle (z. B. starten, anhalten)
2. Änderung von Einstellungen, wie allgemeine Konfigurationen, Skalierungseinstellungen, Sicherungseinstellungen und Überwachungseinstellungen
3. Zugriff auf Anmeldedaten für die Veröffentlichung oder andere geheime Schlüssel wie App- und Verbindungseinstellungen
4. Streamingprotokolle
5. Konfiguration von Diagnoseprotokollen
6. Konsole (Eingabeaufforderung)
7. Aktive und kürzlich vorgenommene Bereitstellungen (für fortlaufende Bereitstellungen lokaler Gits)
8. Geschätzte Ausgaben
9. Webtests
10. Virtuelles Netzwerk (für Leser nur sichtbar, wenn ein virtuelles Netzwerk zuvor von einem Benutzer mit Schreibzugriff konfiguriert wurde)

Wenn Sie auf keine dieser Kacheln zugreifen können, benötigen Sie Zugriff als Mitwirkender auf diese Web-App.

### Umgang mit zugehörigen Ressourcen

Web-Apps können aufgrund verschiedener miteinander verknüpfter Ressourcen kompliziert sein. Im Folgenden ist eine typische Ressourcengruppe mit einigen Websites dargestellt:

![Web-App-Ressourcengruppe](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Wenn Sie daher lediglich auf die Website Zugriff gewähren, sind viele Funktionen des Blatts "Website" vollständig deaktiviert.

1. Diese Elemente erfordern Zugriff auf die **App Service-Plan** die Ihre Website entspricht:  
    * Anzeigen des Tarifs für die Web-App (z. B. Free oder Standard)
    * Skalierungskonfiguration (d. h. Anzahl der Instanzen, Größe des virtuellen Computers, Einstellungen für automatische Skalierung)
    * Kontingente (z. B. Speicher, Bandbreite, CPU)
2. Die folgenden Elemente erfordern Zugriff auf die gesamte **Ressourcengruppe** die Ihre Website enthält:  
    * SSL-Zertifikate und -Bindungen (Der Grund dafür ist, dass SSL-Zertifikate von Websites derselben Ressourcengruppe und desselben geografischen Standorts gemeinsam genutzt werden können).
    * Warnregeln
    * Einstellungen für automatische Skalierung
    * Application Insights-Komponenten
    * Webtests

## Workloads virtueller Computer

Ähnlich wie bei Web-Apps erfordern einige Funktionen auf dem Blatt "Virtueller Computer" Schreibzugriff auf den virtuellen Computer oder auf andere Ressourcen in der Ressourcengruppe.

Virtuelle Computer verfügen über folgende zugehörige Ressourcen:
* Domänennamen
* Virtuelle Netzwerke
* Speicherkonten
* Warnungsregeln

1. Die folgenden Elemente erfordern **Schreiben** Zugriff auf den virtuellen Computer:  
    * Endpunkte
    * IP-Adressen
    * Datenträger
    * Erweiterungen
2. Elemente erfordern Schreibzugriff beide virtuellen Computer, und die **Ressourcengruppe** (zusammen mit dem Domänennamen) wird:  
    * Verfügbarkeitsgruppe
    * Satz mit Lastenausgleich
    * Warnungsregeln

Wenn Sie auf keine dieser Kacheln zugreifen können, fragen Sie den Administrator nach Zugriff als Mitwirkender auf diese Ressourcengruppe.



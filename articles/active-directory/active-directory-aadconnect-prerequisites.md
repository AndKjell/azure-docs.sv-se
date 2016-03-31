<properties
   pageTitle="Azure AD Connect: Voraussetzungen und Hardware | Microsoft Azure"
   description="Artikelbeschreibung, die auf Angebotsseiten und für die meisten Suchergebnisse angezeigt wird."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="stevenpo"
   editor="curtand"/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="11/16/2015"
   ms.author="andkjell;billmath"/>

# Voraussetzungen für Azure AD Connect
Dieses Thema beschreibt die Voraussetzungen und die Hardwareanforderungen für Azure AD Connect.

## Vor der Installation von Azure AD Connect
Vor der Installation von Azure AD Connect gibt es einige Dinge, die Sie benötigen.

**Azure AD**

- Ein Azure-Abonnement oder ein [Azure-Testabonnement](http://azure.microsoft.com/pricing/free-trial/). Dies ist nur erforderlich für den Zugriff auf das Azure-Portal und nicht für die Verwendung von Azure AD Connect.  Bei Verwendung von PowerShell oder Office 365 benötigen Sie für Azure AD Connect kein Azure-Abonnement. Wenn Sie über eine Office 365-Lizenz verfügen, können Sie auch das Office 365-Portal verwenden. Mit einer kostenpflichtigen Office 365-Lizenz können Sie auch über das Office 365-Portal auf das Azure-Portal zugreifen.
- [Hinzufügen und überprüfen die Domäne](active-directory-add-domain.md) in Azure AD verwenden möchten. Wenn Sie beispielsweise planen, "contoso.com" für Ihre Benutzer zu verwenden, sollten Sie sicherstellen, dass diese Domäne überprüft wurde und nicht nur die Standarddomäne "contoso.onmicrosoft.com" verwendet wird.
- Ein Azure AD-Verzeichnis lässt standardmäßig 50.000 Objekte zu. Beim Überprüfen der Domäne wird die Beschränkung auf 300.000 Objekte erhöht. Wenn Sie noch mehr Objekte in Azure AD benötigen, müssen Sie eine Supportanfrage eröffnen, um den Grenzwert noch weiter erhöhen zu lassen. Wenn Sie mehr als 500.000 Objekte benötigen, benötigen Sie eine Lizenz, z. B. Office 365, Azure AD Basic, Azure AD Premium oder Enterprise Mobility Suite.

**Lokale Server und Umgebung**

- Die AD-Schemaversion und Funktionsebene der Gesamtstruktur müssen Windows Server 2003 oder höher entsprechen. Die Domänencontroller können eine beliebige Version ausführen, solange die Anforderungen an Schema und Gesamtstrukturebene erfüllt werden.
- Wenn Sie planen, verwenden Sie die Funktion **das Rückschreiben von Kennwörtern** Domänencontroller muss auf Windows Server 2008 (mit dem aktuellsten SP) oder höher sein.
- Azure AD Connect kann nicht auf dem Small Business Server oder Windows Server Essentials installiert werden. Der Server muss Windows Server Standard oder höher verwenden.
- Azure AD Connect muss unter Windows Server 2008 oder höher installiert werden.  Dieser Server kann bei Verwendung der Expresseinstellungen ein Domänencontroller oder ein Mitgliedsserver sein. Wenn Sie benutzerdefinierte Einstellungen verwenden, kann der Server auch eigenständig sein und muss nicht mit einer Domäne verknüpft werden.
- Wenn Sie Azure AD Connect unter Windows Server 2008 installieren, achten Sie darauf, die neuesten Hotfixes über Windows Update anzuwenden. Die Installation kann mit einem nicht gepatchten Server nicht gestartet werden.
- Wenn Sie planen, verwenden Sie die Funktion **Synchronisierung von Kennwörtern**, der Azure AD Connect-Server muss unter Windows Server 2008 R2 SP1 oder höher sein.
- Azure AD Connect-Server müssen [.Net 4.5.1](#component-prerequisites) oder höher und [PowerShell 3.0](#component-prerequisites) oder höher installiert sein.
- Wenn Active Directory-Verbunddienste bereitgestellt werden, müssen die Server, auf denen die Active Directory-Verbunddienste oder der Webanwendungsproxy installiert werden, Windows Server 2012 R2 oder höher ausführen. [Windows-Remoteverwaltung](#windows-remote-management) muss auf diesen Servern für remote-Installation aktiviert werden.
- Wenn Active Directory Federation Services bereitgestellt wird, müssen Sie [SSL-Zertifikate](#ssl-certificate-requirements).
- Azure AD Connect erfordert eine SQL Server-Datenbank zum Speichern von Identitätsdaten. Standardmäßig wird SQL Server 2012 Express LocalDB (eine Light-Version von SQL Server Express) installiert, und das Dienstkonto für den Dienst wird auf dem lokalen Computer erstellt. Für SQL Server Express gilt ein 10-GB-Limit, mit dem Sie etwa 100.000 Objekte verwalten können. Wenn Sie eine höhere Anzahl von Verzeichnisobjekten verwalten möchten, müssen Sie während der Installation auf eine andere Version von SQL Server verweisen.  Azure AD Connect unterstützt sämtliche Versionen von Microsoft SQL Server – von SQL Server 2008 (mit SP4) bis hin zu SQL Server 2014.

**Konten**

- Ein globales Azure AD-Administratorkonto für das Azure AD-Verzeichnis, das Sie integrieren möchten.
- Ein Enterprise-Administratorkonto für Ihr lokales Active Directory, wenn Sie Expresseinstellungen verwenden oder von DirSync upgraden.
- [Konten ist Active Directory](active-directory-aadconnect-accounts-permissions.md) Wenn Sie den Installationspfad für benutzerdefinierte Einstellungen verwenden.

**Konnektivität**

- Wenn Sie einen ausgehenden Proxyserver verwenden für die Verbindung mit dem Internet, die folgende Einstellung in der **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** Datei muss für den Installations-Assistenten und Azure AD synchronisieren, um die Verbindung mit dem Internet und Azure AD können hinzugefügt werden. Dieser Text muss am Ende der Datei eingegeben werden.  In diesem Codeabschnitt steht „&lt;PROXYADRESS&gt;“ für die Proxy-IP-Adresse oder den Hostnamen.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Wenn Ihr Proxyserver eine Authentifizierung erfordert, sollte dieser Abschnitt stattdessen wie folgt aussehen.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Durch diese Änderung in „machine.config“ antworten der Installationsassistent und das Synchronisierungsmodul auf Authentifizierungsanfragen des Proxyservers. Alle Installation Seiten des Assistenten, ausgenommen die **konfigurieren** Seite signierten Benutzers Anmeldeinformationen verwendet werden. Auf der **konfigurieren** Seite am Ende des Installations-Assistenten, den Kontext einer Unterbrechung der [-Dienstkonto](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) das erstellt wurde.

Siehe MSDN für Weitere Informationen zu den [Proxy Element standardmäßig](https://msdn.microsoft.com/library/kd3cf2ex.aspx).

- Wenn der Proxy welche URLs einschränkt zugreifen können dann die URLs in dokumentiert [Office 365-URLs und IP-Adressbereiche ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) im Proxy geöffnet werden müssen.

**Andere**

- Optional: Ein Testbenutzerkonto zur Überprüfung der Synchronisierung.

## Voraussetzungen an Komponenten

Azure AD Connect ist abhängig von PowerShell und .NET 4.5.1. Führen Sie abhängig von Ihrer Windows Server-Version folgende Schritte aus:

- Windows Server 2012 R2
  - PowerShell ist standardmäßig installiert, es ist keine Aktion erforderlich.
  - .NET 4.5.1 und neuere Versionen werden über Windows Update angeboten. Stellen Sie in der Systemsteuerung sicher, dass die neuesten Updates von Windows Server installiert sind.
- Windows Server 2008 R2 und Windows Server 2012
  - Die neueste Version von PowerShell ist verfügbar in **Windows Management Framework 4.0**, verfügbar auf [Microsoft Download Center](http://www.microsoft.com/downloads).
  - .NET 4.5.1 und höheren Versionen stehen auf [Microsoft Download Center](http://www.microsoft.com/downloads).
- Windows Server 2008
  - Die neueste unterstützte Version von PowerShell ist verfügbar in **Windows Management Framework 3.0**, auf [Microsoft Download Center](http://www.microsoft.com/downloads).
 - .NET 4.5.1 und höheren Versionen stehen auf [Microsoft Download Center](http://www.microsoft.com/downloads).

## Windows-Remoteverwaltung

 Überprüfen Sie bei Verwendung von Azure AD Connect für die Bereitstellung von Active Directory-Verbunddiensten oder des Webanwendungsproxys die unten aufgeführten Anforderungen, um die Konnektivität und eine erfolgreiche Konfiguration sicherzustellen:

 - Wenn der Zielserver mit der Domäne verknüpft ist, stellen Sie sicher, dass die Windows-Remoteverwaltung aktiviert ist.
    - Verwenden Sie in einem PSH-Befehlsfenster mit erhöhten Rechten den Befehl `Enable-PSRemoting –force`.
 - Wenn der Zielserver kein mit der Domäne verknüpfter WAP-Computer ist, gibt es einige zusätzliche Anforderungen.
    - Auf dem Zielcomputer (WAP-Computer): 
         - Stellen Sie sicher, dass der WinRM-Dienst (Windows-Remoteverwaltung/WS-Verwaltung) über das Dienste-Snap-In ausgeführt wird.
         - Verwenden Sie in einem PSH-Befehlsfenster mit erhöhten Rechten den Befehl `Enable-PSRemoting –force`.
    - Auf dem Computer, auf dem der Assistent ausgeführt wird (wenn der Zielcomputer nicht der Domäne beigetreten ist oder sich in einer nicht vertrauenswürdigen Domäne befindet):
        - Verwenden Sie in einem PSH-Befehlsfenster mit erhöhten Rechten den Befehl `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`.
        - In Server Manager:
             - Fügen Sie den DMZ-WAP-Host zum Computerpool hinzu (Server-Manager -> "Verwalten" -> "Server hinzufügen"-> Registerkarte "DNS verwenden").
             - Server-Manager, Registerkarte "Alle Server": Klicken Sie mit der rechten Maustaste auf "WAP-Server", und wählen Sie "Verwalten als" aus. Geben Sie dann die Anmeldeinformationen für den lokalen WAP-Computer ein (nicht für die Domäne).
             - Um die PSH-Remotekonnektivität zu überprüfen, klicken Sie auf der Registerkarte "Alle Server" mit der rechten Maustaste auf "WAP-Server", und wählen Sie "Windows PowerShell" aus.  Daraufhin sollte sich eine PSH-Remotesitzung öffnen, um sicherzustellen, dass PowerShell-Remotesitzungen hergestellt werden können.

## SSL-Zertifikatanforderungen

**Wichtig:** Es wird dringend empfohlen, um dasselbe SSL-Zertifikat für alle Knoten des AD FS-Farm sowie alle webanwendungsproxy-Server zu verwenden.

- Das Zertifikat muss ein X.509-Zertifikat sein.
- Sie können ein selbstsigniertes Zertifikat für Verbundserver in einer Testumgebung verwenden. Bei einer Produktionsumgebung wird jedoch empfohlen, dass Sie das Zertifikat von einer öffentlichen Zertifizierungsstelle beziehen.
    - Wenn Sie ein Zertifikat verwenden, das nicht öffentlich vertrauenswürdig ist, stellen Sie sicher, dass dem auf jedem Webanwendungsproxy-Server installierten Zertifikat sowohl auf dem lokalen Server als auch auf allen Verbundservern vertraut wird.
- Die Identität des Zertifikats muss mit dem Namen des Verbunddiensts (z. B. "fs.contoso.com") übereinstimmen.
    - Die Identität ist die Erweiterung eines  alternativen Antragstellernamens (SAN) des Typs "dNSName". Wenn keine SAN-Einträge vorhanden sind, wird der Name des Antragstellers als allgemeiner Name angegeben.  
    - Im Zertifikat können mehrere SAN-Einträge hinterlegt werden, sofern einer von ihnen mit dem Namen des Verbunddiensts übereinstimmt.
    - Wenn Sie beabsichtigen, die Arbeitsplatzbeitritt verwenden, muss ein zusätzlicher SAN mit dem Wert **Enterpriseregistration.** gefolgt vom Suffix UPN (User Principal Name, Benutzerprinzipalname) Ihrer Organisation, z. B. **enterpriseregistration.contoso.com**.
- Auf CNG-Schlüsseln (CryptoAPI Next Generation) und Schlüsselspeicheranbietern basierende Zertifikate werden nicht unterstützt. Dies bedeutet, dass Sie ein CSP-basiertes Zertifikat (Kryptografie-Service Provider) verwenden müssen, kein Zertifikat von einem Schlüsselspeicheranbieter.
- Platzhalterzertifikate werden unterstützt.

## Azure AD Connect unterstützende Komponenten

Nachfolgend finden Sie eine Liste der Komponenten, die Azure AD Connect auf dem Server installiert, auf dem Azure AD Connect installiert ist. Diese Liste gilt für eine einfache Expressinstallation.  Wenn Sie eine andere SQL Server-Version auf der Installationsseite für Synchronisierungsdienste verwenden möchten, wird die SQL Express LocalDB nicht lokal installiert.

- Microsoft SQL Server 2012 – Befehlszeilenprogramme
- Microsoft SQL Server 2012 Native Client
- Microsoft SQL Server 2012 Express LocalDB
- Azure Active Directory-Modul für Windows PowerShell
- Microsoft Online Services-Anmelde-Assistent für IT-Experten
- Microsoft Visual C++ 2013 Redistributionspaket


## Hardwareanforderungen für Azure AD Connect
Die folgende Tabelle zeigt die Mindestanforderungen für den Azure AD Connect-Synchronisierungscomputer.

| Anzahl der Objekte in Active Directory | CPU | Arbeitsspeicher | Festplattengröße |
| ------------------------------------- | --- | ------ | --------------- |
| Weniger als 10.000 | 1,6 GHz | 4 GB | 70 GB |
| 10.000 bis 50.000 | 1,6 GHz | 4 GB | 70 GB |
| 50.000 bis 100.000 | 1,6 GHz | 16 GB | 100 GB |
| Für 100.000 oder mehr Objekte ist die Vollversion von SQL Server erforderlich|  |  |  |
| 100.000 bis 300.000 | 1,6 GHz | 32 GB | 300 GB |
| 300.000 bis 600.000 | 1,6 GHz | 32 GB | 450 GB |
| Mehr als 600.000 | 1,6 GHz | 32 GB | 500 GB |

Im Folgenden sind die Mindestanforderungen für Computer mit AD FS oder Webanwendungsservern aufgeführt:

- CPU: Doppelkern mit 1,6 GHz oder mehr
- Arbeitsspeicher: 2 GB oder mehr
- Azure-VM: A2-Konfiguration oder höher


## Nächste Schritte
Erfahren Sie mehr über [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).



<properties
   pageTitle="Wie werden Anwendungen zu Azure Active Directory hinzugefügt?"
   description="Dieser Artikel beschreibt, wie Anwendungen zu einer Instanz von Azure Active Directory hinzugefügt werden."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="07/30/2015"
      ms.author="shoatman"/>

# Wie und warum werden Anwendungen zu Azure AD hinzugefügt?

Wenn Sie in Ihrer Instanz von Azure Active Directory eine Anwendungsliste anzeigen, fragen Sie sich vielleicht zunächst, woher die Anwendungen stammen und warum sie dort angezeigt werden.  Dieser Artikel beschreibt im Überblick, wie Anwendungen im Verzeichnis dargestellt werden, und vermittelt Kontext, anhand dem Sie besser verstehen, wie die Anwendungen in Ihr Verzeichnis kamen.

## Welche Services bietet Azure AD Anwendungen?

Anwendungen werden zu Azure AD hinzugefügt, damit sie die von Azure AD bereitgestellten Dienste nutzen können.  Diese Dienste umfassen:

* Authentifizierung und Autorisierung von Anwendungen
* Authentifizierung und Autorisierung von Benutzern
* Einmaliges Anmelden (SSO) durch Verbund oder Kennwort
* Benutzerbereitstellung und Synchronisierung
* Rollenbasierte Zugriffssteuerung (mithilfe des Verzeichnisses definieren Sie Anwendungsrollen für rollenbasierte Zugriffsprüfungen in einer Anwendung)
* oAuth-Autorisierungsdienste (von Office 365 und anderen Microsoft-Anwendungen zum Autorisieren des Zugriffs auf APIs und Ressourcen verwendet)
* Anwendungsveröffentlichung und Proxy (Veröffentlichung einer Anwendung aus einem privaten Netzwerk im Internet)

## Wie werden die Anwendungen im Verzeichnis dargestellt?

Anwendungen werden in Azure AD durch zwei Objekttypen dargestellt: das Anwendungsobjekt und das Dienstprinzipalobjekt.  Eine Anwendung wird in jedem Verzeichnis, in dem sie agiert, durch ein Anwendungsobjekt, das in einem „home“-, „owner“- oder „publishing“-Verzeichnis registriert ist, und durch ein oder mehrere Dienstprinzipalobjekte dargestellt.  

Das Anwendungsobjekt beschreibt die Anwendung in Azure AD (dem mehrinstanzenfähigen Dienst) eventuell eine der folgenden: (*Hinweis*: Dies ist keine vollständige Liste.)

* Name, Logo und Herausgeber
* Geheime Schlüssel (symmetrische und/oder asymmetrische Schlüssel für die Authentifizierung der Anwendung)
* API-Abhängigkeiten (oAuth)
* APIs/Ressourcen/veröffentlichte Bereiche (oAuth)
* App-Rollen (RBAC)
* Metadaten und Konfiguration für die einmalige Anmeldung (SSO)
* Metadaten und Konfiguration für die Benutzerbereitstellung
* Metadaten und Konfiguration für den Proxy

Der Dienstprinzipal ist ein Anwendungsdatensatz in jedem Verzeichnis, in dem die Anwendung agiert, auch im Basisverzeichnis „home“.  Der Dienstprinzipal hat folgende Aufgaben:

* Er verweist mit der Eigenschaft „App-ID“ auf ein Anwendungsobjekt.
* Er zeichnet Anwendungsrollenzuweisungen der lokalen Benutzer und Gruppen auf.
* Er zeichnet die der Anwendung erteilten Berechtigungen für lokale Benutzer und Administratoren auf.
    * Beispiel: die Berechtigung für die Anwendung, auf die E-Mail eines bestimmten Benutzers zuzugreifen
* Er zeichnet lokale Richtlinien einschließlich der geräteabhängigen Zugriffsrichtlinie auf.
* Er zeichnet alternative lokale Einstellungen einer Anwendung auf:
    * Transformationsregeln für Ansprüche
    * Attributzuordnungen (Benutzerbereitstellung)
    * Mandantenspezifische Anwendungsrollen (wenn die Anwendung benutzerdefinierte Rollen unterstützt)
    * Name/Logo

### Diagramm der Anwendungsobjekte und Dienstprinzipale in Verzeichnissen

![Ein Diagramm zur Veranschaulichung der Anwendungsobjekte und Dienstprinzipale in Azure AD-Instanzen][apps_service_principals_directory]

Wie Sie dem vorangegangenen Diagramm entnehmen können,  stellt Microsoft intern (auf der linken Seite) zwei Verzeichnisse für die Veröffentlichung von Anwendungen bereit.

* Eines für Microsoft-Anwendungen (Verzeichnis für Microsoft-Dienste)
* Eines für bereits integrierte Anwendungen von Drittanbietern (Verzeichnis für Anwendungskatalog)

Herausgeber bzw. Anbieter von Anwendungen, die ihre Anwendungen in Azure AD integrieren, benötigen ein Veröffentlichungsverzeichnis  (ein beliebiges SAAS-Verzeichnis).

Sie selbst können zum Beispiel folgende Anwendungen hinzufügen:

* Selbst entwickelte Anwendungen (in AAD integriert)
* Anwendungen, mit denen Sie über SSO verbunden sind
* Anwendungen, die Sie mithilfe des Azure AD-Anwendungsproxys veröffentlicht haben

### Hinweise und Ausnahmen

* Nicht alle Dienstprinzipale verweisen zurück auf Anwendungsobjekte.  Wie das? In den Anfängen von Azure AD wurden den Anwendungen wesentlich eingeschränktere Dienste bereitgestellt und der Dienstprinzipal reichte zur Einrichtung einer Anwendungsidentität völlig aus.  Der ursprüngliche Dienstprinzipal sah dem Active Directory-Dienstkonto von Windows Server noch wesentlich ähnlicher.  Aus diesem Grund ist die Erstellung von Dienstprinzipalen mit Azure AD PowerShell nach wie vor ohne vorherige Erstellung eines Anwendungsobjekts möglich.  Die Graph-API setzt hingegen für die Erstellung eines Dienstprinzipals ein Anwendungsobjekt voraus.
* Nicht alle oben beschriebenen Informationen werden derzeit programmgesteuert bereitgestellt.  Die folgenden Informationen sind nur in der Benutzeroberfläche verfügbar:
    * Transformationsregeln für Ansprüche
    * Attributzuordnungen (Benutzerbereitstellung)
* Weitere Informationen zu Dienstprinzipalen und Anwendungsobjekten finden Sie in der Referenzdokumentation zur Azure AD Graph-REST-API.  *Hinweis*: die Azure AD Graph-API-Dokumentation ist am ehesten an eine Schemareferenz für Azure AD, die derzeit verfügbar ist.  
    * [Anwendung](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Dienstprinzipal](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## Wie werden Anwendungen meiner Azure AD-Instanz hinzugefügt?
Es gibt viele Möglichkeiten, eine Anwendung in Azure AD hinzuzufügen:

* Hinzufügen einer Anwendung aus der [Anwendungskatalog von Azure Active Directory](http://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Anmelden bei a 3rd Party-App in Azure Active Directory integriert (z. B.: [Smartsheet](https://app.smartsheet.com/b/home) oder [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Bei der Anmeldung werden die Benutzer aufgefordert, der Anwendung neben weiteren Berechtigungen auch die Berechtigung für den Zugriff auf deren Profil zu erteilen.  Der erste Benutzer, der hierzu seine Zustimmung erteilt, bewirkt, dass dem Verzeichnis ein Dienstprinzipal für die Anwendung hinzugefügt wird.
* Anmeldung bei Microsoft online Services wie [Office 365](http://products.office.com/)
    * Wenn Sie Office 365 abonnieren oder ein Testabonnement starten, werden im Verzeichnis ein oder mehrere Dienstprinzipale erstellt, die die verschiedenen Dienste darstellen, mit denen die Funktionen von Office 365 bereitgestellt werden.
    * Einige Office 365-Dienste wie SharePoint erstellen fortlaufend neue Dienstprinzipale, um eine sichere Kommunikation zwischen den Komponenten, einschließlich Workflows, sicherzustellen.
* Hinzufügen von einer Anwendung, die Sie entwickeln in der Azure-Verwaltungsportal finden Sie unter: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Hinzufügen einer in Visual Studio entwickelten Anwendung; siehe:
    * [Authentifizierungsmethoden für ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Verbundene Dienste](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Hinzufügen einer Anwendung zur Verwendung von der [Azure AD-Anwendungsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Verbinden einer Anwendung für die einmalige Anmeldung (Single-Sign-On) über SAML oder Kennwort-SSO
* Verschiedene weitere Methoden wie entwicklerspezifische Methoden in Azure oder API-Explorer-Methoden in einem Developer Center

## Wer hat die Berechtigung zum Hinzufügen von Anwendungen zu meiner Azure AD-Instanz?

Nur globale Administratoren können Folgendes machen:

* Hinzufügen von Anwendungen aus dem Azure AD-Anwendungskatalog (bereits integrierte Anwendungen von Drittanbietern)
* Veröffentlichen einer Anwendung mithilfe des Azure AD-Anwendungsproxy

Alle Benutzer in Ihrem Verzeichnis haben die Berechtigung zum Hinzufügen von Anwendungen, die sie selbst entwickelt haben. Außerdem entscheiden sie selbst darüber, welche Anwendungen sie freigeben und welchen sie Zugriff auf ihre Unternehmensdaten erteilen.  *Denken Sie daran, dass bei der Anmeldung eines Benutzers bei einer Anwendung und der Erteilung von Berechtigungen ein Dienstprinzipal erstellt werden kann.*

Diese mag zunächst bedenklich klingen, Sie sollten dabei aber auch Folgendes berücksichtigen:

* Seit vielen Jahren konnten Anwendungen Windows Server Active Directory für die Benutzerauthentifizierung nutzen, ohne dass die Anwendung im Verzeichnis registriert sein musste oder auf andere Weise im Verzeichnis aufgezeichnet wurde.  Mit der Neuerung bietet sich Unternehmen mehr Transparenz über die Anwendungen, die ihr Verzeichnis nutzen und wofür sie es verwenden.
* Es besteht keine Notwendigkeit für einen vom Administrator gesteuerten Veröffentlichungs- bzw. Registrierungprozess für Anwendungen.  Mit Active Directory Federation Services passierte es häufig, dass ein Administrator eine Anwendung im Namen des Entwicklers als vertrauende Seite hinzufügen musste.  Dies können Entwickler nun im eigenen Namen erledigen.
* Die Benutzeranmeldung bei Anwendungen über das Unternehmenskonto ist im Rahmen ihrer Geschäftsabläufe ein großer Vorteil.  Sobald ein Benutzer aber das Unternehmen verlässt, verliert er in der bislang verwendeten Anwendung Zugriff auf sein Konto.
* Auch eine Aufzeichnung darüber, welche Daten für welche Anwendungen freigegeben wurden, ist von Vorteil.  Daten lassen sich heute besser austauschen und übertragen als jemals zuvor, und eine klare Aufzeichnung darüber, wer welche Daten für welche Anwendungen freigegeben hat, ist äußerst wichtig.
* Anwendungen, die Azure AD für oAuth nutzen, legen genau fest, welche Berechtigungen die Benutzer Anwendungen erteilen können und bei welchen Berechtigungen die Zustimmung eines Administrators erforderlich ist.  Es versteht sich von selbst, dass nur Administratoren größeren Umfängen und wichtigen Berechtigungen zustimmen können.
* Bei Benutzern, die Anwendungen hinzufügen und ihnen Zugriff auf ihre Daten erlauben, handelt es sich um überwachte Ereignisse. Sie können daher in den Überwachungsberichten des Azure-Verwaltungsportals überprüfen, wie eine Anwendung zum Verzeichnis hinzugefügt wurde.

**Hinweis:** *Microsoft selbst mit der Standardkonfiguration Monaten ausgeführt wurde.*

Bei all dem muss auch gesagt werden, dass es nach wie vor möglich ist, zu verhindern, dass Benutzer Anwendungen zu Ihrem Verzeichnis hinzufügen und im eigenen Ermessen entscheiden, welche Informationen sie den Anwendungen freigeben. Sie brauchen dazu nur die Verzeichniskonfiguration im Azure-Verwaltungsportal zu ändern.  Auf die nachfolgend abgebildete Konfiguration kann im Azure-Verwaltungsportal auf der Registerkarte „Konfigurieren“ Ihres Verzeichnisses zugegriffen werden.

![Ein Screenshot der Benutzeroberfläche für die Konfiguration der Einstellungen integrierter Anwendungen][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

Holen Sie weitere Informationen zum Hinzufügen von Anwendungen in Azure AD und zum Konfigurieren von Diensten für Anwendungen ein.

* Entwickler: [erfahren Sie, wie Sie eine Anwendung in AAD integrieren](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Entwickler: [Überprüfen Sie Beispielcode für apps in Azure Active Directory auf Github integriert](https://github.com/AzureADSamples)
* Entwickler und IT-Profis: [Überprüfen Sie die REST-API-Dokumentation für die Azure Active Directory Graph-API](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-Experten: [enthält Informationen zum Verwenden von Azure Active Directory integrierter Anwendungen von App-Katalog](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-Experten: [Suchen von Lernprogrammen zur Konfiguration bestimmter bereits integrierter Anwendungen](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-Experten: [enthält Informationen zum Veröffentlichen einer app mit Azure Active Directory-Anwendungsproxy](https://msdn.microsoft.com/library/azure/dn768219.aspx)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg



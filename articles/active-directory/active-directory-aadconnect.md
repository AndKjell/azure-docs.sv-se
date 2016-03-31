<properties
    pageTitle="Azure AD Connect: Integrieren Ihrer lokalen Identitäten in Azure Active Directory. | Microsoft Azure"
    description="Nachfolgend finden Sie einen Überblick über Azure AD Connect sowie eine Beschreibung des Einsatzes dieser Technologie."
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
    ms.topic="get-started-article"
    ms.date="12/02/2015"
    ms.author="andkjell;billmath"/>

# Integrieren Ihrer lokalen Identitäten in Azure Active Directory
Mit Azure AD Connect können Sie Ihr lokales Identitätssystem wie Windows Server Active Directory in Azure Active Directory integrieren und eine Verbindung für Ihre Benutzer mit Office 365, Azure und tausenden Saas-Anwendungen herstellen. Dieses Thema bietet eine umfassende Anleitung für das Vorbereiten und Bereitstellen der erforderlichen Komponenten für Ihre Endbenutzer, um mit der gleichen Identität auf die Clouddienste zuzugreifen, mit der sie bereits auf vorhandene Unternehmens-Apps zugreifen.

![Was ist Azure AD Connect?](./media/active-directory-aadconnect/arch.png)

## Gründe für die Verwendung von Azure AD Connect
Die Integration Ihrer lokalen Verzeichnisse in Azure AD steigert die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird.  Mit dieser Integration können Benutzer und Organisationen die folgenden Vorteile nutzen:

- Benutzer können eine einzelne Identität verwenden, um auf lokale Anwendungen und Clouddienste wie z. B. Office 365 zuzugreifen.

- Einzelnes Tool zum Bereitstellen einer einfachen Bereitstellungserfahrung für die Synchronisierung und Anmeldung.

- Bietet die neuesten Funktionen für Ihre Szenarien. Azure AD Connect ersetzt ältere Versionen von Identitätsintegrationstools, wie z. B. DirSync und Azure AD Sync. Weitere Informationen finden Sie unter [Verzeichnisintegration tools Vergleich](active-directory-aadconnect-get-started-tools-comparison.md).


### Funktionsweise von Azure AD Connect

Azure Active Directory Connect besteht aus drei Hauptbestandteilen.  Sie sind die Synchronisierungsdienste, die optionale Active Directory Federation Services Information und die Überwachung durchgeführt, wobei [Azure AD Connect Health](active-directory-aadconnect-health.md).

<center>![Azure AD Connect-Stapel](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

- Synchronisierung – dieser Teil besteht aus Komponenten und Funktionen, die zuvor als veröffentlicht wurden [Dirsync und Azure AD Sync](active-directory-aadconnect-get-started-tools-comparison.md).  Dieser Teil ist für das Erstellen von Benutzern und Gruppen zuständig.  Er stellt ebenfalls sicher, dass Benutzer- und Gruppeninformationen in Ihrer lokalen Umgebung denen in der Cloud entsprechen.
- AD FS: Dies ist eine optionale Komponente von Azure AD Connect und kann zum Einrichten einer Hybridumgebung mithilfe einer lokalen AD FS-Infrastruktur verwendet werden.  Dieser Teil kann von Organisationen verwendet werden, um sich mit komplexen Bereitstellungen zu befassen, z. B. Domänenbeitritts-SSO, Erzwingen von AD-Anmelderichtlinien und Smartcard- bzw. Drittanbieter-MFA.
- Systemüberwachung: Azure AD Connect Health bietet eine stabile Überwachung Ihrer AD FS-Server und einen zentralen Speicherort im Azure-Portal, um diese Aktivität anzuzeigen.  Weitere Informationen finden Sie unter [Azure Active Directory Connect Health](active-directory-aadconnect-health.md).

## Installieren von Azure AD Connect

Sie finden den Download für Azure AD Connect auf [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).


| Lösung | Szenario |
| ----- | ----- |
| Vorbereitung | <li>[Azure AD Connect: Hardware und Voraussetzungen](active-directory-aadconnect-prerequisites.md)</li> |
| [Express-Einstellungen](active-directory-aadconnect-get-started-express.md) | <li>Empfohlen und die Standardoption, wenn Sie eine Active Directory-Gesamtstruktur verfügen.</li> <li>Benutzer melden Sie sich mit der Synchronisierung von Kennwörtern verwenden dasselbe Kennwort.</li>
| [Benutzerdefinierte Einstellungen](active-directory-aadconnect-get-started-custom.md) | <li>Verwendet, wenn Sie mehrere Gesamtstrukturen haben. Unterstützt viele lokalen [Topologien](active-directory-aadconnect-topologies.md).</li> <li>Passen Sie die Anmeldeoption, an, wie z. B. AD FS für Verbund oder verwenden eine 3rd Identitätsanbieter party.</li> <li>Anpassen von Synchronisierungsfunktionen, wie die Filterung und Rückschreiben.</li>
| [Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) | <li>Wenn Sie einen vorhandenen DirSync-Server bereits ausgeführt haben.</li>
| Upgrade von Azure AD Sync | <li>Dies ist eine nahtlose in-Place-Aktualisierung.</li>


[Nach der Installation](active-directory-aadconnect-whats-next.md) sollten Sie überprüfen, wird es wie erwartet funktioniert und den Benutzern Lizenzen zuweisen.

### Nächste Schritte für die Installation von Azure AD Connect

| Thema |  |
| --------- | --------- |
| Azure AD Connect herunterladen | [Azure AD Connect herunterladen](http://go.microsoft.com/fwlink/?LinkId=615771) |
| Installieren mit den Express-Einstellungen | [Expressinstallation von Azure AD Connect](active-directory-aadconnect-get-started-express.md) |
| Installieren mit benutzerdefinierten Einstellungen | [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md) |
| Upgrade von DirSync | [Upgrade von Azure AD-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md) |
| Nach der Installation | [Überprüfen der Installation und Zuweisen von Lizenzen ](active-directory-aadconnect-whats-next.md) |

### Weitere Informationen über die Installation von Azure AD Connect

Möchten Sie bereiten [betriebliche](active-directory-aadconnectsync-operations.md) Bedenken. Möglicherweise möchten Sie ein Standby-Servers, damit Sie problemlos im Fall von über fallen können eine [nach einem Notfall](active-directory-aadconnectsync-operations.md#disaster-recovery). Wenn Sie planen, häufige Änderungen müssen Sie planen ein [stagingmodus](active-directory-aadconnectsync-operations.md#staging-mode) Server.

| Thema |  |
| --------- | --------- |
| Unterstützte Topologien | [Topologien für Azure AD Connect](active-directory-aadconnect-topologies.md) |
| Entwurfskonzepte | [Entwurfskonzepte für Azure AD Connect](active-directory-aadconnect-design-concepts.md) |
| Für die Installation verwendete Konten | [Weitere Informationen zu den Anmeldeinformationen und Berechtigungen von Azure AD Connect](active-directory-aadconnect-accounts-permissions.md) |
| Operative Planung | [Azure AD Connect Sync: Operative Aufgaben und Überlegungen](active-directory-aadconnectsync-operations.md) |
| Optionen für die Benutzeranmeldung | [Azure AD Connect-Optionen für die Benutzeranmeldung](active-directory-aadconnect-user-signin.md) |

## Konfigurieren von Funktionen
Azure AD Connect verfügt über mehrere Funktionen, die Sie optional aktivieren können oder die standardmäßig aktiviert sind. Einige Funktionen benötigen in einigen Fällen möglicherweise eine zusätzliche Konfiguration in bestimmten Szenarios und Topologien.

[Filtern von](active-directory-aadconnectsync-configure-filtering.md) wird verwendet, um einzuschränken, welche Objekte in Azure AD synchronisiert werden. Standardmäßig werden alle Benutzer, Kontakte, Gruppen und Windows 10-Computer synchronisiert, aber Sie können dies auf Grundlage von Domänen, Organisationseinheiten oder Attributen einschränken.

[Synchronisierung von Kennwörtern](active-directory-aadconnectsync-implement-password-synchronization.md) den Hashwert des Kennworts in Active Directory in Azure AD synchronisiert werden. Dies ermöglicht es dem Benutzer, das gleiche Kennwort lokal und in der Cloud zu verwenden, es jedoch nur an einem Ort zu verwalten. Da es das lokale Active Directory verwendet, können Sie auch Ihre eigene Kennwortrichtlinie verwenden.

[Das Rückschreiben von Kennwörtern](active-directory-passwords-getting-started.md) können Benutzer, zu ändern und Zurücksetzen ihrer Kennwörter in der Cloud und die lokale Kennwortrichtlinie.

[Geräterückschreiben](active-directory-aadconnect-get-started-custom-device-writeback.md) lässt ein Gerät registriert in Azure AD in einer lokalen Active Directory zurückgeschrieben werden, damit sie für bedingten Zugriff verwendet werden kann.

Die [zu verhindern, dass versehentlich löscht](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) Feature ist standardmäßig aktiviert und Ihr Cloud-Verzeichnis von vielen löscht gleichzeitig geschützt. Standardmäßig sind 500 Löschvorgänge pro Durchlauf zulässig; dies kann abhängig von der Größe Ihres Unternehmens geändert werden.

### Nächste Schritte zum Konfigurieren der Funktionen

| Thema |  |
| --------- | --------- |
| Konfigurieren der Filterung | [Azure AD Connect-Synchronisierung: Konfigurieren der Filterung](active-directory-aadconnectsync-configure-filtering.md) |
| Kennwortsynchronisierung | [Azure AD Connect-Synchronisierung: Implementieren der Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) |
| Rückschreiben von Kennwörtern | [Erste Schritte mit der Kennwortverwaltung](active-directory-passwords-getting-started.md) |
| Geräterückschreiben | [Aktivieren des Geräterückschreibens in Azure AD Connect](active-directory-aadconnect-get-started-custom-device-writeback.md) |
| Verhindern von versehentlichen Löschungen | [Azure AD Connect-Synchronisierung: Verhindern von versehentlichen Löschvorgängen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) |

## Anpassen der Azure AD Connect-Synchronisierung
Für die Azure AD Connect-Synchronisierung ist eine Standardkonfiguration vorgegeben, die für die meisten Kunden und Topologien konzipiert wurde. Es gibt jedoch immer Situationen, in denen die Standardkonfiguration nicht funktioniert und angepasst werden muss. Sie können Änderungen wie in diesem Abschnitt und in den verknüpften Themen beschrieben vornehmen.

Wenn Sie nicht mit einer Synchronisierungstopologie gearbeitet haben, bevor die Grundlagen und wie in beschrieben verwendeten Begriffe verstehen beginnen soll die [technische Konzepte](active-directory-aadconnectsync-technical-concepts.md). Azure AD Connect ist die Weiterentwicklung von MIIS2003, ILM2007 und FIM2010. Selbst wenn einige Dinge identisch sind, hat sich doch auch eine Menge verändert.

Die [Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md) geht davon aus, in der Konfiguration kann mehr als einer Gesamtstruktur vorhanden sein. In diesen Topologien kann ein Benutzerobjekt möglicherweise in einer anderen Gesamtstruktur als Kontakt dargestellt werden. Der Benutzer kann in einer anderen Ressourcengesamtstruktur auch über ein verknüpftes Postfach verfügen. Das Verhalten der Standardkonfiguration wird in beschrieben [Benutzer und Kontakte](active-directory-aadconnectsync-understanding-users-and-contacts.md).

Das Konfigurationsmodell synchron aufgerufen [deklarativen Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). Verwenden der erweiterten attributflüsse [Funktionen](active-directory-aadconnectsync-functions-reference.md) Attribut Transformationen Ausdrücken. Sie können die gesamte Konfiguration mithilfe von Tools, die im Lieferumfang von Azure AD Connect enthalten sind, anzeigen und überprüfen. Wenn Sie die Konfiguration ändern möchten, sicherstellen führen Sie die [bewährte Methoden](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) es wird einfacher sein, neue Versionen von übernehmen, wie diese zur Verfügung gestellt werden.

### Nächste Schritte zur Anpassung der Azure AD Connect-Synchronisierung

| Thema |  |
| --------- | --------- |
| Technische Konzepte  | [Azure AD Connect-Synchronisierung: Technische Konzepte](active-directory-aadconnectsync-technical-concepts.md) |
| Grundlegendes zur Standardkonfiguration | [Azure AD Connect-Synchronisierung: Grundlegendes zur Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md) |
| Grundlegendes zu Benutzern und Kontakten | [Azure AD Connect-Synchronisierung: Grundlegendes zu Benutzern und Kontakten](active-directory-aadconnectsync-understanding-users-and-contacts.md) |
| Deklarative Bereitstellung | [Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Funktionsreferenz für deklarative Bereitstellung | [Azure AD Connect-Synchronisierung: Funktionsreferenz](active-directory-aadconnectsync-functions-reference.md) |
| Bewährte Methoden | [Bewährte Methoden zum Ändern der Standardkonfiguration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) |

## Weitere Informationen und Referenzen

| Thema |  |
| --------- | --------- |
| Versionsverlauf | [Versionsverlauf](active-directory-aadconnect-version-history.md) |
| Vergleich von DirSync, Azure ADSync und Azure AD Connect | [Vergleich von Tools für die Verzeichnisintegration](active-directory-aadconnect-get-started-tools-comparison.md) |
| Synchronisierte Attribute | [Synchronisierte Attribute](active-directory-aadconnectsync-attributes-synchronized.md) |
| Überwachen mit Azure AD Connect Health | [Azure AD Connect Health](active-directory-aadconnect-health.md) |
| Häufig gestellte Fragen | [Häufig gestellte Fragen zu Azure AD Connect](active-directory-aadconnect-faq.md) |


**Zusätzliche Ressourcen**


Ignite 2015-Präsentation über die Erweiterung lokaler Verzeichnisse in die Cloud.

[AZURE.VIDEO microsoft-ignite-2015-extending-on-premises-directories-to-the-cloud-made-easy-with-azure-active-directory-connect]



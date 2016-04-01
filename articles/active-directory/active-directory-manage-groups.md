<properties
    pageTitle="Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen | Microsoft Azure"
    description="Verwendung von Gruppen in Azure Active Directory für die zugriffsverwaltung auf lokale und Cloudanwendungen und Ressourcen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/17/2015"
    ms.author="curtand"/>


# Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen

Azure Active Directory (Azure AD) ist eine umfassende Lösung für die Identitäts- und Zugriffsverwaltung, die einen robusten Satz von Funktionen zur Verwaltung des Zugriffs auf lokale und Cloudanwendungen und -ressourcen bereitstellt, einschließlich Microsoft-Onlinediensten wie Office 365 und zahlreicher Microsoft-SaaS-Anwendungen.


> [AZURE.NOTE] Um Azure Active Directory verwenden zu können, benötigen Sie ein Azure-Konto. Wenn Sie kein Konto haben, können Sie [Melden Sie sich für ein kostenloses Azure-Konto](http://azure.microsoft.com/pricing/free-trial/).


Eines der wichtigsten Features innerhalb von Azure Active Directory ist die Möglichkeit, den Zugriff auf Ressourcen zu verwalten. Diese Ressourcen können Teil des Verzeichnisses sein, wie im Fall von Berechtigungen zum Verwalten von Objekten mithilfe von Rollen im Verzeichnis, oder externe Ressourcen, z. B. SaaS-Anwendungen, Azure-Dienste und SharePoint-Websites oder lokale Ressourcen. Es gibt vier Möglichkeiten, einem Benutzer Zugriffsrechte auf eine Ressource zuzuweisen:


1\. Direkte Zuweisung

Benutzer können einer Ressource direkt vom Besitzer dieser Ressource zugewiesen werden.

2\. Gruppenmitgliedschaft

Eine Gruppe kann einer Ressource durch den Besitzer der Ressource zugewiesen werden, wodurch den Mitgliedern dieser Gruppe Zugriff auf die Ressource gewährt wird. Die Mitgliedschaft der Gruppe kann dann vom Besitzer der Gruppe verwaltet werden. Der Besitzer der Ressource delegiert die Berechtigung, dieser Ressource Benutzer zuzuweisen, an den Besitzer der Gruppe.

3\. Regelbasiert

Der Besitzer der Ressource kann eine Regel verwenden, um auszudrücken, welchen Benutzern Zugriff auf eine Ressource zugewiesen werden soll. Das Ergebnis der Regel hängt von den in der Regel verwendeten Attributen und deren Werten für bestimmte Benutzer ab. So delegiert der Ressourcenbesitzer das Recht zur Verwaltung des Zugriffs auf die Ressource an die autoritative Quelle für die Attribute, die in der Regel verwendet werden. Beachten Sie, dass der Ressourcenbesitzer weiterhin die Regel selbst verwaltet und festlegt, welche Attribute und Werte Zugriff auf die Ressource bereitstellen.

4\. Externe Autorität

Der Zugriff auf eine Ressource stammt aus einer externen Quelle, z. B. einer Gruppe, die mittels einer autoritativen Quelle synchronisiert wird, z. B. eines lokalen Verzeichnisses oder einer SaaS-App, wie etwa WorkDay. Der Besitzer der Ressource weist der Gruppe Zugriff auf die Ressource zu, und die externe Datenquelle verwaltet die Mitglieder der Gruppe.

  ![Übersicht über das Access Management-Diagramm](./media/active-directory-access-management-groups/access-management-overview.png)


## Dieses Video bietet Ihnen eine Erläuterung der Zugriffsverwaltung.

Sie können ein kurzes Video anschauen, das nähere Informationen zum Thema bereitstellt.

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## Wie funktioniert die Zugriffsverwaltung in Azure Active Directory?
Im Mittelpunkt der Lösung zur Zugriffsverwaltung von Azure Active Directory steht die Sicherheitsgruppe. Die Verwendung einer Sicherheitsgruppe zum Verwalten des Zugriffs auf Ressourcen ist ein bekanntes Paradigma, mit dem auf flexible und leicht verständliche Weise einer dafür vorgesehenen Gruppe von Benutzern Zugriff auf eine Ressource bereitgestellt werden kann. Der Besitzer der Ressource (oder der Administrator des Verzeichnisses) kann eine Gruppe zuweisen, um bestimmte Zugriffsrechte für seine Ressourcen bereitzustellen. Den Mitgliedern der Gruppe wird Zugriff erteilt, und der Besitzer der Ressource kann das Recht zur Verwaltung der Mitgliederliste einer Gruppe an Dritte delegieren – z. B. einen Abteilungsleiter oder Helpdesk-Administrator.

![Das Azure Active Directory Access Management-Diagramm](./media/active-directory-access-management-groups/active-directory-access-management-works.png)
Der Besitzer einer Gruppe kann auch dieser Gruppe für Self-Service-Anfragen zur Verfügung stellen. In diesem Fall kann ein Endbenutzer die Gruppe suchen und finden und eine Beitrittsanfrage stellen. Damit strebt er die Berechtigung zum Zugriff auf die Ressourcen an, die durch die Gruppe verwaltet werden. Der Besitzer der Gruppe kann die Gruppe so einrichten, dass Beitrittsanfragen automatisch genehmigt werden, oder die Genehmigung durch den Besitzer der Gruppe fordern. Wenn ein Benutzer eine Gruppenbeitrittsanfrage stellt, wird die Anfrage an die Besitzer der Gruppe weitergeleitet. Wenn einer der Besitzer die Anfrage genehmigt, wird der anfragende Benutzer benachrichtigt und der Gruppe hinzugefügt. Wenn einer der Besitzer die Anfrage ablehnt, wird der anfragende Benutzer benachrichtigt, jedoch nicht der Gruppe hinzugefügt.


## Erste Schritte mit der Zugriffsverwaltung
Wollen Sie loslegen? Sie sollten einige der grundlegenden Aufgaben testen, die Sie mit Azure AD-Gruppen ausführen können. Verwenden Sie diese Funktionen, um verschiedenen Gruppen von Benutzern spezialisierten Zugriff auf verschiedene Ressourcen in Ihrer Organisation bereitzustellen. Eine Liste der ersten grundlegenden Schritte ist unten aufgeführt.


* [Erstellen einer einfachen Regel zum Konfigurieren von dynamischen Mitgliedschaften für eine Gruppe](active-directory-accessmanagement-simplerulegroup.md)

* [Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen](active-directory-accessmanagement-group-saasapps.md)

* [Einrichten einer Gruppe für Self-Service durch Endbenutzer](active-directory-accessmanagement-self-service-group-management.md)

* [Synchronisieren einer lokalen Gruppe in Azure mithilfe von Azure AD Connect](active-directory-aadconnect.md)

* [Verwalten von Besitzern einer Gruppe](active-directory-accessmanagement-managing-group-owners.md)


## Nächste Schritte für die Zugriffsverwaltung
Da Sie nun die Grundlagen der Zugriffsverwaltung kennen, können Sie sich jetzt mit einigen erweiterten Funktionen vertraut machen, die in Azure Active Directory für die Verwaltung des Zugriffs auf Anwendungen und Ressourcen zur Verfügung stehen.

* [Verwenden einer einfachen Regel zum Erstellen einer Gruppe](active-directory-accessmanagement-simplerulegroup.md)

* [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Verwalten von Sicherheitsgruppen in Azure Active Directory](active-directory-accessmanagement-manage-groups.md)

* [Einrichten dedizierter Gruppen in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md)


## Weitere Informationen
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)

* [Graph-API-Referenz für Gruppen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)



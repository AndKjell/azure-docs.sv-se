<properties
    pageTitle="Azure AD und Anwendungen: Leitfaden für Entwickler | Microsoft Azure"
    description="Dieser Artikel wendet sich an IT-Fachpersonal und erläutert Richtlinien zur Integration von Azure-Anwendungen in Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/09/2015"
    ms.author="inhenk"/>

# Azure AD und Anwendungen: Leitfaden für Entwickler

## Übersicht

Dieses Handbuch bietet eine Übersicht über die Entwicklung von Branchenanwendungen (Line of Business, LoB) für Azure Active Directory und richtet sich speziell an den Bedarf globaler Administratoren von Active Directory/Office 365.

Das Erstellen von Anwendungen mit Azure AD bedeutet für die Benutzer der betreffenden Organisation das einmalige Anmelden mit Office 365. Die Platzierung der Anwendung in Azure AD bietet Ihnen Kontrolle über die für die Anwendung festgelegte Authentifizierungsrichtlinie. Weitere Informationen zu bedingten Zugriff und apps mit Multi-Factor Authentication (MFA) schützen finden Sie im folgende Dokument: [Konfigurieren von Zugriffsregeln](active-directory-conditional-access-azuread-connected-apps.md)

Die Anwendung muss registriert sein, um Azure Active Directory zu verwenden. Durch Registrieren der Anwendung können Entwickler in Ihrer Organisation deren Mitglieder mithilfe von Azure AD authentifizieren und Zugriff auf die Benutzerressourcen wie E-Mail, Kalender, Dokumente usw. anfordern.

Jedes Mitglied Ihres Verzeichnisses (nicht Gäste) kann eine Anwendung, auch bekannt als registrieren *Erstellung eines Anwendungsobjekts*.

Durch das Registrieren einer Anwendung können alle Benutzer folgende Aktionen ausführen:

- Eine Identität für ihre Anwendung erhalten, die von Azure AD erkannt wird
- Ein oder mehrere Kennwörter/Schlüssel erhalten, mit denen die Anwendung sich gegenüber AD authentifizieren kann
- Die Anwendung im Azure-Portal mit einem Namen, Logo usw. kennzeichnen
- Die Azure AD-Autorisierungsfunktionen für ihre Anwendung nutzen
  - Rollenbasierte Zugriffssteuerung (Role based Access Control, RBAC) von Anwendungen
  - Azure Active Directory als oAuth-Autorisierungsserver (Absichern einer von der Anwendung offengelegten API)

- Legen Sie die erforderlichen Berechtigungen für das ordnungsgemäße Funktionieren der Anwendung fest. Hierzu gehören folgende Berechtigungen:
      -App Berechtigungen (nur für globale Administratoren). Beispiel:
        -Die Rollenmitgliedschaft in einem anderen Azure AD-Anwendung oder Rollenmitgliedschaft relativ zu einer Azure-Ressource, Ressourcengruppe oder ein Abonnement
      -Delegierte Berechtigungen verfügen (alle Benutzer). Beispiel:
        -(AAD) anmelden und Read-Profil
        -(Exchange) lesen, E-Mail, senden
        – Lesen (SharePoint)

> [AZURE.NOTE]Standardmäßig kann alle Mitglieder eine Anwendung registrieren. Informationen zum Einschränken von Berechtigungen für die Registrierung der Anwendung auf bestimmte Mitglieder finden Sie im Dokument "Hinzufügen von Anwendungen in Azure AD".

Folgende Schritte sind für globale Administratoren erforderlich, um Entwickler dabei zu unterstützen, ihre Anwendungen zur Produktionsreife zu bringen:

- Konfigurieren von Zugriffsregeln (Access Policy/MFA)
- Konfigurieren der Anwendung für das Erfordern von Benutzerrechten und Zuweisen von Benutzern
- Standardmäßiges Unterdrücken der Oberfläche für die Benutzerzustimmung

## Konfigurieren von Zugriffsregeln

Wie bereits erwähnt finden Sie im folgenden Artikel weitere Informationen zum Konfigurieren von Zugriffsregeln für jede Anwendung.

[Konfigurieren von Zugriffsregeln](active-directory-conditional-access-azuread-connected-apps.md).

## Konfigurieren der Anwendung für das Erfordern von Benutzerrechten und Zuweisen von Benutzern

Standardmäßig ist eine Benutzerzuweisung keine Vorbedingung für einen Zugriff des Benutzers auf die Anwendung. Falls die Anwendung jedoch Rollen offenlegt oder gewünscht wird, dass die Anwendung im Zugriffspanel des Benutzers angezeigt wird, sollten Benutzerzuweisungen erforderlich gemacht und entsprechende Benutzer bzw. Gruppen zugewiesen werden.

[Erfordern der Benutzerzuweisung](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Abonnenten von Azure AD Premium oder Enterprise Mobility Suite (EMS) empfehlen wir ausdrücklich die Nutzung von Gruppen. Durch das Zuweisen von Gruppen zu der Anwendung kann die laufende Zugriffsverwaltung an den Besitzer der Gruppe delegiert werden. Sie können eine solche Gruppe selbst erstellen oder die zuständige Person in Ihrer Organisation bitten, die Gruppe mithilfe des vorhandenen Tools für die Gruppenverwaltung zu erstellen.

- [Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md)
- [Zuweisen von Gruppen zu einer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)

## Unterdrücken der Benutzerzustimmung

Standardmäßig muss der Benutzer seine Zustimmung zu der für die Anmeldung erforderlichen Berechtigung geben. Die Oberfläche für die Benutzerzustimmung kann Benutzern durch die Abfrage einer Zustimmung zu Berechtigungen einen negativen Eindruck vermitteln, sofern sie mit einer solchen Entscheidung nicht vertraut sind.

Für vertrauenswürdige Anwendungen ist es möglich, dass Administratoren der Anwendung im Namen aller Benutzer der Organisation zustimmen.

Weitere Informationen über die Zustimmung des Benutzers und die Zustimmung in Azure auftreten, finden Sie unter [Integration von Anwendungen mit Azure Active Directory](active-directory-integrating-applications.md)



<properties
    pageTitle="Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 8.1-Geräte | Microsoft Azure"
    description=" Schritte zum Konfigurieren einer Gruppenrichtlinie für in eine Windows 8.1-Domäne eingebundene Geräte für die automatische Registrierung bei Azure AD. "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/24/2015"
    ms.author="femila"/>

# Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 8.1-Geräte

Sie können eine Active Directory-Gruppenrichtlinie verwenden, um Ihre in eine Domäne eingebundenen Windows 8.1-Geräte für die automatische Registrierung bei Azure AD zu konfigurieren. Um die Gruppenrichtlinie zu konfigurieren, müssen Sie über mindestens einen in eine Domäne eingebundenen Windows Server 2012 R2- oder Windows 8.1-Computer verfügen, auf dem die Gruppenrichtlinienverwaltungs-Funktion installiert ist. Nachdem diese Gruppenrichtlinie für Ihre Domäne aktiviert ist, wird jeder Domänenbenutzer, der sich bei dem Computer anmeldet, automatisch und im Hintergrund mit einem Geräteobjekt in Azure AD registriert. Für jeden registrierten Benutzer des physischen Geräts gibt es ein Geräteobjekt in Azure AD. Stellen Sie sicher, dass Sie die Voraussetzungen in "Automatische Geräteregistrierung mit Azure Active Directory für in Domänen eingebundene Windows-Geräte" gelesen haben und erfüllen.

## Konfigurieren der Gruppenrichtlinie für Ihre in eine Domäne eingebundenen Windows 8.1-Geräte

1. Öffnen Sie Server-Manager, und navigieren Sie zu **Tools** > **Group Policy Management**.
2. Navigieren Sie aus der Gruppenrichtlinien-Verwaltungskonsole auf den Knoten der Domäne, die der Domäne entspricht, in dem Sie aktivieren möchten **Automatische Arbeitsbereichverknüpfung**.
3. Mit der rechten Maustaste **Group Policy Objects** und wählen Sie **Neu**. Geben Sie Ihrem Gruppenrichtlinienobjekt einen Namen, z. B. **Automatische Arbeitsbereichverknüpfung**. Klicken Sie auf **OK**.
4. Mit der rechten Maustaste auf Ihr neues Gruppenrichtlinienobjekt, und wählen Sie dann **Bearbeiten**.
5. Navigieren Sie zu **Computerkonfiguration** > **Richtlinien** > **Administrative Vorlagen** > **Windows-Komponenten** > **Arbeitsplatzbeitritt**.
6. Mit der rechten Maustaste automatisch in Clientcomputer Arbeitsbereich einbinden, und wählen Sie dann **Bearbeiten**.
7. Aktivieren Sie das Optionsfeld "Aktiviert", und klicken Sie dann auf "Übernehmen". Klicken Sie auf **OK**.
8. Sie können das Gruppenrichtlinienobjekt jetzt mit einem Speicherort Ihrer Wahl verknüpfen. Um diese Richtlinie für alle in eine Domäne eingebundenen Windows 8.1-Geräte in Ihrer Organisation zu aktivieren, verknüpfen Sie die Gruppenrichtlinie mit der Domäne.

## Aufheben der Registrierung Ihrer in eine Domäne eingebundenen Windows 8.1-Geräte

Sie können auch die Domäne eingebundenen Windows 8.1-Geräte wie folgt aufheben:
Ändern Sie die Arbeitsbereich verknüpfen von Gruppenrichtlinien-Einstellungen, die im vorherigen Abschnitt erstellt haben. Legen Sie die Richtlinie "Clientcomputer automatisch in Arbeitsbereich einbinden" auf "Deaktiviert" fest. Dadurch wird verhindert, dass neue Geräte automatisch in den Arbeitsbereich eingebunden werden.

Heben Sie die Registrierung der vorhandenen in eine Domäne eingebundenen Windows 8.1-Computer mithilfe einer der folgenden zwei Optionen auf:

* Option 1: **Gerät mithilfe von PC-Einstellungen der Domäne Unregister ein Windows 8.1**
  1. Navigieren Sie auf dem Windows 8.1-Gerät zu **PC-Einstellungen** > **Netzwerk** > **Arbeitsplatz**
  2. Wählen Sie **lassen**.
Dieser Vorgang muss für jeden Domänenbenutzer wiederholt werden, der sich bei dem Computer angemeldet hat und automatisch in den Arbeitsbereich eingebunden wurde.

* Option 2: Aufhebung der Registrierung eines in eine Domäne eingebundenen Windows 8.1-Geräts mithilfe eines Skripts
    1. Öffnen Sie ein Eingabeaufforderungsfenster auf dem Computer Windows 8.1, und führen Sie den folgenden Befehl aus:
   ` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Dieser Befehl muss im Kontext jedes Domänenbenutzers ausgeführt werden, der sich bei dem Computer angemeldet hat.
Ereignisanzeige & Fehler für in eine Domäne eingebundene Windows 8.1-Geräte

Das Windows-Ereignisprotokoll auf dem Windows 8.1-Computer zeigt Fehlermeldungen im Zusammenhang mit der Geräteregistrierung an. Sie finden dort sowohl Meldungen zu erfolgreichen als auch fehlgeschlagenen Ereignissen. Das Ereignisprotokoll kann finden Sie in der Ereignisanzeige unter Programme und Dienste **Protokolle** > **Microsoft** > **Windows > Arbeitsbereichverknüpfung**.

##Zusätzliche Details

Die Gruppenrichtlinie aktiviert einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt und bei Anmeldung eines Benutzers ausgelöst wird. Der Task registriert den Benutzer und das Gerät im Hintergrund bei Azure AD, nachdem die Anmeldung abgeschlossen ist. Den geplanten Task finden Sie unter Windows 8.1-Geräte in der Aufgabenplanungsbibliothek unter **Microsoft** > **Windows** > **Arbeitsplatzbeitritt**. Der Task wird für alle Active Directory-Benutzer ausgeführt, die sich bei dem Computer anmelden, und registriert sie.  

## Weitere Themen
- [Azure Active Directory-Geräteregistrierung – Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 7-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)




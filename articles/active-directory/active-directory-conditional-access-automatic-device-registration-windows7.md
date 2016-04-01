<properties
    pageTitle="# Konfigurieren Sie die automatische geräteregistrierung für Windows 7-Domäne beitreten Geräte| Microsoft Azure"
    description="Schritte zum Konfigurieren der Windows 7-Domäne verknüpft Geräte automatisch mit Azure AD registrieren. und Schritte zum Bereitstellen von der geräteregistrierungs-Softwarepaket für Ihre Domäne Windows 7-Geräte mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager gehören."
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

# Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 7-Geräte

Als IT-Administrator können Sie Ihre in eine Domäne eingebundenen Windows 7-Geräte für die automatische Registrierung bei Azure AD konfigurieren. Hierzu müssen Sie das Geräteregistrierungs-Softwarepaket für Ihre in eine Domäne eingebundenen Windows 7-Geräte mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereitstellen. Stellen Sie sicher, dass Sie die Voraussetzungen in "Automatische Geräteregistrierung mit Azure Active Directory für in Domänen eingebundene Windows-Geräte" gelesen haben und erfüllen.

##Installieren des Geräteregistrierung-Softwarepakets auf in eine Domäne eingebundenen Windows 7-Geräten

Geräteregistrierung für Windows 7 steht als eine [herunterladbares MSI-Paket](https://connect.microsoft.com/site1164). Das Paket muss auf Windows 7-Computern installiert werden, die in eine Active Directory-Domäne eingebunden sind. Sie sollten das Paket mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager bereitstellen. Das MSI-Paket unterstützt die standardmäßigen Hintergrundinstallationsoptionen mithilfe des Parameters "/quiet".
Das Softwarepaket steht zum Download auf der [Microsoft Connect-Website](https://connect.microsoft.com/site1164). Dort können Sie "Arbeitsbereichverknüpfung für Windows 7" auswählen und dann herunterladen.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## Arbeitsbereichverknüpfung mit Azure Active Directory
Die Geräteregistrierung für in eine Domäne eingebundene Windows 7-Geräte erfordert keine Benutzeroberfläche, weshalb auch keine enthalten ist. Nach der Installation auf dem Computer wird jeder Domänenbenutzer, der sich bei dem Computer anmeldet, automatisch und im Hintergrund mit einem Geräteobjekt bei Azure AD registriert. Für jeden registrierten Benutzer des physischen Geräts gibt es ein Geräteobjekt in Azure AD. 

Das Installationsprogramm erstellt einen geplanten Task auf dem System, der im Kontext des Benutzers ausgeführt und bei Anmeldung eines Benutzers ausgelöst wird. Der Task registriert den Benutzer und das Gerät im Hintergrund bei Azure AD, nachdem die Anmeldung des Benutzers abgeschlossen ist. 
Den geplanten Task finden Sie in der Aufgabenplanungsbibliothek unter **Microsoft** > **Arbeitsplatzbeitritt**.
Der Task wird für alle Active Directory-Benutzer ausgeführt, die sich bei dem Computer anmelden, und registriert sie. 
In der folgenden Abbildung ist der schrittweise Vorgang für die automatische Geräteregistrierung aufgeführt.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Ein Benutzer (Information-Worker) meldet sich mit Anmeldeinformationen für eine Active Directory-Domäne bei einem Windows 7-Clientcomputer an.
1. Der geplante Task "Arbeitsbereichverknüpfung" wird ausgeführt.
1. Der Benutzer wird im Hintergrund bei AD FS mithilfe der integrierten Windows-Authentifizierung authentifiziert.
1. Der Windows 7-PC wird für den Benutzer in Azure AD registriert.
1. Ein Geräteobjekt und Zertifikat werden in Azure AD erstellt. Das Objekt stellt die user@device dar.
1. Das Arbeitsbereichverknüpfungs-Zertifikat wird auf dem Computer gespeichert.

## Aufheben der Registrierung Ihrer in eine Domäne eingebundenen Windows 7-Geräte

Wahlweise können Sie Ihre Domäne eingebundenen Windows 7-Geräte wie folgt aufheben:
Deinstallieren Sie das Softwarepaket Arbeitsplatzbeitritt auf Ihre Windows 7 eine Domäne eingebundene Geräte mithilfe eines Softwareverteilungssystems wie System Center Configuration Manager.

Öffnen Sie dann eine Eingabeaufforderung auf dem Windows 7-Computer, und führen Sie den folgenden Befehl zum Aufheben der Registrierung des Geräts aus:
    
    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Dieser Befehl muss im Kontext jedes Domänenbenutzers ausgeführt werden, der sich bei dem Computer angemeldet hat.
Ereignisanzeige und Fehler für in eine Domäne eingebundene Windows 7-Geräte

Das Windows-Ereignisprotokoll auf dem Windows 7-Computer zeigt Meldungen im Zusammenhang mit der Arbeitsbereichverknüpfung an. Sie finden dort sowohl Meldungen zu erfolgreichen als auch fehlgeschlagenen Arbeitsbereichverknüpfungs-Ereignissen. Das Ereignisprotokoll finden Sie in der Ereignisanzeige unter "Anwendungs- und Dienstprotokolle > Microsoft -Arbeitsbereichverknüpfung".

## Weitere Themen

- [Azure Active Directory-Geräteregistrierung – Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 8.1-Geräte](active-directory-conditional-access-automatic-device-registration-windows8_1.md)

 


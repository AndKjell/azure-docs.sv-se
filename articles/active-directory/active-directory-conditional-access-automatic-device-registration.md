<properties
    pageTitle="Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte | Microsoft Azure"
    description="IT-Administratoren können Ihre in Domänen eingebundenen Windows-Geräte automatisch und im Hintergrund bei Azure Active Directory (Azure AD) registrieren."
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

# Automatische Geräteregistrierung bei Azure Active Directory für in Domänen eingebundene Windows-Geräte

Als IT-Administrator können Sie Ihre in Domänen eingebundenen Windows-Geräte automatisch und im Hintergrund bei Azure Active Directory (Azure AD) registrieren. Dies ist hilfreich, wenn Sie gerätebasierte Richtlinien für den bedingten Zugriff auf Office 365-Anwendungen oder Anwendungen, die lokal von AD FS verwaltet werden, konfiguriert haben. Sie können weitere Informationen zu geräteregistrierungsszenarios durch Lesen der [Azure Active Directory-Geräteregistrierung – Übersicht](active-directory-conditional-access-device-registration-overview.md).

Automatische Geräteregistrierung bei Azure Active Directory steht für Windows 7- und Windows 8.1-Computer zur Verfügung, die einer Active Directory-Domäne beigetreten sind. In der Regel sind dies Computer, die sich im Besitz von Unternehmen befinden und für Information-Worker zur Verfügung gestellt wurden.

Beachten Sie die folgenden Voraussetzungen, um Ihre in Domänen eingebundenen Windows-Geräte bei Azure AD zu registrieren. Nachdem Sie die Voraussetzungen erfüllt haben, konfigurieren Sie automatische Geräteregistrierung für Ihre in Domänen eingebundenen Windows-Geräte. 

## Voraussetzungen für die automatische Geräteregistrierung von in Domänen eingebundenen Windows-Geräten bei Azure Active Directory

Bereitstellen von AD FS und Verbinden mit Azure Active Directory mittels Azure Active Directory Connect
----------------------------------------------------------------------------------------------
1. Verwenden Sie Azure Active Directory, um Active Directory-Verbunddienste (AD FS) mit Windows Server 2012 R2 bereitzustellen, und richten Sie eine Verbundbeziehung mit Azure Active Directory ein.
2. Konfigurieren einer zusätzlichen Anspruchsregel für die Vertrauensstellung der vertrauenden Seite für Azure Active Directory
3. Öffnen Sie die AD FS-Verwaltungskonsole, und navigieren Sie zu **AD FS**>**Vertrauensstellungen > Vertrauensstellungen für vertrauende Seiten**. Mit der rechten Maustaste auf das Microsoft Office 365 Identity Platform Partei Vertrauensstellungsobjekt, und wählen Sie **Anspruchsregeln bearbeiten...**
4. Auf der **Ausstellungstransformationsregeln** Registerkarte **Regel hinzufügen**.
5. Wählen Sie **Ansprüche mit benutzerdefinierter Regel senden** aus der **Anspruchsregel** Vorlage Dropdown-Feld. Wählen Sie **Weiter**.
6. Typ *Authentifizierungsmethode Anspruchsregel* in die **Name der Anspruchsregel:** Textfeld.
7. Geben Sie die folgende Anspruchsregel in das **Anspruchsregel:** Textfeld:

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);
    
8. Klicken Sie auf **OK** zweimal, um das Dialogfeld zu schließen.

Konfigurieren eines zusätzlichen Authentifizierungsklassenverweises für die Vertrauensstellung der vertrauenden Seite für Azure Active Directory
-----------------------------------------------------------------------------------------------------
Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann Folgendes ein:


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`
   
Where <RPObjectName> Objektname der vertrauenden Seite für Ihr Azure Active Directory Vertrauensstellungsobjekt basiert. Dieses Objekt trägt normalerweise den Namen "Microsoft Office 365 Identity Platform".

Globale Authentifizierungsrichtlinie für AD FS
-----------------------------------------------------------------------------
Konfigurieren Sie die globale primäre Authentifizierungsrichtlinie für AD FS, um die integrierte Windows-Authentifizierung für das Intranet zuzulassen (Dies ist die Standardeinstellung).


Internet Explorer-Konfiguration
------------------------------------------------------------------------------
Konfigurieren Sie die folgenden Einstellungen in Internet Explorer auf Ihren Windows-Geräten für die Sicherheitszone "Lokales Intranet":

- Keine Aufforderung zur Clientzertifikatauswahl, wenn nur ein Zertifikat vorhanden ist:  **Aktivieren**
- Scripting zulassen:  **Aktivieren**
- Automatisches Anmelden nur in der Intranetzone:  **aktiviert**

Dies sind die Standardeinstellungen für die Sicherheitszone "Lokales Intranet" des Internet Explorers. Sie können anzeigen oder verwalten Sie diese Einstellungen in Internet Explorer durch Navigieren zu **Internetoptionen** > **Security** > Lokales Intranet > Stufe. Sie können diese Einstellungen auch mithilfe der Active Directory-Gruppenrichtlinie konfigurieren.

Netzwerkverbindung
-------------------------------------------------------------
In Domänen eingebundene Windows-Gerät müssen über eine Verbindung zu AD FS und Active Directory-Domänencontroller verfügen, um sich automatisch bei Azure AD zu registrieren. Dies bedeutet normalerweise, dass der Computer mit dem Unternehmensnetzwerk verbunden sein muss. Dies kann eine Kabelverbindung, Wi-Fi-Verbindung, DirectAccess oder VPN einschließen.

## Konfigurieren der Ermittlung für die Azure Active Directory-Geräteregistrierung
Windows 7 und Windows 8.1-Geräte ermitteln den Geräteregistrierungsserver, indem sie den Benutzerkontonamen mit dem Namen eines bekannten Geräteregistrierungsservers kombinieren. Sie müssen einen DNS CNAME-Eintrag erstellen, der auf den A-Eintrag verweist, der Ihrem Azure Active Directory-Geräteregistrierungsdienst zugeordnet ist. Der CNAME-Eintrag muss das bekannte Präfix verwenden **Enterpriseregistration** gefolgt vom UPN-Suffix, das von den Benutzerkonten in Ihrer Organisation verwendet. Wenn Ihre Organisation mehrere UPN-Suffixe verwendet, müssen in DNS mehrere CNAME-Einträge erstellt werden.

Wenn Sie z. B. in Ihrer Organisation zwei UPN-Suffixe namens "@contoso.com" und "@region.contoso.com" verwenden, erstellen Sie die folgenden DNS-Einträge.

| Eintrag                                     | Typ  | Adresse                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.windows.net |

##Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 7- und Windows 8.1-Geräte

Konfigurieren Sie die automatische Geräteregistrierung für Ihre in eine Domäne eingebundene Windows 7- und Windows 8.1-Geräte anhand der folgenden Links. Stellen Sie sicher, dass Sie die oben genannten Voraussetzungen abgeschlossen haben, bevor Sie fortfahren.

* [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 8.1-Geräte](active-directory-conditional-access-automatic-device-registration-windows8_1.md)

* [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 7-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)

Zusätzliche Hinweise
--------------------------------------------------------------------

Geräteregistrierung bei Azure AD bietet die breiteste Palette von Gerätefunktionen. Mit Azure AD-Geräteregistrierung können Sie persönliche mobile Geräte (BYOD) und in Domänen eingebundene firmeneigene Geräte registrieren. Die Geräte können mit beiden gehosteten Diensten, wie z. B. Office 365 und Diensten, die lokal mit AD FS verwaltet werden, verwendet werden. 

Unternehmen, die sowohl mobile als auch herkömmliche Geräte oder Office 365, Azure AD oder andere Microsoft-Dienste verwenden, sollten Geräte bei Azure AD mithilfe des Azure AD-Geräteregistrierungsdienstes registrieren. Wenn Ihr Unternehmen keine mobilen Geräte und keine Microsoft-Dienste wie z. B. Office 365, Azure AD oder Microsoft Intune verwendet und stattdessen nur lokale Anwendungen hostet, dann können Sie Geräte mittels AD FS bei Active Directory registrieren. 

Weitere Informationen finden Sie Informationen zum Bereitstellen der geräteregistrierung mit AD FS [hier](https://technet.microsoft.com/library/dn486831.aspx).

## Weitere Themen

- [Azure Active Directory-Geräteregistrierung – Übersicht](active-directory-conditional-access-device-registration-overview.md)
- [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 7-Geräte](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Konfigurieren der automatischen Geräteregistrierung für in eine Domäne eingebundene Windows 8.1-Geräte](active-directory-conditional-access-automatic-device-registration-windows8_1.md)


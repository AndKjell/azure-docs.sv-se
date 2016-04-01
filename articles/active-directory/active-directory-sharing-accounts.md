<properties
    pageTitle="Freigeben von Konten, die mithilfe von Azure AD |  Microsoft Azure"
    description="Beschreibt, wie es Azure Active Directory Organisationen ermöglicht, Konten für lokale Apps und für Verbraucherclouddienste auf sichere Weise gemeinsam zu nutzen."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="stevenpo"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2015"  
    ms.author="stevenpo"/>

# Freigeben von Konten in Azure AD

## Übersicht
Es kann vorkommen, dass in Unternehmen ein Benutzername mit Kennwort für mehrere Personen verwendet werden muss. Dies geschieht in der Regel in zwei Fällen:

- Beim Zugriff auf Anwendungen, die einen eindeutigen Benutzernamen und ein Kennwort für jeden Benutzer erfordern, wobei es sich dabei um lokale Apps oder Clouddienste für Verbraucher (z. B. Unternehmenskonten für Social Media) handeln kann.
- Beim Erstellen von Mehrbenutzerumgebungen. Möglicherweise verfügen Sie über ein einzelnes lokales Konto mit erhöhten Rechten, das zur Durchführung des Kernsetups sowie für Verwaltungs- und Wiederherstellungsvorgänge verwendet wird (beispielsweise das lokale "globale Administratorkonto" für Office 365, oder das Stammkonto in Salesforce).

Normalerweise würden diese Konten gemeinsam genutzt werden, indem die Anmeldeinformationen (Benutzername/Kennwort) an geeignete Personen verteilt oder an gemeinsam genutzten Orten aufbewahrt werden, auf die vertrauenswürdige Personen Zugriff haben.

Das herkömmliche Modell der gemeinsamen Nutzung hat mehrere Nachteile:

- Das Bereitstellen des Zugriffs auf neue Anwendungen erfordert die Verteilung von Anmeldeinformationen an jeden, der Zugriff benötigt.
- Jede freigegebene Anwendung erfordert möglicherweise einen eigenen, eindeutigen Satz gemeinsam genutzter Anmeldeinformationen, sodass die Benutzer mehrere verschiedene Anmeldeinformationen kennen müssen. Wenn Benutzer viele Anmeldeinformationen merken müssen, erhöht das Risiko, dass sie risikoreiche Methoden zurückgreifen werden. (z. B. aufzuschreiben Kennwörter).
- Es ist nicht erkennbar, wer genau Zugriff auf eine Anwendung hat.
- Sie können nicht erkennen, wer hat *zugegriffen* einer Anwendung.
- Wenn der Zugriff auf eine Anwendung entfernt werden soll, müssen die Anmeldeinformationen aktualisiert und erneut an alle Benutzer verteilt werden, die Zugriff benötigen.

## Azure Active Directory-Kontofreigabe

Azure AD bietet einen neuen Ansatz für die Verwendung von gemeinsam genutzten Konten, der diese Nachteile beseitigt.

Der Azure AD-Administrator konfiguriert, auf welche Anwendungen ein Benutzer zugreifen kann, indem er den Zugriffsbereich verwendet und die Art von einmaligem Anmelden auswählt, die für diese Anwendung am besten geeignet ist. Einen dieser Typen *kennwortbasierte einmalige Anmeldung*, können mit Azure AD fungieren als eine Art "Broker" bei der Anmeldung für die app.

Benutzer melden Sie sich einmal mit ihrem Organisationskonto an. Dies ist dasselbe Konto, das Benutzer regelmäßig zum Zugreifen auf den Desktop oder ihre E-Mail verwenden. Sie können nur diejenigen Anwendungen ermitteln und auf sie zugreifen, die ihnen zugewiesen wurden. Bei gemeinsam genutzten Konten kann diese Liste von Anwendungen beliebig viele gemeinsam genutzte Anmeldeinformationen enthalten. Der Endbenutzer muss sich die unterschiedlichen Konten nicht mehr merken oder notieren.

Gemeinsam genutzte Konten verbessern nicht nur den Überblick, sondern auch die Sicherheit. Benutzer mit Berechtigungen zur Verwendung der Anmeldeinformationen sehen das gemeinsame Kennwort nicht, sondern erhalten vielmehr die Berechtigung, es als Teil des orchestrierten Authentifizierungsablaufs nutzen zu dürfen. Darüber hinaus steht bei einigen Kennwort-SSO-Anwendungen die Option zur Verfügung, das Kennwort in regelmäßigen Abständen von Azure AD gegen ein langes, komplexes Kennwort austauschen/aktualisieren zu lassen, wodurch die Kontosicherheit erhöht wird. Der Administrator kann Zugriffsrechte auf bestimmte Anwendungen mühelos gewähren oder widerrufen. Er weiß auch, wer genau Zugriff auf das Konto hat und wer bereits darauf zugegriffen hat.

Azure AD unterstützt freigegebene Konten für alle lizenzierten Benutzer von Enterprise Mobility Suite (EMS) Premium oder Basic für alle Typen des einmaligen Anmeldens per Kennwort. Sie können Konten für alle in der Galerie für integrierte Anwendung Tausende freigeben und können Ihre eigene Anwendung Kennwort-Authentifizierung mit hinzufügen [benutzerdefinierte SSO-apps](active-directory-single-sign-on-newly-acquired-saas-apps.md).

Zu den Azure AD-Features zur gemeinsamen Nutzung von Konten gehören folgende Funktionen:

- [Einmaliges Anmelden per Kennwort](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Agent für einmaliges Anmelden per Kennwort
- [Gruppenzuweisung](active-directory-accessmanagement-self-service-group-management.md)
- Apps mit benutzerdefinierten Kennwörtern
- [Dashboard für Anwendungsnutzung/Berichte](active-directory-passwords-get-insights.md)
- Zugriffsbereiche für Endbenutzer
- [Anwendungsproxy](active-directory-application-proxy-get-started.md)
- [Active Directory Marketplace](http://azure.microsoft.com/marketplace/active-directory/all/)

## Gemeinsames Nutzen eines Kontos
Um Azure AD zur gemeinsamen Verwendung eines Kontos zu verwenden, sind folgende Schritte erforderlich:

- Hinzufügen einer Anwendung [Anwendungskatalog](https://azure.microsoft.com/marketplace/active-directory/) oder [benutzerdefinierte Anwendung](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfigurieren der Anwendung für einmaliges Anmelden per Kennwort (SSO)
- Verwendung [Gruppenbasierte Zuweisung](active-directory-accessmanagement-group-saasapps.md) und wählen Sie die Option zur Eingabe von Anmeldeinformationen mit einem gemeinsamen
- Optional: in einigen Clientanwendungen wie Facebook, Twitter und LinkedIn, aktivieren Sie die Option für [Azure AD automatisierte drehendes Kennwort](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Sie können auch Ihre freigegebene Konto sicherer machen mit Multi-Factor Authentication (MFA) (erfahren Sie mehr über [Sichern der Anwendung in Azure AD](multi-factor-authentication-get-started.md)) und Sie können die Fähigkeit zum Verwalten der Zugriff auf die Anwendung mit Delegieren [Azure AD Self-Service-](active-directory-accessmanagement-self-service-group-management.md) Group Policy Management.

## Verwandte Artikel

- [Schützen von Apps durch bedingten Zugriff](active-directory-conditional-access.md)
- [Self-Service-Gruppenverwaltung/SSAA](active-directory-accessmanagement-self-service-group-management.md)



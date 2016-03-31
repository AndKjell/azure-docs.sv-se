<properties
    pageTitle="Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung | Microsoft Azure"
    description="Eine Schrittanleitung, die zeigt, wie Sie mithilfe der Active Directory-Verbunddienste (AD FS) in Windows Server 2012 R2 den bedingten Zugriff auf lokale Anwendungen ermöglichen."
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


# Einrichten des lokalen bedingten Zugriffs mithilfe der Azure Active Directory-Geräteregistrierung

Eigene Geräte der Benutzer können als Ihrer Organisation bekannt markiert werden. Dazu müssen die Benutzer für ihre Geräte eine Arbeitsbereichverknüpfung mit dem Geräteregistrierungsdienst für Azure Active Directory durchführen. Die folgende Schrittanleitung zeigt, wie Sie mithilfe der Active Directory-Verbunddienste (AD FS) in Windows Server 2012 R2 den bedingten Zugriff auf lokale Anwendungen ermöglichen. 

> [AZURE.NOTE]
> Wenn Geräte verwendet werden, die über den Azure Active Directory-Geräteregistrierungsdienst registriert und mit Richtlinien für den bedingten Zugriff konfiguriert sind, wird eine Office 365-Lizenz oder eine Azure AD Premium-Lizenz benötigt. Hierzu gehören auch Richtlinien, die von den Active Directory-Verbunddiensten (AD FS) für lokale Ressourcen erzwungen werden.

Weitere Informationen zu den Szenarien für bedingten Zugriff für lokale, finden Sie unter [Verbinden mit einem Arbeitsplatz von einem beliebigen Gerät für SSO und nahtlose zweistufige Authentifizierung über Unternehmensanwendungen](https://technet.microsoft.com/library/dn280945.aspx).

Diese Funktionen sind für Kunden verfügbar, die eine Azure Active Directory Premium-Lizenz erwerben.

Unterstützte Geräte
-------------------------------------------------------------------------
* Windows 7-Geräte, die einer Domäne angehören.
* Persönliche Windows 8.1-Geräte und Windows 8.1-Geräte, die einer Domäne angehören.
* iOS 6 und höher, für Safari-Browser
* Telefone mit Android 4.0 oder höher, Samsung GS3 oder höher, Tablets mit Samsung Note2 oder höher.


Voraussetzungen für die einzelnen Szenarien
------------------------------------------------------------------------
* Abonnement für Office 365 oder Azure Active Directory Premium
* Azure Active Directory Tenant
* Windows Server Active Directory (Windows Server 2008 oder höher)
* Aktualisiertes Schema in Windows Server 2012 R2
* Azure Active Directory Premium-Abonnement
* Windows Server 2012 R2-Verbunddienste, konfiguriert für SSO in Azure AD
* Windows Server 2012 R2-Webanwendungsproxy Microsoft Azure Active Directory Connect (Azure AD Connect). [Herunterladen von Azure AD Connect hier](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Überprüfte Domäne. 

Bekannte Probleme in dieser Version
-------------------------------------------------------------------------------
* Gerätebasierte bedingte Zugriffsrichtlinien erfordern das Zurückschreiben von Geräteobjekten aus Azure Active Directory nach Active Directory. Es kann bis zu drei Stunden dauern, bis Geräteobjekte nach Active Directory zurückgeschrieben werden.
* iOS 7-Geräte fordern den Benutzer immer auf, während der Zertifikatauthentifizierung ein Zertifikat auszuwählen. 
* Einige Versionen von iOS 8 vor iOS 8.3 funktionieren nicht. 

## Annahmen für das Szenario
Bei diesem Szenario wird davon ausgegangen, dass Sie über eine Hybrid-Umgebung verfügen, die aus einem Azure AD-Mandanten und einer lokalen aktiven Active Directory-Komponente besteht. Diese Mandanten sollten mit Azure AD Connect sowie mit einer überprüften Domäne und AD FS für SSO verbunden sein. Die unten angegebene Checkliste hilft Ihnen bei der Konfiguration Ihrer Umgebung für die oben beschriebene Stufe. 

Checkliste: Erforderliche Komponenten für Szenario mit bedingtem Zugriff
--------------------------------------------------------------
Verbinden Sie Ihren Azure AD Tenant mit dem lokalen Active Directory-Element. 

## Konfigurieren des Azure Active Directory-Geräteregistrierungsdiensts
Verwenden Sie diese Anleitung, um den Azure Active Directory-Geräteregistrierungsdienst für Ihre Organisation bereitzustellen und zu konfigurieren.

In diesem Handbuch wird vorausgesetzt, dass Sie Windows Server Active Directory konfiguriert und Microsoft Azure Active Directory abonniert haben. Weitere Informationen zu den Voraussetzungen finden Sie oben.

Wenn Sie den Azure Active Directory-Geräteregistrierungsdienst für Ihren Azure Active Directory-Mandanten bereitstellen möchten, führen Sie die in der folgenden Prüfliste aufgeführten Aufgaben in der angegebenen Reihenfolge aus. Wenn ein Ressourcenlink auf ein grundlegendes Thema verweist, kehren Sie nach der Durchsicht des grundlegenden Themas zu dieser Prüfliste zurück, um die verbleibenden Aufgaben in dieser Prüfliste auszuführen. Einige Aufgaben erfordern einen Überprüfungsschritt für das Szenario, mit dem bestätigt werden kann, dass der Schritt erfolgreich abgeschlossen wurde.

## Teil 1: Aktivieren der Azure Active Directory-Geräteregistrierung

Führen Sie die Aufgaben in der Prüfliste unten aus, um den Azure Active Directory-Geräteregistrierungsdienst zu aktivieren und zu konfigurieren.

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Aktivieren Sie Geräteregistrierung in Ihrem Azure Active Directory-Mandanten, damit Geräte dem Arbeitsplatz beitreten können. Standardmäßig ist die mehrstufige Authentifizierung nicht für den Dienst aktiviert. Es wird jedoch empfohlen, beim Registrieren eines Geräts die mehrstufige Authentifizierung zu verwenden. Stellen Sie vor dem Aktivieren der mehrstufigen Authentifizierung in ADRS sicher, dass AD FS für einen MFA-Anbieter (Multi-Factor Authentication) konfiguriert ist. | [Aktivieren der Azure Active Directory-Geräteregistrierung](active-directory-conditional-access-device-registration-overview.md)               |
| Geräte ermitteln den Azure Active Directory-Geräteregistrierungsdienst, indem sie nach bekannten DNS-Einträgen suchen. Sie müssen den DNS-Eintrag für Ihr Unternehmen so konfigurieren, dass Geräte den Azure Active Directory-Geräteregistrierungsdienst ermitteln können.                                                                                                                                                   | [Konfigurieren der Ermittlung für die Azure Active Directory-Geräteregistrierung](active-directory-conditional-access-device-registration-overview.md) |

##Teil 2: Bereitstellen und Konfigurieren von Windows Server 2012 R2 Active Directory-Verbunddiensten und Einrichten einer Partnerbeziehung mit Azure AD

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Bereitstellen der Active Directory-Domänendienste mit den Windows Server 2012 R2-Schemaerweiterungen. Es ist nicht erforderlich, ein Upgrade Ihrer Domänencontroller auf Windows Server 2012 R2 auszuführen. Das Upgrade des Schemas ist die einzige Anforderung. | [Ausführen eines Upgrades des Schemas der Active Directory-Domänendienste](#Ausführen eines Upgrades des Schemas der Active Directory-Domänendienste)               |
| Geräte ermitteln den Azure Active Directory-Geräteregistrierungsdienst, indem sie nach bekannten DNS-Einträgen suchen. Sie müssen den DNS-Eintrag für Ihr Unternehmen so konfigurieren, dass Geräte den Azure Active Directory-Geräteregistrierungsdienst ermitteln können.                                                                                                                                                   | [Vorbereiten von Active Directory-Unterstützungsgeräten](#Vorbereiten von Active Directory-Unterstützungsgeräten) |


##Teil 3: Aktivieren des Geräterückschreibens in Azure AD

| Aufgabe                                                                                                                                                                                                                                                                                                                                                                                             | Referenz                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Führen Sie Teil 2 von „Aktivieren des Geräterückschreibens in Azure AD Connect“ aus. Kehren Sie nach Abschluss des Vorgangs zu dieser Anleitung zurück. | [Aktivieren von „Geräterückschreiben“ in Azure AD Connect](#Ausführen eines Upgrades des Schemas der Active Directory-Domänendienste)               |
     

##[Optional] Teil 4: Aktivieren der Multi-Factor Authentication

Es wird dringend empfohlen, eine der verschiedenen Optionen für die Multi-Factor Authentication zu konfigurieren. Die MFA erforderlich ist, finden Sie unter [Wählen Sie die Multi-Factor-Lösung für Sie](multi-factor-authentication-get-started.md). Darin ist eine Beschreibung der einzelnen Lösungen enthalten, und außerdem Links mit hilfreichen Informationen zum Konfigurieren der Lösung Ihrer Wahl. 

## Teil 5: Überprüfung

Die Bereitstellung ist jetzt abgeschlossen. Sie können nun einige Szenarien ausprobieren. Folgen Sie den unten angegebenen Links, um mit dem Dienst zu experimentieren und sich mit den Funktionen vertraut zu machen.


| Aufgabe                                                                                                                                                                                                                         | Referenz                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Verknüpfen Sie einige Geräte mit dem Arbeitsbereich, indem Sie die Azure Active Directory-Geräteregistrierung verwenden. Sie können iOS-, Windows- und Android-Geräte verknüpfen.                                                                                         | [Arbeitsbereichverknüpfung für Geräte mithilfe der Azure Active Directory-Geräteregistrierung](#Arbeitsbereichverknüpfung für Geräte mithilfe der Azure Active Directory-Geräteregistrierung) |
| Sie können registrierte Geräte mithilfe des Verwaltungsportals anzeigen und aktivieren/deaktivieren. In dieser Aufgabe zeigen Sie mit dem Administratorportal einige registrierte Geräte an.                                                        | [Azure Active Directory-Geräteregistrierung – Übersicht](active-directory-conditional-access-device-registration-overview.md)                             |
| Stellen Sie sicher, dass Geräteobjekte aus Azure Active Directory in Windows Server Active Directory zurückgeschrieben werden.                                                                                                                  | [Überprüfen, ob registrierte Geräte nach Active Directory zurückgeschrieben werden](#Überprüfen, ob registrierte Geräte nach Active Directory zurückgeschrieben werden)                  |
| Da Benutzer ihre Geräte jetzt registrieren können, können Sie Richtlinien für den Anwendungszugriff in AD FS erstellen, bei denen nur registrierte Geräte zugelassen sind. In dieser Aufgabe erstellen Sie eine Regel für den Anwendungszugriff und eine benutzerdefinierte Meldung „Zugriff verweigert“. | [Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“](#Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“)            |



## Integrieren von Azure Active Directory in lokales Active Directory
Diese Anleitung hilft Ihnen bei der Integration Ihres Azure AD-Mandanten in das lokale Active Directory mithilfe von Azure AD Connect. Auch wenn die Schritte im Azure-Portal verfügbar sind, sollten Sie die speziellen Anweisungen in diesem Abschnitt beachten. 

1.  Melden Sie sich als Administrator beim Azure-Portal an.
2.  Wählen Sie im linken Bereich **Active Directory**.
3.  Auf der **Verzeichnis** Registerkarte, wählen Sie Ihr Verzeichnis.
4.  Wählen Sie die **Verzeichnisintegration** Registerkarte.
5.  Unter **Bereitstellen und Verwalten von** Abschnitt, führen Sie die Schritte 1 bis 3, um Azure Active Directory in Ihrem lokalen Verzeichnis integrieren.
  1.    Fügen Sie Domänen hinzu.
  2.    Installieren und Ausführen von Azure AD Connect: Installieren von Azure AD Connect mithilfe der folgenden Anweisungen, [benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).
  3. Überprüfen und verwalten Sie die Verzeichnissynchronisierung. In diesem Schritt sind Anweisungen zum einmaligen Anmelden enthalten.
  >[AZURE.NOTE] Konfigurieren Sie mit AD FS-Verbund, wie im Dokument weiter oben Bezug genommen. 
  >[AZURE.NOTE] Sie müssen nicht die Vorschau-Features konfigurieren.
  
   


## Ausführen eines Upgrades des Schemas der Active Directory-Domänendienste
> [AZURE.NOTE]
> Das Upgrade des Active Directory-Schemas kann nicht rückgängig gemacht werden. Es wird empfohlen, dieses Upgrade zuerst in einer Testumgebung auszuführen.

1. Melden Sie sich an Ihrem Domänencontroller mit einem Konto an, das über die Rechte "Unternehmensadministrator" und "Schemaadministrator" verfügt.
2. Kopieren der **[Medien] \support\adprep** Verzeichnis und Unterverzeichnisse auf einen Ihrer Active Directory-Domänencontroller. 
3. Hierbei steht "[medien]" für den Pfad zum Windows Server 2012 R2-Installationsmedium.
4. Navigieren Sie zum Verzeichnis Adprep eine Befehlszeile und führen Sie: **adprep.exe/ForestPrep**. Folgen Sie den Anweisungen auf dem Bildschirm, um das Upgrade des Schemas abzuschließen.

## Vorbereiten von Active Directory für die Unterstützung von Geräten
>[AZURE.NOTE] Dies ist ein Einmaliger Vorgang, den Sie, zum Vorbereiten von Active Directory-Gesamtstruktur ausführen müssen, Geräte zu unterstützen. Sie müssen mit der Berechtigung "Unternehmensadministrator" angemeldet sein, und Ihre Active Directory-Gesamtstruktur muss über das Windows Server 2012 R2-Schema verfügen, damit dieses Verfahren abgeschlossen werden kann.


##Vorbereiten der Active Directory-Gesamtstruktur für die Unterstützung von Geräten

> [AZURE.NOTE]
>Dies ist ein einmaliger Vorgang, den Sie ausführen müssen, um Ihre Active Directory-Gesamtstruktur für die Unterstützung von Geräten vorzubereiten. Sie müssen mit der Berechtigung "Unternehmensadministrator" angemeldet sein, und Ihre Active Directory-Gesamtstruktur muss über das Windows Server 2012 R2-Schema verfügen, damit dieses Verfahren abgeschlossen werden kann.

### Vorbereiten der Active Directory-Gesamtstruktur

1.  Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann Folgendes ein:
     Initialize-ADDeviceRegistration
2.  Wenn Sie aufgefordert werden **ServiceAccountName**, geben Sie den Namen des Dienstkontos, das Sie als Dienstkonto für AD FS ausgewählt haben. Wenn sie ein gMSA-Konto ist, geben Sie das Konto in der **Domäne\Kontoname$** Format. Verwenden Sie für ein Domänenkonto ein, das Format **Domäne\Kontoname**.



### Aktivieren der Geräteauthentifizierung in AD FS

1. Klicken Sie auf dem Verbundserver die AD FS-Verwaltungskonsole öffnen, und navigieren Sie zu **AD FS** > **Authentifizierungsrichtlinien**.
2. Wählen Sie **globale primäre Authentifizierung bearbeiten...** aus der **Aktionen** Bereich.
3. Überprüfen Sie **Geräteauthentifizierung aktivieren** und wählen Sie dann**OK**.
4. Standardmäßig entfernt AD FS regelmäßig nicht verwendete Geräte aus Active Directory. Sie müssen diese Aufgabe deaktivieren, wenn Sie die Azure Active Directory-Geräteregistrierung verwenden, damit Geräte in Azure verwaltet werden können.


### Deaktivieren der Bereinigung nicht verwendeter Geräte
1. Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann Folgendes ein:
    Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### Vorbereiten von Azure AD Connect für das Geräterückschreiben

1.  Teil 1 durchführen: Vorbereiten von AAD Connect. 


## Arbeitsbereichverknüpfung für Geräte mithilfe der Azure Active Directory-Geräteregistrierung 

### Verknüpfen eines iOS-Geräts mithilfe der Azure Active Directory-Geräteregistrierung 

Die Azure Active Directory-Geräteregistrierung verwendet einen drahtlosen Profilregistrierungsvorgang für iOS-Geräte. Dieser Vorgang beginnt damit, dass ein Benutzer eine Verbindung mit der Profilregistrierungs-URL mithilfe des Safari-Webbrowsers herstellt. Die URL weist das folgende Format auf:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Hier steht `yourdomainname` für den Domänennamen, den Sie mit Azure Active Directory konfiguriert haben. Wenn Sie z. B. die Domäne "contoso.com" verwenden, lautet die URL folgendermaßen:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Diese URL kann auf unterschiedlichste Weise an die Benutzer kommuniziert werden. Ein empfohlenes Verfahren besteht im Veröffentlichen dieser URL in einer benutzerdefinierten Anwendungsmeldung "Zugriff verweigert" in AD FS. Dies wird im nächsten Abschnitt behandelt: [Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“](#Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung „Zugriff verweigert“).

###Verknüpfen eines Windows 8.1-Geräts mithilfe der Azure Active Directory-Geräteregistrierung

1. Navigieren Sie auf Ihrem Windows 8.1-Gerät zu **PC-Einstellungen** > **Netzwerk** > **Arbeitsplatz**.
2. Geben Sie Ihren Benutzernamen im UPN-Format ein. Beispielsweise dan@contoso.com...
3. Wählen Sie **Verknüpfen**.
4. Melden Sie sich bei Aufforderung mit Ihren Anmeldeinformationen an. Das Gerät ist jetzt verknüpft.

### Verknüpfen eines Android-Geräts mithilfe der Azure Active Directory-Geräteregistrierung 

Die [Azure Authenticator für Android Thema](active-directory-conditional-access-azure-authenticator-app.md) umfasst Anweisungen zum Installieren von Azure Authenticator-app auf Ihrem Android-Gerät und Hinzufügen eines Geschäftskontos. Nach der erfolgreichen Erstellung eines Geschäftskontos auf einem Android-Gerät wird das Gerät per Arbeitsbereich mit der Organisation verknüpft.

## Überprüfen, ob registrierte Geräte nach Active Directory zurückgeschrieben werden
Sie können mithilfe von "LDP.exe" oder ADSI Edit Geräteobjekte anzeigen und bestätigen, dass diese zurück in Ihr Active Directory-Verzeichnis geschrieben wurden. Beide Tools gehören zum Lieferumfang der Administratortools von Active Directory.

Standardmäßig werden Geräteobjekte, die aus Azure Active Directory zurückgeschrieben werden, in der gleichen Domäne platziert wie Ihre AD FS-Farm.

    CN=RegisteredDevices,defaultNamingContext

## Erstellen einer Anwendungszugriffsrichtlinie und einer benutzerdefinierten Meldung "Zugriff verweigert"
Stellen Sie sich folgendes Szenario vor: Sie erstellen eine Anwendungsvertrauensstellung der vertrauenden Seite in AD FS und konfigurieren eine Ausstellungsautorisierungsregel , die nur registrierte Geräte zulässt. Nun dürfen ausschließlich registrierte Geräte auf die Anwendung zugreifen. Damit der Zugriff auf die Anwendung für Ihre Benutzer vereinfacht wird, konfigurieren Sie eine benutzerdefinierte Meldung "Zugriff verweigert", die Anweisungen zum Hinzufügen von Geräten enthält. Jetzt steht Ihren Benutzern ein nahtloses Verfahren zum Registrieren ihrer Geräte zur Verfügung, um Zugriff auf eine Anwendung zu erhalten.

Die folgenden Schritte zeigen, wie dieses Szenario implementiert wird.
>[AZURE.NOTE]
Dieser Abschnitt setzt voraus, dass Sie bereits eine Vertrauensstellung der vertrauenden Seite für Ihre Anwendung in AD FS konfiguriert haben.

1. Öffnen Sie die AD FS-Verwaltungskonsole, und navigieren Sie zu "AD FS > Vertrauensstellungen > Vertrauensstellungen der vertrauenden Seite".
2. Suchen Sie nach der Anwendung, für die diese neue Zugriffsregel gelten soll. Klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie dann "Anspruchsregeln bearbeiten" aus.
3. Wählen Sie die **Ausstellungsautorisierungsregeln** Registerkarte, und wählen Sie dann **Regel hinzufügen...**
4. Aus der **Anspruchsregel** Vorlage Dropdown-Liste **Zulassen oder verweigern Benutzern auf der Grundlage eines eingehenden Anspruchs**. Wählen Sie **Weiter**.
5. Der Name der Anspruchsregel: Feldtyp: **Zugriff von registrierten Geräten zulassen**
6. Typ der eingehenden Anspruchs: Dropdown-Liste **ist registrierter Benutzer**.
7. In der eingehenden Anspruchswert: Feld: **true**
8. Wählen Sie die **ermöglichen den Zugriff auf Benutzer mit diesem eingehenden Anspruch** Optionsfeld.
9. Wählen Sie **Fertig stellen** und wählen Sie dann **Übernehmen**.
10. Entfernen Sie alle Regeln, die weniger restriktiv sind als die Regel, die Sie soeben erstellt haben. Entfernen Sie z. B. das standardmäßige **ermöglichen den Zugriff auf alle Benutzer** Regel.

Ihre Anwendung ist nun so konfiguriert, dass der Zugriff nur erlaubt wird, wenn der Benutzer ein registriertes Gerät verwendet, das dem Arbeitsplatz hinzugefügt wurde. Erweiterte Zugriffsrichtlinien finden Sie unter [Verwalten von Risiken mit der mehrstufigen Zugriffssteuerung](https://technet.microsoft.com/library/dn280949.aspx).

Im nächsten Schritt konfigurieren Sie eine benutzerdefinierte Fehlermeldung für Ihre Anwendung. Diese Fehlermeldung informiert den Benutzer, dass er sein Gerät dem Arbeitsplatz hinzufügen muss, bevor er auf die Anwendung zugreifen darf. Sie können eine benutzerdefinierte Anwendungsmeldung "Zugriff verweigert" mithilfe von benutzerdefiniertem HTML und Windows PowerShell erstellen.

Öffnen Sie auf Ihrem Verbundserver ein Windows PowerShell-Befehlsfenster, und geben Sie dann den folgenden Befehl ein. Sie müssen Teile des Befehls durch Elemente ersetzen, die für Ihr System spezifisch sind:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Sie müssen Ihr Gerät registrieren, bevor Sie auf diese Anwendung zugreifen können.

**Wenn Sie ein iOS-Gerät verwenden, wählen Sie diesen Link, um Ihr Gerät verbinden**: 

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Dieses iOS-Gerät mit Ihrem Arbeitsplatz verknüpfen


**Wenn Sie ein Windows 8.1-Gerät verwenden**, Sie können Ihr Gerät verknüpfen, indem Sie auf ** PC-Einstellungen ** > **Netzwerk** > **Arbeitsplatz**.


Wobei "**der vertrauenden Seite Parteiname Vertrauensstellung**" ist der Name des Objekts Applikationen Vertrauensstellung der vertrauenden Seite in AD FS. 
Wobei **IhreDomäne.com** ist der Domänenname, den Sie mit Azure Active Directory konfiguriert haben. Beispiel: contoso.com. 
Achten Sie darauf, dass Sie alle Zeilenumbrüche (sofern vorhanden) aus dem html-Inhalt zu entfernen, die Sie zum Übergeben der **Set-AdfsRelyingPartyWebContent** Cmdlet.


Wenn Benutzer auf Ihre Anwendung jetzt über ein Gerät zugreifen, das nicht unter dem Azure Active Directory-Geräteregistrierungsdienst registriert ist, wird eine Seite angezeigt, die dem unten angegebenen Screenshot ähnelt. 

![Screenshot eines Fehlers, den Benutzer erhalten, wenn sie ihr Gerät nicht unter Azure AD registriert haben](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)


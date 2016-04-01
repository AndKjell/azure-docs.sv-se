<properties
    pageTitle="Einmaliges Anmelden mit Anwendungsproxy | Microsoft Azure"
    description="Erläutert das Bereitstellen von einmaligem Anmelden mit dem Azure AD-Anwendungsproxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2015"
    ms.author="kgremban"/>



# Einmaliges Anmelden mit Anwendungsproxy
Die einmalige Anmeldung ist ein wichtiges Element des Azure AD-Anwendungsproxys. Sie bietet optimale Benutzerfreundlichkeit: Ein Benutzer meldet sich bei der Cloud an, alle Sicherheitsprüfungen finden in der Cloud statt (Vorauthentifizierung), und wenn die Anforderung an die lokale Anwendung gesendet wird, nimmt der Anwendungsproxyconnector die Identität des Benutzers an, damit die Back-End-Anwendung von einem regulären Benutzer eines mit einer Domäne verknüpften Geräts ausgeht.
[](.media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)
Azure AD-Anwendungsproxy können Sie in einer Single-Sign-on (SSO) für Ihre Benutzer kommen. Gehen Sie folgendermaßen vor, um Ihre Apps mit einmaligem Anmelden zu veröffentlichen:


- Einmaliges Anmelden für lokale IWA-Apps unter Verwendung von KCD mit Anwendungsproxy
- SSO für Nicht-Windows-Apps
- Der Umgang mit SSO im lokalen Umfeld unterscheidet sich von dem mit Cloud-Identitäten.

## Einmaliges Anmelden für lokale IWA-Apps unter Verwendung von KCD mit Anwendungsproxy
Sie können für Anwendungen, die die integrierte Windows-Authentifizierung (IWA) verwenden, einmaliges Anmelden (SSO) aktivieren, indem Sie einem Anwendungsproxy-Connector in Active Directory die Berechtigung erteilen, die Identität von Benutzern anzunehmen und Token in deren Namen zu senden und zu empfangen.

> [AZURE.IMPORTANT] Anwendungsproxy ist ein Feature, ist nur verfügbar, wenn Sie auf die Premium oder Basic Edition von Azure Active Directory aktualisiert. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).


### Netzwerkdiagramm

![Flussdiagramm für Microsoft AAD-Authentifizierung](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

Dieses Diagramm erläutert die Vorgänge, die bei einem Zugriff eines Benutzers auf eine lokale Anwendung, die IWA verwendet, ablaufen. Im Allgemeinen geschieht Folgendes:

1. Der Benutzer gibt die URL ein, um über den Anwendungsproxy auf die lokale Anwendung zuzugreifen.
2. Der Anwendungsproxy leitet die Anforderung zur Vorauthentifizierung an Azure AD-Authentifizierungsdienste weiter. Zu diesem Zeitpunkt wendet Azure AD alle gültigen Authentifizierungs- und Autorisierungsrichtlinien an, wie z. B. mehrstufige Authentifizierung. Nachdem der Benutzer überprüft wurde, erstellt Azure AD ein Token und sendet es an den Benutzer.
3. Der Benutzer übergibt das Token an den Anwendungsproxy.
4. Der Anwendungsproxy überprüft das Token und ruft den Benutzerprinzipalnamen (UPN) daraus ab. Dann sendet er die Anforderung, den UPN und den Dienstprinzipalnamen (SPN) über einen zweifach authentifizierten, sicheren Kanal an den Connector.
5. Der Connector führt eine KCD-Aushandlung mit dem lokalen AD aus und nimmt dabei die Identität des Benutzers an, um ein Kerberos-Token für die Anwendung zu erhalten.
6. Active Directory sendet das Kerberos-Token für die Anwendung an den Connector.
7. Der Connector sendet die ursprüngliche Anforderung an den Anwendungsserver und verwendet dabei das von AD empfangene Kerberos-Token.
8. Die Anwendung sendet die Antwort an den Connector, die dann an den Anwendungsproxydienst und schließlich an den Benutzer zurückgegeben wird.

### Voraussetzungen

1. Stellen Sie sicher, dass Ihre Apps, wie z. B. Ihre SharePoint-Web-Apps, zur Verwendung der integrierten Windows-Authentifizierung konfiguriert sind. Weitere Informationen finden Sie unter [Unterstützung für Kerberos-Authentifizierung aktivieren](https://technet.microsoft.com/library/dd759186.aspx), oder für SharePoint finden Sie unter [Planen der Kerberos-Authentifizierung in SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).
2. Erstellen Sie Dienstprinzipalnamen für Ihre Anwendungen.
3. Stellen Sie sicher, dass der Server, auf dem der Connector ausgeführt wird, und der Server, auf dem die App ausgeführt wird, die Sie veröffentlichen, in die Domäne eingebunden und Teil der gleichen Domäne sind. Weitere Informationen zum Domänenbeitritt finden Sie unter [Hinzufügen eines Computers zu einer Domäne](https://technet.microsoft.com/library/dd807102.aspx).


### Active Directory-Konfiguration

Die Active Directory-Konfiguration variiert in Abhängigkeit davon, ob Ihr Anwendungsproxy-Connector und der veröffentlichte Server sich in derselben Domäne befinden oder nicht.

#### Connector und veröffentlichter Server in der gleichen Domäne



1. Wechseln Sie in Active Directory zu **Tools** > **-Benutzer und-Computer**.
2. Wählen Sie den Server aus, der den Connector ausführt.
3. Klicken Sie mit der rechten Maustaste, und wählen Sie **Eigenschaften** > **Delegierung**.
4. Wählen Sie **Computer bei Delegierungen angegebener Dienste vertrauen** und unter **Dienste, die dieses Konto delegierte Anmeldeinformationen bereitstellen kann**, fügen Sie den Wert für die Identität der Dienstprinzipalnamen (SPN) des Anwendungsservers.
5. Auf diese Weise kann der Anwendungsproxy-Connector die Identität von Benutzern in AD für die Anwendungen annehmen, die in der Liste definiert sind.

![Screenshot des Connector-SVR-Eigenschaftenfensters](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### Connector und veröffentlichter Server in unterschiedlichen Domänen

1. Eine Liste der erforderlichen Komponenten für die Arbeit mit der eingeschränkten Kerberos-Delegierung über Domänengrenzen hinweg, finden Sie unter [eingeschränkte Kerberos-Delegierung über Domänengrenzen hinweg](https://technet.microsoft.com/library/hh831477.aspx).
2. Verwenden Sie unter Windows 2012 R2 die Eigenschaft `principalsallowedtodelegateto` auf dem Connector-Server, um für den Anwendungsproxy das Delegieren für den Connector-Server zu aktivieren, wobei `sharepointserviceaccount` der veröffentlichte Server und `connectormachineaccount` der delegierende Server ist.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount` kann die SPS-Computerkonto oder ein Dienstkonto, unter dem der SPS-Anwendungspool ausgeführt wird.


### Konfiguration des Azure-Portals

1. Veröffentlichen der Anwendung gemäß der Anleitung in [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md). Stellen Sie sicher, dass **Azure Active Directory** als die **Vorauthentifizierungsmethode**.
2. Nachdem die Anwendung in der Liste der Programme angezeigt wird, wählen Sie ihn, und klicken Sie auf **konfigurieren**.
3. Unter **Eigenschaften**, legen **interne Authentifizierungsmethode** zu **integrierte Windows-Authentifizierung**.<br>![Erweiterte Anwendungskonfiguration](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)

4. Geben Sie die **SPN der internen Anwendung** des Anwendungsservers. In diesem Beispiel ist der SPN für die veröffentlichte Anwendung http/lob.contoso.com.

>[AZURE.IMPORTANT] Die UPNs in Azure Active Directory müssen mit den UPNs in Ihrem lokalen Active Directory in der Reihenfolge für die Vorauthentifizierung arbeiten identisch sein. Stellen Sie sicher, dass Ihr Azure Active Directory mit Ihrem lokalen Active Directory synchronisiert ist.

| | |
| --- | --- |
| Interne Authentifizierungsmethode | Bei Verwendung von Azure AD für die Vorauthentifizierung können Sie eine interne Authentifizierungsmethode festlegen, die Ihren Benutzern ermöglicht, vom einmaligen Anmelden (SSO) bei dieser Anwendung zu profitieren. <br><br> Wählen Sie **integrierte Windows-Authentifizierung** (IWA), wenn Ihre Anwendung IWA verwendet und Sie Kerberos eingeschränkte Delegierung (KCD) zum Aktivieren von SSO für diese Anwendung konfigurieren können. Anwendungen, die IWA verwenden, müssen mithilfe von KCD konfiguriert werden. Ansonsten kann der Anwendungsproxy diese Anwendungen nicht veröffentlichen. <br><br> Wählen Sie **keine** wenn Ihre Anwendung IWA nicht verwendet. |
| Interner Anwendungs-SPN | Dies ist der Dienstprinzipalname (SPN) der internen Anwendung wie im lokalen Azure AD konfiguriert. Der SPN wird vom Anwendungsproxy-Connector verwendet, um Kerberos-Token für die Anwendung mithilfe der eingeschränkten Kerberos-Delegierung (KCD) abzurufen.  |

<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg


## SSO für Nicht-Windows-Apps
Der Ablauf der Kerberos-Delegierung im Azure AD-Anwendungsproxy wird gestartet, wenn Azure AD den Benutzer in der Cloud authentifiziert. Sobald die Anforderung lokal ankommt, gibt der Azure AD-Anwendungsproxy-Connector durch Interaktion mit dem lokalen Active Directory ein Kerberos-Ticket für den Benutzer aus. Dieser Prozess wird als Kerberos Constrained Delegation (KCD) bezeichnet. In der nächsten Phase wird mit diesem Kerberos-Ticket eine Anforderung an die Back-End-Anwendung gesendet. Es gibt eine Reihe von Protokollen, die definieren, wie solche Anforderungen gesendet werden. Die meisten Nicht-Windows-Server erwarten Negotiate/SPNego, das jetzt im Azure AD-Anwendungsproxy unterstützt wird.<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### Teilweise delegierte Identität
Nicht-Windows-Anwendung wird in der Regel die Benutzeridentität in Form eines Benutzernamens oder SAM-Kontonamen, keine e-Mail-Adresse (username@domain) erhalten. Dies unterscheidet sich von den meisten Windows-basierten Systemen, bei denen eher ein UPN bevorzugt wird, der aussagekräftiger ist und dafür sorgt, dass keine domänenübergreifende Duplizierung vorliegt.
Aus diesem Grund lässt sich bei Anwendungsproxy für jede Anwendung festlegen, welche Identität im Kerberos-Ticket verwendet wird. Einige dieser Optionen eignen sich für Systeme, die kein E-Mail-Adressformat akzeptieren.<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)
Wenn eine partielle Identität verwendet wird, die möglicherweise nicht für alle Domänen oder Gesamtstrukturen der Organisation eindeutig ist, kann es sinnvoll sein, die betreffenden Anwendungen zweimal mit zwei unterschiedlichen Connectorgruppen zu veröffentlichen. Da jede Anwendung einen anderen Benutzerkreis aufweist, lassen sich die Connectors mit einer unterschiedlichen Domäne verknüpfen.


## Der Umgang mit SSO im lokalen Umfeld unterscheidet sich von dem mit Cloud-Identitäten.
Sofern nicht anderweitig konfiguriert, geht der Anwendungsproxy davon aus, dass Benutzer in der Cloud genau dieselbe Identität wie lokal besitzen. Für jede Anwendung lässt sich festlegen, welche Identität beim Ausführen der einmaligen Anmeldung verwendet wird.
Diese Funktion ermöglicht vielen Organisationen mit unterschiedlichen lokalen und Cloudidentitäten das einmalige Anmelden aus der Cloud auf lokale Apps, ohne dass der Benutzer unterschiedliche Benutzernamen und Kennwörter eingeben muss. Hierzu gehören Organisationen mit folgenden Merkmalen:


- Intern mehrere Domänen umfassen (joe@us.contoso.com, joe@eu.contoso.com) und eine einzelne Domäne in der Cloud (joe@contoso.com).


- Intern nicht routingfähig Domänennamen haben (joe@contoso.usa) und eine rechtliche in der Cloud.


- Sie verwenden intern keine Domänennamen (Joe).


- Sie verwenden lokal und in der Cloud unterschiedliche Aliase. Beispiel: Joe-Johns@contoso.com im Vergleich zu joej@contoso.com
Dies hilft auch bei Anwendungen, die keine Adressen in Form von E-Mail-Adressen akzeptieren – einem häufigen Szenario bei nicht Windows-basierten Back-End-Servern.
### Festlegen von SSO für unterschiedliche lokale und cloudbasierte Identitäten
1. Konfigurieren Sie die Azure AD Connect-Einstellungen so, dass die E-Mail-Adresse die Hauptidentität ist. Dies erfolgt als Teil des Anpassungsvorgangs durch Änderung des Benutzerprinzipalnamens in den Synchronisierungseinstellungen.<br>![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)
HINWEIS: Diese Einstellungen bestimmen auch, wie sich Benutzer bei Office 365, Windows 10-Geräten und anderen Anwendungen anmelden, die Azure AD als Identitätsspeicher verwenden.
2. Die Konfiguration der Einstellungen für die Anwendung, die Sie ändern möchten, wählen Sie die **delegiert Anmelde-ID** verwendet werden:


- Benutzerprinzipalnamen: joe@contoso.com


- Alternative Benutzerprinzipalnamen: joed@contoso.local


- Benutzernamensteil des Benutzerprinzipalnamens: joe


- Benutzernamensteil des alternativen Benutzerprinzipalnamens: joed


- Lokaler SAM-Kontoname: je nach Konfiguration des lokalen Domänencontrollers
![](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

### Problembehandlung bei SSO für verschiedene Identitäten
Wenn ein Fehler, in der SSO-Prozess vorliegt wird es angezeigt im Ereignisprotokoll Connector-Computer wie in beschrieben [Problembehandlung](active-directory-application-proxy-troubleshoot.md).
In einigen Fällen wird die Anforderung jedoch erfolgreich an die Back-End-Anwendung gesendet, während die Anwendung mit verschiedenen anderen HTTP-Antworten reagiert. Die Problembehandlung beginnt in diesen Fällen zweckmäßigerweise mit der Untersuchung der Ereignisnummer 24029 auf dem Connectorcomputer im Sitzungsereignisprotokoll des Anwendungsproxys. Die Identität des Benutzers, die für die Delegierung verwendet wurde, wird im Feld "Benutzer" in den Ereignisdetails angezeigt (im folgenden Beispiel "joe@contoso55.com"). Wählen Sie zum Aktivieren des Sitzungsprotokolls **Analytische und Debugprotokolle einblenden** im Menü "Ansicht" der Ereignisanzeige aus.







## Weitere Informationen
Der Anwendungsproxy bietet Ihnen noch viele weitere Möglichkeiten:


- [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md)
- [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
- [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
- [Arbeiten mit Anwendungen, die Ansprüche unterstützen](active-directory-application-proxy-claims-aware-apps.md)
- [Beheben Sie Probleme, mit dem Anwendungsproxy aufgetreten ist](active-directory-application-proxy-troubleshoot.md)

## Weitere Informationen zum Anwendungsproxy
- [Onlinehilfe anzeigen](active-directory-application-proxy-enable.md)
- [Blog zum Anwendungsproxy aufrufen](http://blogs.technet.com/b/applicationproxyblog/)
- [Sehen Sie sich unsere Videos auf Channel 9 an!](http://channel9.msdn.com/events/Ignite/2015/BRK3864)



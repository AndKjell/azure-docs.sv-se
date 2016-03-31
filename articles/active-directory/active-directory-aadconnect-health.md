<properties
    pageTitle="Überwachen Sie Ihre lokalen Identitätsinfrastruktur in der Cloud."
    description="Auf der Seite "Azure AD Connect Health" wird der Dienst beschrieben, und es werden die Gründe für seine Verwendung erörtert."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/16/2015"
    ms.author="billmath"/>

# Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud

Azure AD Connect Health unterstützt Sie bei der Überwachung Ihrer lokalen Identitätsinfrastruktur und über Azure AD Connect verfügbarer Synchronisierungsdienste und vermittelt Ihnen wichtige Einblicke.  Sie können damit Warnungen, Leistungsdaten, Verwendungsmuster und Konfigurationseinstellungen anzeigen, eine zuverlässige Verbindung mit Office 365 aufrechterhalten und viele weitere Funktionen nutzen. Dies wird mithilfe eines Agents erreicht, der auf den Zielservern installiert ist.  

![Was ist Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)


Diese Informationen sind alle im Azure AD Connect Health-Portal aufgeführt.  Im Azure AD Connect Health-Portal können Sie Warnungen, Leistungsüberwachungsdaten und Nutzungsanalysen anzeigen.  Diese Informationen werden alle an einer zentralen Stelle präsentiert, damit Sie keine Zeit mit der Suche nach den Informationen verschwenden, die Sie benötigen.

![Was ist Azure AD Connect Health?](./media/active-directory-aadconnect-health/usage.png)

Künftige Updates für Azure AD Connect Health werden zusätzliche Überwachungsfunktionen und Einblicke in andere Identitätskomponenten umfassen. Entsprechend können Sie mithilfe eines zentralen Dashboards im Hinblick auf die Identität für eine stabilere, fehlerfreie und besser integrierte Umgebung sorgen, damit Ihre Benutzer ihre Aufgaben schneller erledigen können.


<center>![Was ist Azure AD Connect Health?](./media/active-directory-aadconnect-health/logo1.png)</center>




## Gründe für die Verwendung von Azure AD Connect Health

Die Integration Ihrer lokalen Verzeichnisse in Azure AD steigert die Produktivität Ihrer Benutzer, da für den Zugriff auf die Cloud und lokale Ressourcen nur eine Identität benötigt wird. Allerdings erfordert diese Integration, dass diese Umgebung fehlerfrei bleibt, sodass Benutzer von jedem Gerät verlässlich auf lokale ebenso wie auf Cloudressourcen zugreifen können. Azure AD Connect Health bietet einen einfachen cloudbasierten Ansatz zum Überwachen und Gewinnen von Einblicken in Ihre lokale Identitätsverwaltungsinfrastruktur, die für den Zugriff auf Office 365 oder andere Azure AD-Anwendungen genutzt wird. Dieser Ansatz ist ähnlich unkompliziert wie das Installieren eines Agents auf Ihren lokalen Identitätsservern. 

Azure AD Connect Health für AD FS unterstützt AD FS 2.0 unter Windows Server 2008/2008 R2 und AD FS unter Windows Server 2012/2012R2. Dies umfasst auch alle AD FS-Proxy- oder Webanwendungsproxy-Server, die die Authentifizierung für den Extranetzugriff unterstützen. Azure AD Connect Health für AD FS stellt die folgenden wichtigen Funktionen bereit:

- Anzeigen von Warnungen und Einleiten entsprechender Gegenmaßnahmen, um den zuverlässigen Zugriff auf AD FS-geschützte Anwendungen wie Azure AD zu ermöglichen
- E-Mail-Benachrichtigungen für kritische Warnungen
- Anzeigen von Leistungsdaten für die Kapazitätsplanung
- Detaillierte Ansichten Ihrer AD FS-Anmeldemuster, um Abweichungen zu ermitteln und Baselines für die Kapazitätsplanung festzulegen


Azure AD Connect Health für die Synchronisierung überwacht die Synchronisierungen zwischen Ihrem lokalen Active Directory und Azure Active Directory. Azure AD Connect Health für die Synchronisierung stellt die folgenden wichtigen Funktionen bereit:

- Anzeigen und Reagieren auf Warnungen, um zuverlässige Synchronisierungen zwischen der lokalen Infrastruktur und Azure Active Directory sicherzustellen.
- E-Mail-Benachrichtigungen für kritische Warnungen
- Anzeigen von Leistungsdaten

Das folgende Video bietet eine Übersicht über Azure AD Connect Health:

[AZURE.VIDEO azure-ad-connect-health--monitor-you-identity-bridge]



## Erste Schritte im Azure-Portal
Führen Sie zum Einstieg in Azure Active Directory Connect Health die folgenden Schritte aus. 

1. Melden Sie sich bei der [Microsoft Azure-Portal.](https://portal.azure.com/)
2. Sie greifen auf Azure Active Directory Connect Health zu, indem Sie zum Marketplace wechseln und dort danach suchen, oder Sie wechseln zum Marketplace und wählen dort "Sicherheit + Identität" aus.
3. Klicken Sie im einführenden Blatt (ein Blatt ist ein Element der Gesamtansicht, Sie können ein Blatt als Fenster vorstellen oder nach außen), klicken Sie auf **Erstellen**. Es wird ein weiteres Blatt mit Informationen zu Ihrem Verzeichnis geöffnet.
4. Klicken Sie auf das auf dem verzeichnisblatt auf **Erstellen**. Zur Verwendung von Azure AD Connect Health benötigen Sie eine Azure Active Directory Premium-Lizenz. Informationen zu Azure AD Premium finden Sie unter "Erste Schritte mit Azure AD Premium".

>[AZURE.NOTE]Denken Sie daran, dass Sie keine Daten in Ihrer Instanz von Azure AD Connect Health sehen, Sie um die Azure AD Connect Health-Agents auf Ihren Zielservern installieren müssen. Um den Azure AD Connect Health-Agent herunterzuladen, wählen Sie im ersten Blatt "Schnellstart und Tools herunterladen". Sie können auch den Agent direkt mit der [Link](#download-the-agent) unten.  Gehen Sie zur Verwendung von Azure Active Directory Connect Health folgendermaßen vor:



### Azure AD Connect Health-Portal und -Dienste
Im Azure AD Connect Health-Portal können Sie Warnungen, Leistungsüberwachungsdaten und Nutzungsanalysen anzeigen. Beim ersten Zugriff auf Azure AD Connect Health wird das erste Blatt angezeigt.  Sie können es sich wie ein Fenster vorstellen. Das zuerst angezeigte Blatt zeigt Schnellstart, Dienste und Konfiguration. Der nachstehende Screenshot zeigt jedes dieser Elemente, die nachfolgend kurz beschrieben werden.  Dieser Abschnitt zeigt die aktiven Dienste und Instanzen von Diensten, die von Azure AD Connect Health überwacht werden. 

![Azure AD Connect Health-Portal](./media/active-directory-aadconnect-health/portal2.png)

- **Schnellstart** – durch Auswahl dieser Option werden Sie das Blatt "Schnellstart" öffnen. Hier können Sie den Azure AD Connect Health-Agent herunterladen, indem Sie auf "Tools abrufen" klicken, Sie können auf die Dokumentation zugreifen und Feedback geben.
- **Active Directory Federation Services** – stellt alle AD FS-Dienste, die derzeit von Azure AD Connect Health überwacht werden. Durch Auswahl einer der Instanzen wird ein Blatt mit Informationen zu dieser Dienstinstanz geöffnet.  darunter beispielsweise eine Übersicht, Eigenschaften, Warnungen, Überwachungsinformationen und eine Nutzungsanalyse. 
- "Konfigurieren" – Ermöglicht das Aktivieren oder Deaktivieren der folgenden Optionen:
<ol>
1. Automatische Aktualisierung des Azure AD Connect Health-Agents auf die aktuelle Version – Dies bedeutet, dass eine automatische Aktualisierung auf die aktuelle Version des Azure AD Connect Health-Agents durchgeführt wird, sobald diese verfügbar ist. Diese Einstellung ist standardmäßig aktiviert.
2. Erlauben Sie Microsoft, zu Zwecken der Problembehandlung den Zugriff auf die Integritätsdaten für Ihr Azure AD-Verzeichnis – Wenn Sie diese Einstellung aktivieren, kann Microsoft dieselben Daten anzeigen wie Sie. Dies kann bei der Problembehandlung und zur Unterstützung bei der Fehlerbeseitigung von Nutzen sein. Diese Einstellung ist standardmäßig deaktiviert.




## Anforderungen
Nachfolgend wird eine Liste der Voraussetzungen bereitgestellt, die vor der Verwendung von Azure AD Connect Health erfüllt werden müssen.

| Voraussetzung | Beschreibung|
| ----------- | ---------- |
|Azure AD Premium| Azure AD Connect Health ist ein Azure AD Premium-Feature und erfordert Azure AD Premium. </br></br>Weitere Informationen finden Sie unter [Erste Schritte mit Azure AD Premium](active-directory-get-started-premium.md).</br></br>Starten Sie eine kostenlose 30-tägige Testversion finden Sie unter [eine Testversion zu starten.](https://azure.microsoft.com/trial/get-started-active-directory/)|.
|Sie müssen ein globaler Administrator Ihres Azure AD sein, um Azure AD Connect Health aktivieren (erstellen) zu können|Standardmäßig können nur globale Administratoren aktivieren (erstellen), auf alle Informationen zugreifen und alle Vorgänge in Azure AD Connect Health durchführen. Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](active-directory-administer.md). <br><br> Verwendung von rollenbasierten Zugriffsteuerung ermöglicht Zugriff auf Azure AD Connect Health für andere Benutzer in Ihrer Organisation. Weitere Informationen finden Sie unter [rollenbasierte Zugriffssteuerung für Azure AD Connect Health.](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) </br></br>**Wichtig:** bei der Installation der Agents verwendete Konto muss eine Arbeit oder Organisations-Konto und ein Microsoft-Konto nicht möglich. Weitere Informationen finden Sie unter [für Azure als Organisation registrieren](sign-up-organization.md)|
|Aktivieren der AD FS-Überwachung zum Verwenden der Nutzungsanalyse| Wenn Sie eine Verwendungsanalyse mit AD FS ausführen möchten, muss die AD FS-Überwachung aktiviert werden.  </br></br>Finden Sie unter [Installieren von Azure AD Connect Health-Agent für AD FS.](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)
|Erfüllen der Anforderungen für den Azure AD Connect Health-Agenten|Die folgende Tabelle enthält agentenspezifische Anforderungen.

Nachfolgend wird eine Liste der Voraussetzungen für den Agenten bereitgestellt, die vor der Verwendung von Azure AD Connect Health erfüllt werden müssen.

| Voraussetzung | Beschreibung|
| ----------- | ---------- |
|Der Azure AD Connect Health-Agent ist auf jedem Zielserver installiert| Azure AD Connect Health erfordert, dass ein Agent auf den Zielservern installiert wird, um die im Portal angezeigten Daten bereitzustellen. </br></br>Um Daten auf einem lokalen AD FS-Infrastruktur zu erhalten, muss der Agent beispielsweise auf den AD FS-Servern installiert.  Dies schließt die AD FS-Proxyserver und Webanwendungsproxyserver ein.   </br></br>Informationen zum Installieren des Agents finden Sie unter der [Azure AD Connect Health-Agent-Installation](active-directory-aadconnect-health-agent-install.md).</br></br>**Wichtig:** bei der Installation der Agents verwendete Konto muss eine Arbeit oder Organisations-Konto und ein Microsoft-Konto nicht möglich.   Weitere Informationen finden Sie unter [für Azure als Organisation registrieren](sign-up-organization.md)|
|Azure AD Connect Health-Agent für die Synchronisierung| Dieser Agent wird automatisch mit der neuesten Version von Azure AD Connect installiert.  </br></br>Wenn Sie nur starten müssen Sie keine nichts weiter tun.  Der Agent wird bei der Installation von Azure AD Connect installiert.</br></br> Wenn Sie bereits Azure AD Connect installiert haben, müssen Sie auf die neueste Version zu aktualisieren, die heruntergeladen werden kann [hier](http://www.microsoft.com/download/details.aspx?id=47594).
|Ausgehende Verbindungen zu den Azure-Dienstendpunkten|Während der Installation und der Laufzeit erfordert der Agent Verbindungen mit den nachfolgend aufgeführten Endpunkten des Azure AD Connect Health-Diensts. Wenn Sie ausgehende Verbindungen blockieren, stellen Sie sicher, dass folgende Einträge zur Zulassungsliste hinzugefügt werden: </br></br><li>**neue**: https://management.azure.com </li><li>**neue**: & #42;. können </li><li>**neue**: & #42;. Queue.Core.Windows.NET</li><li>& #42;. *.Servicebus.Windows.NET - Port: 5671</li><li>https://&#42;.adhybridhealth.Azure.com/</li><li>https://&#42;.Table.Core.Windows.NET/</li><li>https://policykeyservice.DC.AD.msft.NET/</li><li>https://Login.Windows.NET</li><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li> |
|Firewall-Ports auf dem Server, auf dem der Agent ausgeführt wird.| Der Agent benötigt die folgenden Firewallports in der Reihenfolge für den Agent mit den Azure AD Health-Dienstendpunkten kommunizieren geöffnet sein.</br></br><li>TCP/UDP-Port 80</li><li>TCP/UDP-Port 443</li><li>TCP/UDP-Port 5671</li>
|Zulassen der folgenden Websites, wenn die verstärkte IE-Sicherheit aktiviert ist|Die folgenden Websites müssen zugelassen werden, wenn die verstärkte Sicherheitskonfiguration für Internet Explorer auf dem Server aktiviert ist, auf dem der Agent installiert wird.</br></br><li>https://Login.microsoftonline.com</li><li>https://Secure.aadcdn.microsoftonline-p.com</li><li>https://Login.Windows.NET</li><li>Der Verbundserver für Ihre Organisation von Azure Active Directory beispielsweise vertraut: https://sts.contoso.com</li>

## Herunterladen des Agents

Führen Sie zu Beginn einen der folgenden Schritte aus:


- Erste Schritte mit Azure AD Connect Health für AD FS können Sie die neueste Version des Agents hier herunterladen:  [Herunterladen von Azure AD Connect Health-Agent für AD FS.](http://go.microsoft.com/fwlink/?LinkID=518973) Stellen Sie vor der Installation der Agents sicher, dass Sie den Dienst aus dem Marketplace hinzugefügt haben.
- Für die ersten Schritte mit Azure AD Connect Health für die Synchronisierung laden Sie die neueste Version von Azure AD Connect herunter, und installieren Sie sie.  Der Health-Agent wird im Rahmen der Installation von Azure AD Connect installiert.  Azure AD Connect unterstützt ein direktes Upgrade von vorherigen Versionen.


## Verwandte Links

* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health – FAQ](active-directory-aadconnect-health-faq.md)



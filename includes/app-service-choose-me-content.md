<a name="tellmeas"></a>
## Weitere Informationen über App Service

Azure Virtual Machines kann eine breite Palette von Cloudhostingaufgaben verarbeiten. Aber für das Erstellen und Verwalten einer VM-Infrastruktur sind spezielle Kenntnisse und beträchtlicher Aufwand erforderlich. Wenn Sie Kontrolle über die virtuellen Computer müssen nicht, die Web-apps, mobile app-Back-Ends, API-apps usw. ausgeführt vollständige, es ist eine einfachere (und günstigere) Lösung: *Plattform als Dienst* (PaaS). Bei PaaS übernimmt Azure die meisten Verwaltungsaufgaben für die VMs, auf denen Ihre Anwendungen ausgeführt werden. [Azure App Service](../article/app-service/app-service-value-prop-what-is.md) ist ein vollständig verwaltetes PaaS-Angebot, das Sie erstellen, kann bereitstellen und Skalieren der Unternehmensklasse-apps in Sekunden.

App Service ist die beste Wahl für viele Arten von Anwendungsworkloads. Ein Unternehmen könnte eine kommerzielle Website erstellen oder migrieren, die Millionen von Aufrufen pro Woche verarbeiten kann und in verschiedenen Rechenzentren weltweit bereitgestellt ist. Dasselbe Unternehmen verfügt möglicherweise auch über eine Branchen-App, die Spesenabrechnungen für authentifizierte Benutzer über Active Directory im Unternehmen verfolgt, und die App könnte eine Komponente für mobile Geräte aufweisen und Verbindungen mit lokalen Ressourcen und Geschäftsprozessen herstellen. Für die Spesenabrechnungen sind möglicherweise regelmäßige Hintergrundaufträge erforderlich, um große Datenmengen zu berechnen und zusammenzufassen. Ein IT-Berater könnte eine gängige Open Source-Anwendung anpassen, um ein Content Management-System für ein kleines Unternehmen zu erstellen. Die folgende Abbildung zeigt einige Arten von Web-Apps, die in Azure App Service ausgeführt werden können.

<a name="appservice_diagram"></a>
![App Service-Diagramm](media/app-service-choose-me-content/diagram.png)
 
**Abbildung: Azure App Service unterstützt statische Webseiten, gängige Webanwendungen und spezielle Webanwendungen mit verschiedenen Technologien. Sie können auch mobile Back-Ends, API-app und webunabhängige computeworkloads (mit WebJobs) ausführen.** 

Mit Azure App Service können Sie auch beliebige Compute Arbeitslast mit Ausführen der [Webaufträge](../article/app-service-web/websites-webjobs-resources.md) Funktion. 

Azure App Service kann auf freigegebenen VMs ausgeführt werden, die mehrere Apps enthalten, die von mehreren Benutzern erstellt wurden, oder auf VMs, die nur von Ihnen verwendet werden. Die VMs sind Teil eines von Azure App Service verwalteten Ressourcenpools und bieten daher hohe Zuverlässigkeit und Fehlertoleranz.

Der Anfang ist denkbar einfach. Mit Azure App Service können Benutzer aus einer breiten Palette an Anwendungen, Frameworks und Vorlagen auswählen und innerhalb von Sekunden eine Web-App erstellen. Anschließend können sie ihre bevorzugten Entwicklungstools (WebMatrix, Visual Studio oder andere Editoren) und Quellcodeverwaltungsoptionen verwenden, um die fortlaufende Integration einzurichten und im Team zu entwickeln. Anwendungen mit MySQL-Datenbanken können einen MySQL-Service konsumieren, der für Azure von dem Microsoft-Partner ClearDB angeboten wird.

Entwickler können mit Azure App Service große und skalierbare Webanwendungen erstellen. Die Technologie unterstützt die Erstellung von Anwendungen in ASP.NET, PHP, Node.js und Python. Anwendungen können z. B. persistente Sitzungen verwenden, und vorhandene Web-Apps können ohne Änderungen in diese Cloudplattform verschoben werden. In Azure App Service erstellte Web-Apps können jederzeit andere Aspekte von Azure verwenden, z. B. Service Bus, SQL-Datenbank und Blobspeicher. Außerdem können Sie mehrere Kopien einer Anwendung in unterschiedlichen VMs ausführen, und Azure App Service führt einen automatischen Lastenausgleich zwischen diesen VMs durch. Und da Web-App-Instanzen in bereits vorhandenen VMs erstellt werden, erfolgt der Start neuer Anwendungsinstanzen sehr schnell im Vergleich zur Wartezeit bei der Erstellung neuer VMs.

Als die [Abbildung](#appservice_diagram) oben zeigt, Sie können Code und andere Webinhalte in Azure App Service auf verschiedene Arten veröffentlichen. Sie können FTP, FTPS oder die WebDeploy-Technologie von Microsoft verwenden. Azure App Service unterstützt außerdem die Veröffentlichung von Code aus Quellcodeverwaltungssystemen, darunter Git, GitHub, CodePlex, BitBucket, Dropbox, Mercurial, Team Foundation Server und dem cloudbasierten Team Foundation Service.



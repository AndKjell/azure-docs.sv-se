Ressource|Standardlimit|Maximales Limit
---|---|---
Azure Media Services (AMS)-Konten in einem einzelnen Abonnement||25
Objekte pro AMS-Konto||1.000.000<sup>1</sup>
Verkettete Aufgaben pro Auftrag||30
Objekte pro Aufgabe||50
Objekte pro Auftrag||100
Aufträge pro AMS-Konto ||50.000<sup>2</sup>
Eindeutige Locators, die einem Objekt gleichzeitig zugeordnet sind||5<sup>4</sup>
Livekanäle pro AMS-Konto </p></td>|5</p></td>|–<sup>1</sup>
Programme im angehaltenen Zustand pro Kanal </p></td>|50</p></td>|–<sup>1</sup>
Programme im ausgeführten Zustand pro Kanal </p></td>|3</p></td>|3
Streamingendpunkte im ausgeführten Zustand pro AMS-Konto</p></td>|2</p></td>|–<sup>1</sup>
Streamingeinheiten pro Streamingendpunkt </p></td>|10 </p></td>|–<sup>1</sup>
Codierungseinheiten pro AMS-Konto </p></td>|25</p></td>|–<sup>1</sup>
Speicherkonten | |1.000<sup>5</sup>

<sup>1</sup> Sie können die Aktualisierung der Grenzwerte für dieses Kontingent anfordern, indem Sie ein Supportticket öffnen. Erstellen Sie keine weiteren AMS-Konten, um die Grenzwerte zu erhöhen, sondern senden Sie stattdessen ein Supportticket.

<sup>2</sup> Diese Zahl umfasst fertig gestellte, aktive und abgebrochene Aufträge sowie Aufträge in der Warteschlange. Gelöschte Aufträge sind nicht enthalten. Sie können die alten Aufträge mithilfe von löschen **IJob.Delete** oder **Löschen** HTTP-Anforderung.

<sup>3</sup> Wenn Sie eine Anforderung zum Auflisten von Auftragsentitäten senden, werden maximal 1.000 Entitäten pro Anforderung zurückgegeben. Wenn Sie alle gesendeten Aufträge Überblick behalten müssen, können Sie oben "/" Überspringen wie in beschrieben [OData-systemabfrageoptionen](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locators sind nicht für die Verwaltung der Zugriffssteuerung pro Benutzer konzipiert. Um einzelnen Benutzern verschiedene Zugriffsrechte zu erteilen, verwenden Sie Lösungen zur Verwaltung digitaler Rechte (Digital Rights Management, DRM).

<sup>5</sup> Die Speicherkonten müssen aus demselben Azure-Abonnement stammen.



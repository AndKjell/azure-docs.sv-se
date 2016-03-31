## Was ist der Azure-Dateispeicher?

Der Dateispeicher bietet einen gemeinsam genutzten Speicher für Anwendungen und verwendet dabei das SMB 2.1- oder SMB 3.0-Protokoll. Virtuelle Microsoft Azure-Computer und Clouddienste können Dateidaten in verschiedenen Anwendungskomponenten über eingebundene Freigaben teilen, und lokale Anwendungen können über die Dateispeicher-API auf freigegebene Dateien zugreifen.

Anwendungen in virtuellen Azure-Computern oder Clouddiensten können eine Dateispeicher-Freigabe einbinden, um auf Dateidaten zuzugreifen, ebenso wie eine Desktopanwendung eine typische SMB-Freigabe einbinden würde. Die Dateispeicherfreigabe kann von einer beliebigen Anzahl von virtuellen Azure-Computern oder -Rollen gleichzeitig bereitgestellt und verwendet werden.

Da eine Dateispeicherfreigabe eine standardmäßige Dateifreigabe in Azure mit dem SMB-Protokoll ist, können in Azure ausgeführte Anwendungen über Datei-E/A-APIs auf Daten der Freigabe zugreifen. Entwickler können daher ihren vorhandenen Code und bereits erlernte Fertigkeiten für die Migration vorhandener Anwendungen verwenden. IT-Fachkräfte können PowerShell-Cmdlets verwenden, um Dateispeicher-Freigaben im Rahmen der Administration von Azure-Anwendungen zu erstellen, einzubinden und zu verwalten. Diese Anleitung zeigt jeweils Beispiele dazu.

Dateispeicher werden hauptsächlich für folgende Zwecke verwendet:

- Migration von lokalen Anwendungen, die Dateifreigaben aus virtuellen Azure-Computern oder Clouddiensten ohne teure Umschreibungen verwenden
- Speichern von gemeinsam genutzten Anwendungseinstellungen, z. B. in Konfigurationsdateien
- Speichern von Diagnosedaten wie Protokolle, Metriken und Absturzabbilder an einem freigegebenen Speicherort. 
- Speichern von Tools und Dienstprogrammen für das Entwickeln und Verwalten von virtuellen Azure-Computern oder Clouddiensten

## Dateispeicherkonzepte

Der Dateispeicher umfasst die folgenden Komponenten:

![files-concepts][files-concepts]

-   **Speicherkonto:** alle Zugriffe auf den Azure-Speicher erfolgen
    über ein Speicherkonto. Finden Sie unter [Azure Storage-Skalierbarkeit und Leistungsziele](http://msdn.microsoft.com/library/azure/dn249410.aspx) Details zur speicherkontokapazität.

-   **Freigabe:** eine Dateispeicher-Freigabe ist eine SMB-Dateifreigabe in Azure. 
    Alle Verzeichnisse und Dateien müssen in der übergeordneten Freigabe erstellt werden. Ein Konto kann eine
    unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine
    Anzahl der Dateien, die 5 TB Gesamtkapazität der Dateifreigabe.

-   **Verzeichnis:** eine optionale Hierarchie von Verzeichnissen. 

-   **Datei:** einer Datei in der Freigabe. Die Datei kann bis zu 1 TB groß sein.

-   **URL-Format:** Dateien sind über die folgende URL adressierbar
    -Format adressierbar:   
    https://`<storage
    account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
    
    Mit der folgenden Beispiel-URL kann z. B. eine der Dateien im Diagramm oben
    adressiert werden:  
    `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

Weitere Informationen zur Benennung von Freigaben, Verzeichnisse und Dateien, finden Sie unter [benennen und referenzieren von Freigaben, Verzeichnisse, Dateien und Metadaten](http://msdn.microsoft.com/library/azure/dn167011.aspx).

[files-concepts]: ./media/storage-file-concepts-include/files-concepts.png


Azure bestimmt, dass Ihre Anwendung Python verwendet **Wenn beide Bedingungen erfüllt sind**:

- requirements.txt-Datei im Stammordner
- eine .py-Datei im Stammordner ODER eine runtime.txt-Datei, die Python angibt

Wenn dies der Fall ist, wird ein Python-Bereitstellungsskript verwendet, das die standardmäßige Synchronisierung von Dateien sowie zusätzliche Python-Vorgänge ausführt, beispielsweise:

- Automatische Verwaltung der virtuellen Umgebung
- Installation von in requirements.txt aufgelisteten Paketen mit Pip
- Die Erstellung der entsprechenden web.config basierend auf der ausgewählten Python-Version
- Sammeln von statischen Dateien für Django-Anwendungen

Sie können bestimmte Aspekte der Standardbereitstellungsschritte steuern, ohne das Skript anzupassen.

Wenn Sie alle Python-spezifischen Bereitstellungsschritte überspringen möchten, können Sie diese leere Datei erstellen:

    \.skipPythonDeployment

Wenn Sie die Sammlung statischer Dateien für die Django-Anwendung zu überspringen möchten:

    \.skipDjango 

Für mehr Kontrolle über die Bereitstellung können Sie das Standardskript für die Bereitstellung überschreiben, indem Sie die folgenden Dateien erstellen:

    \.deployment
    \deploy.cmd

Sie können die [Azure-Befehlszeilenschnittstelle][] Dateien erstellt.  Verwenden Sie diesen Befehl vom Projektordner aus:

    azure site deploymentscript --python

Wenn diese Dateien nicht vorhanden sind, wird von Azure ein temporäres Bereitstellungsskript erstellt und ausgeführt.  Es ist identisch mit dem, das Sie mit dem oben aufgeführten Befehl erstellen.

[Azure command-line interface]: http://azure.microsoft.com/downloads/



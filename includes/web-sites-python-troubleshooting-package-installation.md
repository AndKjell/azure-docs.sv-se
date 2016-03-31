Einige Pakete möglicherweise bei der Ausführung auf Azure bei Verwendung von Pip nicht installiert.  Es kann einfach sein, dass das Paket nicht im Python-Paketindex verfügbar ist.  Möglicherweise ist ein Compiler erforderlich (ein Compiler ist auf dem Computer, auf dem die Web-App in Azure App Service ausgeführt wird, nicht verfügbar).

In diesem Abschnitt werden Methoden zur Behandlung dieses Problems vorgestellt.

### Anfordern von Wheels

Wenn die Installation des Pakets einen Compiler erfordert, sollten Sie sich an den Besitzer des Pakets wenden, um anzufordern, dass die Wheels für das Paket zur Verfügung gestellt werden.

Mit der aktuellen Verfügbarkeit von [Microsoft Visual C++-Compiler für Python 2.7][], es ist jetzt einfacher Pakete erstellen, die systemeigenen Code für Python 2.7 enthalten.

### Erstellen von Wheels (erfordert Windows)

Hinweis: Achten Sie bei Verwendung dieser Option darauf, dass Sie das Paket mit einer Python-Umgebung kompilieren, die mit der Plattform/Architektur/Version übereinstimmt, die auf der Web-App in Azure App Service verwendet wird (Windows/32-Bit/2.7 oder 3.4).

Wenn das Paket nicht installiert wird, weil es einen Compiler erfordert, können Sie den Compiler auf Ihrem lokalen Computer installieren und ein Wheel für das Paket erstellen, das dann in Ihrem Repository enthalten ist.

Mac/Linux-Benutzer: Wenn Sie den Zugriff auf einen Windows-Computer haben, finden Sie unter [Erstellen eines virtuellen Computers unter Windows][] zum Erstellen eines virtuellen Computers in Azure.  Sie können damit Wheels erstellen, diese zum Repository hinzufügen und den virtuellen Computer ggf. verwerfen. 

Sie können für Python 2.7 installieren [Microsoft Visual C++-Compiler für Python 2.7][].

Sie können für Python 3.4 installieren [Microsoft Visual C++ 2010 Express][].

Um die Wheels zu erstellen, benötigen Sie das Wheel-Paket:

    env\scripts\pip install wheel

Verwenden Sie `pip wheel`, um eine Abhängigkeit zu kompilieren:

    env\scripts\pip wheel azure==0.8.4

Dies erstellt eine .whl-Datei im Ordner \wheelhouse.  Fügen Sie den \wheelhouse-Ordner und die Wheel-Dateien zum Repository hinzu.

Bearbeiten Sie die Datei "requirements.txt", und fügen Sie die Option `--find-links` am Anfang ein. Dies weist Pip an, nach einer genauen Übereinstimmung im lokalen Ordner zu suchen, bevor es zum Python-Paketindex wechselt.

    --find-links wheelhouse
    azure==0.8.4

Wenn alle Ihre Abhängigkeiten im \wheelhouse-Ordner enthalten sein sollen und Sie keinen Python-Paketindex verwenden möchten, können Sie erzwingen, dass Pip den Paketindex ignoriert, indem `--no-index` am Anfang von "requirements.txt" einfügen.

    --no-index

### Anpassen der Installation

Sie können das Bereitstellungsskript zum Installieren eines Pakets in der virtuellen Umgebung mithilfe eines alternativen Installationsprogramms anpassen, z. B. easy\_install.  Ein auskommentiertes Beispiel finden Sie unter "deploy.cmd".  Stellen Sie sicher, dass solche Pakete nicht in requirements.txt aufgelistet sind, um zu verhindern, dass Pip sie installiert.

Fügen Sie Folgendes zum Bereitstellungsskript hinzu:

    env\scripts\easy_install somepackage

Sie können möglicherweise auch easy\_install verwenden, um mithilfe eines EXE-Installers zu installieren (einige sind ZIP-kompatibel, sodass Easy\_install unterstützt).  Fügen Sie das Installationsprogramm zum Repository hinzu und rufen Sie easy\_install auf, indem Sie den Pfad zur ausführbaren Datei übergeben.

Fügen Sie Folgendes zum Bereitstellungsskript hinzu:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### Aufnehmen der virtuellen Umgebung in das Repository (erfordert Windows)

Hinweis: Stellen Sie bei Verwendung dieser Option sicher, dass Sie eine virtuelle Umgebung verwenden, die mit der Plattform/Architektur/Version übereinstimmt, die auf der Web-App in Azure App Service verwendet wird (Windows/32-Bit/2.7 oder 3.4).

Wenn Sie die virtuelle Umgebung in das Repository einschließen, können Sie verhindern, dass das Bereitstellungsskript die Verwaltung der virtuellen Umgebung auf Azure ausführt, indem Sie eine leere Datei erstellen:

    .skipPythonDeployment

Es wird empfohlen, dass Sie die vorhandene virtuelle Umgebung auf der App löschen, um übrig gebliebene Dateien aus der Zeit zu verhindern, als die virtuelle Umgebung automatisch verwaltet wurde.


[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949



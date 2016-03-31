## Grundlegendes zu Azure-Ressourcenvorlagen und -Ressourcengruppen

Die meisten Anwendungen, die in Microsoft Azure bereitgestellt und ausgeführt werden, sind aus einer Kombination aus verschiedenen Cloud-Ressourcentypen (z. B. als eine oder mehrere VMs und Speicherkonten, SQL-Datenbank, virtuelles Netzwerk, ein CDN usw.) erstellt.  [Resource Manager-Vorlagen](../resource-group-authoring-templates.md) ermöglichen es Ihnen, bereitstellen und verwalten die unterschiedlichen Ressourcen zusammen mit einer JSON-Beschreibung der Ressourcen sowie der zugeordneten Konfigurations- und Bereitstellungsparameter.

Sobald Sie eine JSON-basierte Ressourcenvorlage definiert haben, können Sie sie ausführen und die darin definierten Ressourcen in Azure mithilfe eines PowerShell-Befehls bereitstellen.  Sie können diesen Befehl entweder eigenständig in der PowerShell-Befehlsshell oder in einem PowerShell-Skript ausführen, das eine zusätzliche Automatisierungslogik enthält.

Die Ressourcen, die Sie mithilfe von Azure Resource Manager-Vorlagen erstellen, werden entweder in einer neuen oder einer vorhandenen Azure-Ressourcengruppe bereitgestellt.  Mithilfe einer Azure-Ressourcengruppe können Sie mehrere bereitgestellte Ressourcen zusammen als eine logische Gruppe verwalten. Eine Gruppe enthält in der Regel Ressourcen, die zu einer bestimmten Anwendung gehören.  Azure-Ressourcengruppen bieten die Möglichkeit, den gesamten Lebenszyklus der Anwendung/Gruppe zu verwalten. Verwaltungs-APIs ermöglichen das Beenden/Starten/Löschen aller in der Gruppe vorhandenen Ressourcen auf einmal. Sie können Role-Based Access Control (RBAC)-Regeln zum Sperren von Sicherheitsberechtigungen anwenden, den Betrieb überwachen oder Ressourcen zur besseren Verfolgung mit zusätzlichen Metadaten kennzeichnen. Weitere Informationen zum Azure-Ressourcengruppen finden Sie unter der [Übersicht über Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-overview/). 

Die folgenden Automatisierungsbeispiele veranschaulichen die Anwendung von Azure Resource Manager-Vorlagen sowie die Bereitstellung von Ressourcengruppen via PowerShell oder CLI.



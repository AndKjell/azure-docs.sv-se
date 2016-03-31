## Erstellen eines Sicherungstresors
Zum Sichern von Dateien und Daten von Windows Server oder Data Protection Manager (DPM) in Azure oder beim Sichern von IaaS-VMs in Azure müssen Sie einen Sicherungstresor in der geografischen Region erstellen, in der die Daten gespeichert werden sollen.

Mit den folgenden Schritten erstellen Sie den zum Speichern von Sicherungen verwendeten Tresor.

1. Melden Sie sich bei der [-Verwaltungsportal](https://manage.windowsazure.com/)
2. Klicken Sie auf **Neu** > **Data Services** > **Recovery Services** > **Sicherungstresor** und wählen Sie **Schnellerfassung**.

    ![Tresor erstellen](./media/backup-create-vault/createvault1.png)

3. Für den **Namen** Parameter, geben Sie einen benutzerfreundlichen Namen zur Identifizierung des sicherungstresors. Dieser muss für jedes Abonnement eindeutig sein.

4. Für die **Region** Parameter, wählen Sie die geografische Region für den sicherungstresor. Die Auswahl bestimmt die geografische Region, an die Ihre Sicherungsdaten gesendet werden. Wenn Sie eine geografische Region in der Nähe Ihres eigenen Standorts auswählen, können Sie dadurch die Netzwerklatenz beim Sichern in Azure reduzieren.

5. Klicken Sie auf **Tresor erstellen** um den Workflow abzuschließen. Es kann eine Weile dauern, bis der Sicherungstresor fertiggestellt wird. Sie können die Benachrichtigungen unten im Portal überwachen, um den Status zu überprüfen.

    ![Erstellen eines Tresors](./media/backup-create-vault/creatingvault1.png)

6. Nach dem erfolgreichen Erstellen des Sicherungstresors wird dies in einer entsprechenden Meldung angezeigt. Tresor Recovery Services als auch in den Ressourcen aufgeführt ist **Active**.

    ![Status zum Erstellen eines Tresors](./media/backup-create-vault/backupvaultstatus1.png)


### Azure Backup – Speicherredundanzoptionen

> Der beste Zeitpunkt zum Bestimmen Ihrer Speicherredundanzoptionen ist unmittelbar nach Erstellung des Tresors, bevor Computer beim Tresor registriert werden. Sobald ein Element beim Tresor registriert wurde, wird die Speicherredundanzoption gesperrt und kann nicht mehr geändert werden.

Die Speicherredundanz des Back-End-Speichers von Azure Backup wird durch Ihre geschäftlichen Anforderungen bestimmt. Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden (wenn Sie beispielsweise von einem Windows Server in Azure sichern), sollten Sie die Option "Georedundanter Speicher" in Betracht ziehen (Standardeinstellung). Dies tritt unter den **konfigurieren** Option der Backup-Tresor.

![GRS](./media/backup-create-vault/grs.png)

#### Georedundanter Speicher (GRS)
GRS bewahrt sechs Kopien Ihrer Daten auf. Mit GRS werden Ihre Daten dreimal innerhalb der primären Region und dreimal in einer sekundären Region Hunderte von Kilometern von der primären Region entfernt repliziert, wodurch höchste Beständigkeit erreicht wird. Im Fall eines Fehlers in der primären Region stellt Azure Backup durch das Speichern von Daten in GRS sicher, dass Ihre Daten dauerhaft in zwei verschiedenen Regionen aufbewahrt werden.

#### Lokal redundanter Speicher (LRS)
Lokal redundanter Speicher (LRS) verwaltet drei Kopien Ihrer Daten. LRS wird innerhalb eines einzelnen Standorts dreimal in einer einzelnen Region repliziert. LRS schützt Ihre Daten vor normalen Hardwareausfällen, jedoch nicht vor dem Ausfall einer ganzen Azure-Einrichtung.

Bei Verwendung von Azure als tertiären Endpunkt (z. B. sind Sie SCDPM verwenden, um eine lokale Sicherung kopieren und mithilfe von Azure für Ihre langfristigen aufbewahrungsanforderungen), sollten Sie lokal redundanter Speicher auswählen die **konfigurieren** Option der Backup-Tresor. Dadurch werden die Kosten zum Speichern von Daten in Azure gesenkt, und es wird eine geringere Dauerhaftigkeit für Ihre Daten bereitgestellt, die möglicherweise für tertiäre Kopien ausreicht.

![LRS](./media/backup-create-vault/lrs.png)



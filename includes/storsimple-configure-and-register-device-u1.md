<!--author=alkohli last changed: 12/01/15-->


### So konfigurieren und registrieren Sie das Gerät

1. Greifen Sie an der seriellen Konsole Ihres StorSimple-Geräts auf die Windows PowerShell-Benutzeroberfläche zu. Finden Sie unter [Verwenden von PuTTY zum Herstellen der seriellen Konsole des Geräts](#use-putty-to-connect-to-the-device-serial-console) Anweisungen. **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen. Andernfalls sind Sie nicht in der Lage, auf die Konsole zuzugreifen.**

2. Drücken Sie in der Sitzung, die geöffnet wird, einmal die EINGABETASTE, um eine Eingabeaufforderung zu öffnen. 

3. Sie werden aufgefordert, die Sprache auszuwählen, die für Ihr Gerät festgelegt werden soll. Geben Sie die Sprache an, und drücken Sie dann die EINGABETASTE. 

    ![StorSimple – Konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)

4. Wählen Sie im Menü der seriellen Konsole, das angezeigt wird, Option 1 aus, d. h. die Anmeldung mit Vollzugriff. 

    ![StorSimple – Registrieren des Geräts 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
  
     Führen Sie die Schritte 5 bis 12 aus, um die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren. **Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden.** Das Menü der seriellen Konsole gibt den Zustand des Controllers in der Bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen Sie die Verbindung, und stellen Sie dann eine Verbindung mit dem aktiven Controller her.

5. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät ist **Kennwort1**.

6. Geben Sie den folgenden Befehl ein: `Invoke-HcsSetupWizard` 

7. Es wird ein Installations-Assistent angezeigt, der Sie beim Konfigurieren der Netzwerkeinstellungen für das Gerät unterstützt. Geben Sie die folgenden Informationen an: 
   - IP-Adresse für die Netzwerkschnittstelle DATA 0
   - Subnetzmaske
   - Gateway
   - IP-Adresse für den primären DNS-Server
    
        Beachten Sie, dass das System die Netzwerkeinstellungen nach jedem Schritt im Vorgang überprüft.
   
      > [AZURE.NOTE] Sie müssen möglicherweise warten einige Minuten, bis die Subnetzmaske und die DNS-Einstellungen angewendet werden. Wenn die Fehlermeldung "Überprüfen Sie die Netzwerkverbindung mit DATA 0" angezeigt wird, überprüfen Sie die physische Netzwerkverbindung für die Netzwerkschnittstelle DATA 0 Ihres aktiven Controllers.

8. (Optional) Konfigurieren Sie Ihren Webproxyserver. Webproxy-Konfiguration ist zwar optional, **Beachten Sie, dass wenn Sie einen Webproxy verwenden, nur hier konfiguriert werden können**. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md).

9. Konfigurieren Sie einen primären NTP-Server für Ihr Gerät NTP-Server sind für die Zeitsynchronisierung erforderlich, damit Ihr Gerät beim Clouddienstanbieter authentifiziert werden kann. Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum ins Internet zulässt. Wenn dies nicht möglich ist, geben Sie einen internen NTP-Server an. 
 
10. Aus Sicherheitsgründen läuft das Administratorkennwort für das Gerät nach der ersten Sitzung ab, und Sie müssen es jetzt ändern. Geben Sie, wenn Sie dazu aufgefordert werden, ein Administratorkennwort für das Gerät an. Ein gültiges Administratorkennwort für das Gerät muss zwischen 8 und 15 Zeichen lang sein. Das Kennwort muss aus drei der folgenden Zeichenkategorien bestehen: Kleinbuchstaben, Großbuchstaben, Zahlen und Sonderzeichen.

    <br/>![StorSimple – Registrieren des Geräts 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)

11. Der letzte Schritt im Installations-Assistenten besteht im Registrieren Ihres Geräts beim StorSimple-Manager-Dienst. Zu diesem Zweck benötigen Sie den Dienstregistrierungsschlüssel, den Sie in Schritt 2 abgerufen haben. Nachdem Sie den Registrierungsschlüssel bereitgestellt haben, müssen Sie ggf. einige Minuten warten, bis das Gerät registriert wurde. 

      > [AZURE.NOTE] Sie können STRG + C drücken, jederzeit aufrufen, um den Assistenten zu beenden. Wenn Sie alle Netzwerkeinstellungen (IP-Adresse für DATA 0, Subnetzmaske und Gateway) angegeben haben, werden Ihre Einträge beibehalten.

    ![StorSimple – Registrieren des Geräts 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)

12. Nachdem das Gerät registriert wurde, wird ein Schlüssel für die Dienstdatenverschlüsselung angezeigt. Kopieren Sie diesen Schlüssel, und bewahren Sie ihn an einem sicheren Ort auf. **Dieser Schlüssel ist mit dem Dienstregistrierungsschlüssel zum Registrieren weiterer Geräte bei StorSimple-Manager-Dienst erforderlich.** Finden Sie unter [StorSimple-Sicherheit](../articles/storsimple/storsimple-security.md) für Weitere Informationen zu diesem Schlüssel.
    
    ![StorSimple – Registrieren des Geräts 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    

      > [AZURE.NOTE] Um den Text aus dem Fenster der seriellen Konsole zu kopieren, markieren Sie den Text einfach. Sie sollten damit in der Lage sein, ihn in die Zwischenablage oder einen beliebigen Texteditor zu kopieren. Verwenden Sie NICHT STRG+C zum Kopieren des Schlüssels für die Dienstdatenverschlüsselung. STRG+C bewirkt, dass der Installations-Assistent beendet wird. Dies führt dazu, dass das Geräteadministratorkennwort nicht geändert und das Gerät auf das Standardkennwort zurückgesetzt wird.

13. Schließen Sie die serielle Konsole.

14. Kehren Sie zum klassischen Azure-Portal zurück, und führen Sie die folgenden Schritte aus:
  1. Doppelklicken Sie auf dem StorSimple-Manager-Dienst für den Zugriff auf die **Schnellstart** Seite.
  2. Klicken Sie auf **Anzeigen verbundener Geräte**.
  3. Auf der **Geräte** sicher, dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status. Der Status des Geräts muss **Online**.
   
        ![StorSimple – Seite "Geräte"](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
  
        Wenn der Gerätestatus **Offline**, warten Sie einige Minuten, bis das Gerät online geschaltet werden kann. 
      
        Wenn das Gerät nach einigen Minuten immer noch offline ist, müssen Sie sicherstellen, dass Ihre Firewall-Netzwerk konfiguriert wurde, wie in beschrieben die [netzwerkanforderungen für Ihr StorSimple-Gerät](../articles/storsimple/storsimple-system-requirements.md). 

        Wenn HTTP 1.1 nicht unterstützt wird, überprüfen Sie Port 9354 um sicherzustellen, dass er für die ausgehende Kommunikation geöffnet ist. Dieser Port wird für die Kommunikation zwischen dem StorSimple Manager-Dienst und Ihrem StorSimple-Gerät verwendet.
     
       


<!--author=SharS last changed: 9/17/15-->

#### So installieren Sie regelmäßige Hotfixes über Windows PowerShell für StorSimple

1. Stellen Sie eine Verbindung mit der seriellen Gerätekonsole her. Weitere Informationen finden Sie unter [Schritt 1: Verbinden mit der seriellen Konsole](storsimple-update-device.md#step1).

2. Wählen Sie im Menü seriellen Konsole Option 1, **Anmeldung mit Vollzugriff**. Geben Sie das Kennwort ein. Das Standardkennwort lautet **Kennwort1**.

3. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `Start-HcsHotfix`

       >[AZURE.IMPORTANT]
       >
       >- Dieser Befehl gilt nur für regelmäßige Hotfixes. Dieser Befehl muss nur auf einem Controller ausgeführt werden, es werden jedoch beide Controller aktualisiert.
       >- Möglicherweise werden Sie während des Updatevorgangs ein Failover des Controllers bemerken. Dieses Failover wirkt sich jedoch nicht auf die Systemverfügbarkeit oder den Betrieb aus.

4. Geben Sie bei entsprechender Aufforderung den Pfad zur Netzwerkfreigabe mit den Hotfix-Dateien ein.

5. Sie werden aufgefordert, diesen Schritt zu bestätigen. Typ **Y** mit der Installation des Hotfixes fortzufahren.



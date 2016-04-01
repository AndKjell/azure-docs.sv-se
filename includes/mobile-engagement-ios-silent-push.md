> [AZURE.IMPORTANT] Um Pushbenachrichtigungen von Mobile Engagement zu erhalten, müssen Sie aktivieren `Silent Remote Notifications` in Ihrer Anwendung. Sie müssen dem "UIBackgroundModes"-Array in der Datei "Info.plist" den Remote-Benachrichtigungswert hinzufügen.

1. Öffnen Sie die Datei `info.plist` im Projekt.
2. Klicken Sie mit der rechten Maustaste auf das oberste Element in der Liste (`Information Property List`), und fügen Sie eine neue Zeile hinzu.

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)

3. Geben Sie in der neuen Zeile `Required background modes` ein.

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)

4. Klicken Sie auf den Pfeil nach links, um die Zeile zu erweitern.
5. Fügen Sie dem Element 0 den folgenden Wert hinzu: `App downloads content in response to push notifications`.

    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)

6. Sobald Sie die Änderung vornehmen, sollte die XML-Datei "Info.plist" den folgenden Schlüssel und Wert enthalten:

        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>

7. Bei Verwendung von **Xcode 7 +** und **iOS 9 +**:
    - Aktivieren Sie **Pushbenachrichtigungen** Ziele > den Zielnamen Ihres > Funktionen.



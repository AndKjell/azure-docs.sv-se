<properties
    pageTitle="Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite"
    description="Bei diesem Thema wird erklärt, wie viele Unternehmen ein einheitliches Erscheinungsbild für all ihre verwalteten Websites und Dienste anstreben, damit ihre Endbenutzer beim Besuch dieser Websites nicht verwirrt werden."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/30/2015" 
    ms.author="MarkVi"/>

# Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite

> [AZURE.NOTE]
>
- Unternehmensbranding ist ein Feature, das nur verfügbar ist, wenn Sie Ihr Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
- Die Azure Active Directory-Editionen Premium und Basic stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Weitere Informationen kontaktieren Sie uns die [Azure Active Directory-Forum](http://feedback.azure.com/forums/169401-azure-active-directory).

Viele Unternehmen streben ein einheitliches Erscheinungsbild für all ihre verwalteten Websites und Dienste an, damit ihre Endbenutzer beim Besuch dieser Websites nicht verwirrt werden. Azure Active Directory ermöglicht dies, indem Sie die Anpassung der Darstellung folgender Websites für Endbenutzer vornehmen können, sodass Firmenlogo und Farbschemas enthalten sind:

- **Anmeldeseite** -auf dieser Seite werden Benutzer weitergeleitet, wenn sie anmelden Office 365 oder anderen webbasierten und modernen Anwendungen, die Azure AD als Identitätsanbieter verwenden. Die meisten Benutzer interagieren mit dieser Seite entweder beim Durchlaufen der Startbereichserkennung, was dem System das Umleiten von Verbundbenutzern auf ihren lokalen STS (z. B. AD FS) ermöglicht, oder bei der Eingabe ihrer Anmeldeinformationen.

- **Seite "Zugriffsbereich" Zugriff** – der Zugriffsbereich ist ein webbasiertes Portal, das einem Endbenutzer mit Geschäfts-oder schulkonto in Azure AD-Verzeichnis anzuzeigen und zu starten Cloud-basierte Clientanwendungen ermöglicht, sie der Zugriff von Azure AD-Administrator gewährt wurde. Der Zugriffsbereich ist für alle Benutzer in Ihrer Organisation über myapps.microsoft.com zugänglich.

## Anpassung der Anmeldeseite

Die Anmeldeseite ist in der Regel die am häufigsten von Endbenutzern verwendete Webseite, die browserbasierten Zugriff auf die Cloudanwendungen und -dienste benötigen, die Ihre Organisation abonniert. Daher ist das Aussehen dieser Seite äußerst wichtig. Wenn Sie die Standardanmeldeseite ohne Branding verwenden möchten, ist keine weitere Aktion erforderlich.

### Wie lange dauert es, bis die Brandingänderungen auf den Anmeldeseiten sichtbar werden?

Es kann bis zu einer Stunde dauern, bis Benutzer Neuerungen sehen, die Sie am Branding der Anmeldeseite vorgenommen haben.

### Wann wird den Benutzern eine Anmeldeseite mit Branding angezeigt?

Benutzer wird eine Anmeldeseite mit Branding angezeigt, wenn sie einen Dienst mit einer mandantenspezifischen URL wie z. B. https://outlook.com/ besuchen**Contoso**.com oder https://mail.**Contoso**.com (Wenn Sie einen CNAME-Eintrag erstellt haben).

Wenn sie einen Dienst ohne mandantenspezifische URLs (z. B. https://mail.office365.com) besuchen, sehen sie eine Anmeldeseite ohne Branding. Der Anmeldeseite wird aktualisiert, um Ihr Branding anzuzeigen, sobald die Benutzer ihre Benutzer-ID eingegeben oder eine Benutzerkachel ausgewählt haben.

> [AZURE.NOTE]
>
- Ihr Domänenname muss als "Aktiv" angezeigt, dem **Active Directory** > **Verzeichnis** > **Domänen** Abschnitt der Azure-Verwaltungsportal, in dem Sie das branding konfiguriert haben.
- Die Anmeldeseite mit dem Branding wird nicht auf die Verbraucheranmeldeseite von Microsoft übertragen. Dies bedeutet, dass Benutzer, die sich mit einem persönlichen Microsoft-Konto (früher Windows Live ID) anmelden, eine Liste an von Azure AD gerenderten Benutzerkacheln mit Branding sehen, aber das Branding Ihrer Organisation nicht auf die Microsoft-Kontoanmeldeseite übertragen wird.

### Was wird meinen Endbenutzern angezeigt, nachdem ich die Anmeldeseite angepasst habe?

Wenn Sie Ihre Unternehmensmarke, -farben und andere anpassbare Elemente auf dieser Seite anzeigen möchten, schauen Sie sich die folgenden Bilder an, um den Unterschied zwischen den beiden Eindrücken zu verstehen.

Wenn ein Benutzer versucht, über einen desktop-Computer anzumelden, hier ist ein Beispiel für die der Benutzer auf die Office 365-Anmeldeseite würde *vor* anpassen:

![][1]

Und hier sieht derselbe Benutzer *nach* anpassen:

![][2]

Wenn ein Benutzer versucht, über ein mobiles Gerät anzumelden, hier ist ein Beispiel für die der Benutzer auf die Office 365-Anmeldeseite würde *vor* anpassen:

![][3]

Und hier sieht derselbe Benutzer *nach* anpassen:

![][4]

### Welche Elemente auf der Seite können angepasst werden?

Sie können die folgenden Elemente auf der Anmeldeseite anpassen:

![][5]

 Seitenelement  | Position auf der Seite
    ------------- | -------------
Bannerlogo | Rechts oben auf der Seite angezeigt. Ersetzt das Logo, das normalerweise von der Zielwebsite angezeigt wird, bei der sich die Benutzer anmelden (z. B. Office 365 oder Azure).
Große Abbildung / Hintergrundfarbe | Am linken Rand der Seite angezeigt. Ersetzt das Bild, das normalerweise von der Zielwebsite angezeigt wird, bei der sich die Benutzer anmelden. Die Hintergrundfarbe wird möglicherweise bei Verbindungen mit geringer Bandbreite oder auf sehr kleinen Bildschirmen anstelle der großen Abbildung angezeigt.
Text der Anmeldeseite | Über dem Seitenfuß angezeigt, wenn Sie hilfreiche Informationen an Ihre Benutzer vermitteln wollen, bevor sie sich mit Ihrem Geschäfts- oder Schulkonto anmelden. Ein Beispiel: Sie möchten die Telefonnummer zu Ihrem Helpdesk oder einen rechtlichen Hinweis einfügen.

> [AZURE.NOTE]
Alle Elemente sind optional. Wenn Sie z. B. ein Bannerlogo, jedoch keine große Abbildung angeben, wird die Anmeldeseite Ihr Logo und die Abbildung für den Zielstandort (d. h. das Office 365-Bild mit dem kalifornischen Highway) anzeigen.

Sie können auch alle Elemente auf dieser Seite lokalisieren. Sobald Sie einen "Standard"-Satz an Anpassungselementen konfiguriert haben, können Sie zusätzliche Versionen für verschiedene Gebietsschemas konfigurieren. Sie können auch verschiedene Elemente miteinander kombinieren. Dazu zählen z. B.:

- Erstellen Sie eine große "Standard"-Abbildung, die für alle Kulturen funktioniert, und erstellen Sie dann spezifische Versionen für Englisch und Französisch. Benutzer mit Browsern, die auf eine dieser beiden Sprachen festgelegt sind, sehen das spezifische Bild, während allen anderen das Standardbild angezeigt wird.
- Konfigurieren Sie verschiedene Logos für Ihre Organisation (z. B. japanische oder hebräische Versionen).

### Wie wird die Abbildung angezeigt, nachdem die Größe des Browsers geändert wurde?

Während der Größenänderung eines Browserfensters wird die große Abbildung (wie die zuvor gezeigte) fast immer so zugeschnitten, dass verschiedene Bildschirm-Seitenverhältnisse möglich sind. Vor diesem Hintergrund sollten Sie die wichtigsten visuellen Elemente in der Abbildung beibehalten, sodass Sie immer in der oberen linken Ecke (oben rechts für Rechts-nach-links-Sprachen) erscheinen. Dies ist wichtig, da die Größenänderung normalerweise über die rechte untere Ecke nach oben links oder von unten nach oben erfolgt.

Die folgende Abbildung zeigt, wie die Abbildung zugeschnitten wird, wenn die Größe des Browsers nach links verschoben wird:

![][6]

Und hier wird deutlich, wie der Browser nach Verschiebung der Größe nach oben aussehen wird:

![][7]

## Anpassung der Zugriffsbereichsseite

Die Seite "Zugriffsbereich" ist im Wesentlichen eine Portalseite für alle Endbenutzer, die schnellen Zugriff über klickbare Anwendungskacheln auf verschiedene Cloud-Anwendungen benötigen, auf die Sie Zugriff erteilt haben. Wenn Sie die Standardzugriffsbereichsseite ohne Branding verwenden möchten, ist keine weitere Aktion erforderlich.

### Was sehen die Endbenutzer, nachdem die Zugriffsbereichsseite angepasst wurde?

![][8]

## Konfigurieren Sie Ihr Verzeichnis mit Unternehmensbranding

Ein Standardsatz von anpassbaren Elementen kann pro Verzeichnis im Verwaltungsportal konfiguriert werden. Nachdem die Standardeinstellungen gespeichert wurden, hat ein Administrator auch die Möglichkeit, lokalisierte Versionen jedes Elements für verschiedene Sprachen/Gebietsschemas hinzuzufügen. Alle anpassbaren Elemente sind optional.

Wenn Sie z. B. ein Standardbannerlogo konfigurieren, jedoch keine große Abbildung, so wird Ihr Logo auf der Anmeldeseite in der oberen rechten Ecke angezeigt und ansonsten die Standardabbildung. Wenn Sie ein Standardbannerlogo und den Text der Anmeldeseite auf Englisch konfigurieren sowie eine sprachspezifische Anmeldeseite für Deutsch, wird Benutzern mit einer deutscher Spracheinstellung das Standardbannerlogo und deutscher Text angezeigt. Während Sie technisch gesehen einen anderen Satz für jede von Azure AD unterstützte Sprache konfigurieren könnten, empfehlen wir, dass die Anzahl der Varianten aus Wartungs- und Leistungsgründen so klein wie möglich bleibt.

So fügen Sie Ihrem Verzeichnis Unternehmensbranding hinzu:

1. Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com) als Administrator des Verzeichnisses, das Sie anpassen möchten.
2. Wählen Sie das Verzeichnis, das Sie anpassen möchten.
3. Wählen Sie die **konfigurieren** Registerkarte, und wählen Sie dann **Branding anpassen**.
4. Ändern Sie die Elemente, die Sie anpassen möchten. Beachten Sie, dass alle Felder optional sind.
5. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Benutzer Neuerungen sehen, die Sie am Branding der Anmeldeseite vorgenommen haben.

So fügen Sie sprachspezifisches Unternehmensbranding hinzu:

1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com), unter der **konfigurieren** Registerkarte **Branding anpassen**.
2. Wählen Sie **Hinzufügen von branding für eine bestimmte Sprache**, wählen Sie die Sprache, die Sie verwenden möchten, das Logo anpassen, und klicken Sie dann auf **Weiter**.
3. Bearbeiten Sie nur die Elemente, für die Sie sprachspezifische Überschreibungen konfigurieren möchten. Beachten Sie, dass alle Felder optional sind. Wenn ein Feld leer bleibt, wird stattdessen der benutzerdefinierte Standardwert angezeigt (oder die Microsoft-Standardeinstellung, wenn kein benutzerdefinierter Standardwert konfiguriert ist).
4. Klicken Sie auf **Speichern**.

So entfernen Sie das Unternehmensbranding aus Ihrem Verzeichnis

1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com), unter der **konfigurieren** Registerkarte **Branding anpassen**.
2. Wählen Sie auf der Seite Branding anpassen **vorhandene Brandingeinstellungen bearbeiten** und wechseln Sie dann zur nächsten Seite.
3. Abhängig von den Elementen, die Sie entfernen möchten, führen Sie eine oder mehrere der folgenden Aktionen aus:
    1. Banner-Logo klicken Sie auf das Kontrollkästchen **hochgeladenes Logo entfernen**.
    2. Kachel-Logo klicken Sie auf das Kontrollkästchen **hochgeladenes Logo entfernen**.
    3. Für die Benutzernamenbezeichnung auf der Anmeldeseite löschen Sie den gesamten Text.
    4. Für den Text auf der Anmeldeseite löschen Sie den gesamten Text.
    5. Abbildung der Anmeldeseite, klicken Sie auf das Kontrollkästchen **Abbildung entfernen**.
    6. Für die Hintergrundfarbe auf der Anmeldeseite löschen Sie den gesamten Text.
4. Klicken Sie auf **Speichern** auf die Elemente zu entfernen.
5. Klicken Sie gegebenenfalls auf **Branding anpassen** erneut, und wiederholen Sie diese Schritte für das gesamte sprachspezifische branding, das entfernt werden soll.
    Alle Brandingeinstellungen wurden entfernt, wenn Sie auf **Branding anpassen** und finden Sie unter der **standardmäßiges Branding anpassen** Form ohne Einstellungen konfiguriert.

## Testen und Beispiele

Es wird empfohlen, dass Sie mit einem Testmandanten experimentieren, bevor Sie Änderungen in der Produktionsumgebung vornehmen. Die einfachste Möglichkeit zum Überprüfen, ob Ihr branding angewendet wurde, werden von einer InPrivate- oder Inkognito-Browsersitzung öffnen und dann auf https://outlook.com/contoso.com, und Ersetzen Sie contoso.com durch die Domäne, die Sie angepasst haben ab. Beachten Sie, dass dies auch für Domänen funktioniert, die wie contoso.onmicrosoft.com aussehen.

Um Ihnen bei der Erstellung effektiver Anpasssätze zu unterstützen, haben wir die folgenden beiden fiktiven Anmeldeseiten angepasst:

- [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
- [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

Um die sprachspezifischen Einstellungen zu testen, müssen Sie die Standard-Spracheinstellungen in Ihrem Webbrowser auf eine Sprache ändern, die Sie in Ihrer Anpassung festgelegt haben. In Internet Explorer wird dies in konfiguriert die **Internetoptionen** Menü.

## Anpassbare Elemente

Einige anpassbare Elemente in Azure AD dienen mehreren Verwendungszwecken. Firmenlogos können einmal pro Verzeichnis konfiguriert werden und werden sowohl auf der Anmeldeseite als auch auf der Zugriffsbereichsseite verwendet, wobei einige anpassbare Elemente speziell auf der Anmeldeseite dargestellt werden. Die folgende Tabelle enthält die Details für die verschiedenen anpassbaren Elemente.

Name | Beschreibung | Einschränkungen | Empfehlungen
    ------------- | ------------- | ------------- | -------------
Bannerlogo | Das Bannerlogo wird auf der Anmeldeseite und im Zugriffsbereich angezeigt. | <p>JPG oder PNG</p><p>60 x 280 Pixel</p><p>10 KB</p> | <p>Verwenden Sie das vollständige Logo Ihrer Organisation (einschließlich Piktogramm und firmenschriftzug)</p><p>Halten Sie es unter 30 Pixel hoch, um Bildlaufleisten auf mobilen Geräten zu vermeiden</p><p>Halten Sie es kleiner als 4 KB</p><p>Verwenden Sie eine transparente PNG-Datei (nicht davon aus, dass die Anmeldeseite immer einen weißen Hintergrund hat)</p>
Kachellogo | (zurzeit nicht auf der Anmeldeseite verwendet) Dieser Text kann in Zukunft verwendet werden, um das generische "Geschäfts- oder Schulkonto"-Piktogramm an unterschiedlichen Stellen zu platzieren. | <p>JPG oder PNG</p><p>120 x 120 Pixel</p><p>10 KB</p> | <p>Halten Sie es einfach (kein kleiner Text), wie dieses Abbild um 50 % verkleinert werden kann
</p> |
Benutzernamenbezeichnung auf der Anmeldeseite | (zurzeit nicht auf der Anmeldeseite verwendet) Dieser Text kann in Zukunft verwendet werden, um die generische "Geschäfts- oder Schulkonto"-Zeichenfolge an unterschiedlichen Stellen zu platzieren. Sie können ihn beispielsweise auf "Contoso-Konto" oder "Contoso-ID" festlegen. | <p>Unicode-Text, bis zu 50 Zeichen</p><p>Als nur-Text (keine Links oder HTML-Tags)</p> | <p>Halten Sie es kurz und einfach</p><p>Bitten Sie die Benutzer an, wie sie in der Regel Geschäfts- oder schulkonto an, die Sie mit bereitstellen.</p>
Text der Anmeldeseite | Dieser "Textbaustein" wird unter dem Anmeldeseitenformular angezeigt und kann verwendet werden, um zusätzliche Anweisungen zu kommunizieren oder mitzuteilen, wo es Hilfe und Support gibt. | <p>Unicode-Text, bis zu 256 Zeichen</p><p>Als nur-Text (keine Links oder HTML-Tags)</p> | Verwenden Sie maximal 250 Zeichen (ungefähr drei Zeilen Text)
Abbildung auf der Anmeldeseite | Die Abbildung zeigt ein großes Bild, das auf der Anmeldeseite links neben dem Anmeldeseitenformular angezeigt wird. | <p>JPG oder PNG</p><p>1420 x 1200</p><p>500 KB</p> | <p>1420 x 1200 Pixel</p><p>Wichtig: Halten Sie es so klein wie möglich, idealerweise unter 200 KB. Wenn dieses Bild zu groß ist, beeinträchtigt es die Leistung der Anmeldeseite, wenn das Bild nicht zwischengespeichert wird</p><p>Dieses Bild wird fast immer zugeschnitten, um verschiedene Bildschirm-Seitenverhältnisse zu ermöglichen. Halten die visuellen Hauptelemente in der oberen linken Ecke (oben rechts für RTL-Sprachen), weil die Größe der Ecke unten rechts nach oben/links, erfolgt, wenn das Browserfenster verkleinert wird.</p>
Hintergrundfarbe auf der Anmeldeseite | Die Hintergrundfarbe auf der Anmeldeseite wird im Bereich links neben dem Anmeldeseitenformular verwendet. Dies wird sichtbar, wenn keine Abbildung auf der Anmeldeseite vorhanden ist. | Muss eine RGB-Farbe im hexadezimalen Format sein (Beispiel: #FFFFFF) | <p>Die Hintergrundfarbe kann anstelle der großen Abbildung Verbindungen mit geringer Bandbreite angezeigt werden</p><p>Wir empfehlen, die Grundfarbe des Banner-Logo auszuwählen</p>


## Nächste Schritte

- [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Anzeigen von Zugriffs- und Nutzungsberichten](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png



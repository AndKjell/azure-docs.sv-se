## Was sind Service Bus-Themen und -Abonnements?

Service Bus-Themen und-Abonnements unterstützen ein *Veröffentlichen/Abonnieren* Modell der messagingkommunikation. Bei der Verwendung von Themen und Abonnements kommunizieren die Komponenten einer verteilten Anwendung nicht direkt miteinander, sondern tauschen Nachrichten über ein Thema aus, das als Zwischenstufe fungiert.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Anders als bei Service Bus-Warteschlangen, bei denen jede Nachricht von einem einzelnen Consumer verarbeitet wird, bieten Themen und Abonnements eine 1:n-Kommunikationsform mit einem Veröffentlichungs- und Abonnementsmuster. Es ist möglich
Registrieren Sie mehrere Abonnements zu einem Thema. Wenn eine Nachricht an ein Thema gesendet wird, steht sie in jedem Abonnement zur Verfügung, wo sie unabhängig von den anderen Abonnements verarbeitet wird.

Ein Themenabonnement ähnelt einer virtuellen Warteschlange, die Kopien der Nachrichten enthält, die an das Thema gesendet wurden. Sie können optional auch Filterregeln für einzelne Abonnements eines Themas anmelden. Auf diese Weise können Sie filtern/einschränken, welche Nachrichten an ein Thema von welchen Themenabonnements empfangen werden.

Mit Service Bus-Themen und -Abonnements können Sie sehr viele Nachrichten an sehr viele Benutzer und Anwendungen verarbeiten.

## Erstellen eines Dienstnamespaces

Um mit der Verwendung von Service Bus-Themen und -Abonnements in Azure beginnen zu können, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Namespace ist ein Bereichscontainer für die Adressierung von Service Bus-Ressourcen innerhalb Ihrer Anwendung.

So erstellen Sie einen Dienstnamespace:

1.  Melden Sie sich auf die [Azure-Portal][].

2.  Klicken Sie im linken Navigationsbereich des Verwaltungsportals auf **Service Bus**.

3.  Klicken Sie im unteren Bereich des Portals auf **Erstellen**.   
    ![][0]

4.  In der **einen neuen Namespace hinzufügen** Dialogfeld Geben Sie einen Namespace ein. Das System überprüft sofort, ob dieser Name verfügbar ist.   
    ![][2]

5.  Wählen Sie nach der Bestätigung, dass der Name für den Namespace verfügbar ist, das Land oder die Region, wo dieser Namespace gehostet werden soll. (Stellen Sie sicher, dass dies dasselbe Land/dieselbe Region ist, in dem/der sie Ihre Rechnerressourcen einsetzen.)

    > [AZURE.IMPORTANT] Wählen Sie die **derselben Region** wählen Sie für Ihre Anwendung bereitstellen möchten. Dies sorgt für die beste Leistung.

6.  Lassen Sie die anderen Felder im Dialogfeld die Standardwerte (**Messaging** und **Standardebene**), klicken Sie dann auf das Häkchen. Ihr Dienstnamespace wird nun erstellt und aktiviert. Eventuell müssen Sie einige Minuten warten, bis die Ressourcen für Ihr Konto bereitgestellt werden.

    ![][6]

## Abrufen der Standard-Anmeldeinformationen für den Namespace

Wenn Sie Verwaltungsvorgänge ausführen möchten, z. B. die Erstellung eines Themas oder Abonnements im neuen Namespace, müssen Sie die Anmeldeinformationen für den Namespace abrufen. Diese Anmeldeinformation erhalten Sie im Azure-Portal.

### Abrufen der Anmeldeinformationen für die Verwaltung aus dem Portal

1.  Klicken Sie im linken Navigationsbereich auf die **Service Bus** Knoten, um die Liste der verfügbaren Namespaces anzuzeigen:   
    ![][0]

2.  Wählen Sie in der angezeigten Liste den Namespace, den Sie gerade erstellt haben:   
    ![][3]

3.  Klicken Sie auf **Verbindungsinformationen**.   
    ![][4]

4.  In den **Zugriff auf die Verbindungsinformationen** Dialogfeld finden Sie die Verbindungszeichenfolge, die den SAS-Schlüssel und den Schlüsselnamen enthält. Notieren Sie sich diese Werte, da Sie diese Informationen später benötigen, um Vorgänge mit dem Namespace durchzuführen. 


  [Azure portal]: http://manage.windowsazure.com
  [0]: ./media/howto-service-bus-topics/sb-queues-13.png
  [2]: ./media/howto-service-bus-topics/sb-queues-04.png
  [3]: ./media/howto-service-bus-topics/sb-queues-09.png
  [4]: ./media/howto-service-bus-topics/sb-queues-06.png
  
  [6]: ./media/howto-service-bus-topics/getting-started-multi-tier-27.png




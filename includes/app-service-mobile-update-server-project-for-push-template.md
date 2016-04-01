Verwenden Sie das Verfahren, das entspricht den Back-End-Projekttyp & ndash; entweder [.NET Back-End-](#dotnet) oder [Node.js Back-End-](#nodejs).

### <a name="dotnet"></a>.NET-Back-End-Projekt
1. Klicken Sie in Visual Studio mit der rechten Maustaste in des Server-Projekts, und klicken Sie auf **NuGet-Pakete verwalten**, suchen Sie nach `Microsoft.Azure.NotificationHubs`, klicken Sie dann auf **Installieren**. Dadurch wird die Notification Hubs-Bibliothek zum Senden von Benachrichtigungen von Ihrem Back-End installiert.

3. Öffnen Sie im Server-Projekt **Controller** > **TodoItemController.cs**, und fügen Sie die folgende using-Anweisungen:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. In der **PostTodoItem** -Methode, fügen Sie den folgenden Code nach dem Aufruf von **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Dieser Code weist den Benachrichtigungs-Hub an, eine Vorlagenbenachrichtigung an alle Vorlagenregistrierungen zu senden, die „messageParam“ enthalten. Die Zeichenfolge wird anstelle von „messageParam“ für jedes PNS eingefügt, das eine Registrierung mit „messageParam“ aufweist. Dadurch können Sie die Benachrichtigung an APNS, GCM, WNS oder eine beliebige andere PNS senden.

    Weitere Informationen zu Vorlagen mit Notification Hubs finden Sie unter [Vorlagen](notification-hubs-templates.md). 

### <a name="nodejs"></a>Node.js-Back-End-Projekt

1. Ersetzen Sie den bestehenden Code in der Datei „todoitem.js“ durch den folgenden Code:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": context.item.text}'; 
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Damit wird eine Vorlagenbenachrichtigung gesendet, die „item.text“ enthält, wenn ein neues Todo-Element eingefügt wird.

2. Beim Bearbeiten der Datei auf Ihrem lokalen Computer veröffentlichen Sie das Serverprojekt erneut.


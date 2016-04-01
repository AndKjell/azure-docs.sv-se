
1. Öffnen Sie die Projektdatei "MainPage.xaml.cs", und fügen Sie die folgende using-Anweisung ein:

        using Windows.UI.Popups;

2. Fügen Sie den folgenden Codeausschnitt zur MainPage-Klasse hinzu:
    
        private MobileServiceUser user;
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            while (user == null)
            {
                string message;
                try
                {
                    user = await App.MobileService
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    message = 
                        string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                        
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    Auf diese Weise wird eine Membervariable zum Speichern des aktuellen Benutzers erstellt und eine Methode zur Verarbeitung des Authentifizierungsprozesses. Der Benutzer wird mithilfe eines Facebook-Logins authentifiziert. Wenn Sie einen anderen Identitätsanbieter als Facebook verwenden, ändern Sie den Wert der **MobileServiceAuthenticationProvider** oben auf den Wert für den Anbieter.

3. Ersetzen Sie die vorhandene **OnNavigatedTo** Methode überschreiben, durch die folgende Methode, die die neue aufruft **authentifizieren** Methode:

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();
            RefreshTodoItems();
        }
        
4. Drücken Sie F5, um die App auszuführen und sich mit dem von Ihnen ausgewählten Identitätsanbieter bei der App anzumelden. 

    Wenn Sie sich erfolgreich angemeldet haben, sollte die App fehlerfrei ausgeführt werden, und Sie sollten Mobile Services abfragen und Daten aktualisieren können.


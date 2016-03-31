Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens "Parameters", der alle Parameterwerte enthält.
Sie sollten einen Parameter definieren, für das Projekt die Werte, die variiert abhängig von bereitstellen, oder basierend auf der 
die Umgebung bereitgestellt werden. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren. 

Nachfolgend wird jeder der in der Vorlage verwendeten Parameter beschrieben.

### gatewayName

Der Name des Gateways. Die API-App wird für dieses Gateway registriert.

    "gatewayName": {
      "type": "string"
    }

### apiAppName

Der Name der zu erstellenden API-App. Der Name muss zwischen 8 und 50 Zeichen lang sein.
    
    "apiAppName": {
      "type": "string"
    }

### apiAppSecret

Der geheime Schlüssel für die API-App. Bei diesem Wert muss es sich um eine base64-codierte Zeichenfolge handeln. Es sollte eine beliebige Zeichenfolge mit 64 Zeichen sein, die nur aus ganzen Zahlen und Kleinbuchstaben besteht.

    "apiAppSecret": {
      "type": "securestring"
    }

### location

Der Speicherort für die neue API-App. Sie können durch Ausführen des PowerShell-Befehls `Get-AzureLocation` oder des Befehls `azure location list` der Azure-Befehlszeilenschnittstelle gültige Speicherorte abrufen.

    "location": {
      "type": "string"
    }




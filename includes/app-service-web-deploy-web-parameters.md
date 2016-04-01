Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens "Parameters", der alle Parameterwerte enthält.
Sie sollten einen Parameter definieren, für das Projekt die Werte, die variiert abhängig von bereitstellen, oder basierend auf der 
die Umgebung bereitgestellt werden. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren. 

Verwenden Sie beim Definieren von Parametern, die **AllowedValues** Feld, um anzugeben, welche Werte ein Benutzer während der Bereitstellung angeben kann. Verwenden der **DefaultValue** Feld für den Parameter einen Wert zuweisen, wenn kein Wert angegeben wird 
während der Bereitstellung.

Jeder Parameter wird in der Vorlage beschrieben.

### siteName

Der Name der Web-App, die Sie erstellen möchten.

    "siteName":{
      "type":"string"
    }

### hostingPlanName

Der Name des App Service-Plans zum Hosten der Web-App.
    
    "hostingPlanName":{
      "type":"string"
    }

### siteLocation

Der Speicherort zum Erstellen von Web-App und Hostingplan. Dabei muss es sich um einen der Azure-Speicherorte handeln, die Web-Apps unterstützen.

    "siteLocation":{
      "type":"string"
    }

### sku

Der Tarif für den Hostingplan.

    "sku":{
      "type":"string",
      "allowedValues":[
        "Free",
        "Shared",
        "Basic",
        "Standard"
      ],
      "defaultValue":"Free"
    }

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind (Free, Shared, Basic oder Standard), und weist einen Standardwert (Free) zu, wenn kein Wert angegeben wird.

### workerSize

Die Instanzgröße des Hostingplans (klein, mittel oder groß).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind (0, 1 oder 2), und weist einen Standardwert (0) zu, wenn kein Wert angegeben wird. Die Werte entsprechen klein, mittel und groß.



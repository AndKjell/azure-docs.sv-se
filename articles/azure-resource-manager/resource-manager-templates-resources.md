---
title: Mallresurser med Azure Resource Manager-| Microsoft Docs
description: Beskriver resursavsnittet i Azure Resource Manager-mallar med hjälp av deklarativa JSON-syntax.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2018
ms.author: tomfitz
ms.openlocfilehash: 6723cf8cc18637c157b295361425357e1c47ec2e
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007169"
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Resursavsnittet i Azure Resource Manager-mallar

I resursavsnittet kan du definiera de resurser som är distribuerade eller uppdateras. Det här avsnittet kan bli komplicerade eftersom du måste förstå de typer som du distribuerar för att ge rätt värden.

## <a name="available-properties"></a>Tillgängliga egenskaper

Du definierar resurser med följande struktur:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Elementnamn | Krävs | Beskrivning |
|:--- |:--- |:--- |
| tillstånd | Nej | Booleskt värde som anger om resursen ska etableras under den här distributionen. När `true`, där resursen skapas under distributionen. När `false`, resursen är hoppades över för den här distributionen. |
| apiVersion |Ja |Version av REST-API för att använda för att skapa resursen. |
| typ |Ja |Typ av resursen. Det här värdet är en kombination av namnområde med resursprovidern och resurstypen (till exempel **Microsoft.Storage/storageAccounts**). |
| namn |Ja |Namnet på resursen. Namnet måste följa URI-komponent begränsningar som definierats i RFC3986. Dessutom är Azure-tjänster som exponerar resursnamnet externa parter Kontrollera namnet och kontrollera att det inte ett försök att imitera en annan identitet. |
| location |Varierar |Geo-platser som stöds för den angivna resursen. Du kan välja någon av de tillgängliga platserna, men vanligtvis det vara bra att välja ett som är nära användarna. Vanligtvis är det också vara bra att placera resurser som interagerar med varandra i samma region. De flesta typer av resurser kräver en plats, men vissa typer (till exempel en rolltilldelning) kräver inte en plats. |
| tags |Nej |Taggar som är kopplade till resursen. Lägga till taggar för att organisera resurser logiskt i din prenumeration. |
| kommentarer |Nej |Dina anteckningar för att dokumentera resurserna i mallen |
| kopiera |Nej |Om fler än en instans, hur många resurser för att skapa. Standardläget är parallell. Ange seriell läge när du inte vill att alla eller resurserna som ska distribueras på samma gång. Mer information finns i [och skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nej |Resurser som måste distribueras innan den här resursen har distribuerats. Resource Manager utvärderar beroenden mellan resurser och distribuerar dem i rätt ordning. När resurserna inte är beroende av varandra, är de distribueras parallellt. Värdet kan vara en kommaavgränsad lista över en resurs namn eller resurs unika identifierare. Endast lista över resurser som distribueras i den här mallen. Resurser som inte har definierats i den här mallen måste redan finnas. Undvik att lägga till onödiga beroenden som de kan sakta distributionen och skapa cirkulärt tjänstberoende. Anvisningar för inställningen beroenden finns i [definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md). |
| properties |Nej |Resurs-specifika konfigurationsinställningar. Värdena för egenskaperna är samma som de värden som du anger i begärandetexten för REST API-åtgärd (PUT-metoden) att skapa resursen. Du kan också ange en kopia matris för att skapa flera instanser av en egenskap. |
| SKU: n | Nej | Vissa resurser kan värden som definierar SKU för att distribuera. Du kan till exempel ange typen av redundans för ett lagringskonto. |
| typ | Nej | Vissa resurser kan ett värde som definierar typ av resurs som du distribuerar. Du kan till exempel ange vilken typ av Cosmos DB för att skapa. |
| plan | Nej | Vissa resurser kan värden som definierar planerar att distribuera. Du kan till exempel ange marketplace-avbildning för en virtuell dator. | 
| resurser |Nej |Underordnade resurser som är beroende av resursen som definieras. Ange endast resurstyper som tillåts av schemat för den överordnade resursen. Den fullständigt kvalificerade typ av den underordnade resursen omfattar resurstyp överordnade som **Microsoft.Web/sites/extensions**. Beroende på den överordnade resursen är inte underförstådd. Du måste uttryckligen definiera det beroendet. |

## <a name="condition"></a>Tillstånd

När du måste bestämma under distributionen om du vill skapa en resurs eller inte, använda den `condition` element. Värdet för det här elementet matchas till true eller false. När värdet är true, skapas resursen. När värdet är FALSKT skapas inte resursen. Normalt använder du det här värdet när du vill skapa en ny resurs eller Använd en befintlig. Till exempel vill ange om ett nytt lagringskonto har distribuerats eller ett befintligt lagringskonto används, använder du:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

För en komplett exempel-mall som använder den `condition` element, se [virtuell dator med ett nytt eller befintligt virtuellt nätverk, lagring och offentlig IP-adress](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions).

## <a name="resource-specific-values"></a>Resurs-specifika värden

Den **apiVersion**, **typ**, och **egenskaper** element är olika för varje resurstyp. Den **sku**, **typ**, och **plan** element är tillgängliga för vissa typer av resurser, men inte alla. För att fastställa värden för dessa egenskaper finns i [mallreferensen](/azure/templates/).

## <a name="resource-names"></a>Resursnamn

I allmänhet bör arbeta du med tre typer av resursnamn i Resource Manager:

* Resursnamn som måste vara unika.
* Resursnamn som inte behöver vara unikt, men du kan du välja att ange ett namn som hjälper dig att identifiera resursen.
* Resursnamn som kan vara allmän.

### <a name="unique-resource-names"></a>Unika resursnamn

Ange ett unikt resursnamn för någon resurstyp som har en slutpunkt för åtkomst av data. Vissa vanliga typer av resurser som kräver ett unikt namn är:

* Azure Storage<sup>1</sup> 
* Funktionen Web Apps i Azure App Service
* SQL Server
* Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Azure HDInsight

<sup>1</sup> lagringskontonamn måste också vara gemener, 24 tecken eller mindre, och inte har någon bindestreck.

När du anger namnet, du kan manuellt skapa ett unikt namn eller använda den [uniqueString()](resource-group-template-functions-string.md#uniquestring) funktionen för att skapa ett namn. Du kanske också vill lägga till ett prefix eller suffix till den **uniqueString** resultatet. Ändra det unika namnet kan du enkelt identifiera resurstyp från namn. Du kan till exempel generera ett unikt namn för ett lagringskonto med hjälp av följande variabel:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Resursnamn för identifiering
Vissa typer av resurser som du kanske vill namn, men namnen behöver inte vara unikt. Du kan ange ett namn som identifierar både resurs-kontexten och resurstypen för dessa typer av resurser.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Allmän resursnamn
Du kan använda ett allmänt namn som är hårdkodad i mallen för resurstyper som du oftast kommer åt via en annan resurs. Du kan till exempel ange ett standard, allmän namn på brandväggsregler på en SQLServer:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Plats
När du distribuerar en mall måste du ange en plats för varje resurs. Olika resurstyper stöds på olika platser. Om du vill se en lista över platser som är tillgängliga för din prenumeration för en viss resurstyp, använder du Azure PowerShell eller Azure CLI. 

I följande exempel använder PowerShell för att hämta platserna för den `Microsoft.Web\sites` resurstyp:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

I följande exempel används Azure CLI för att få platser för den `Microsoft.Web\sites` resurstyp:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Ange den platsen när du har fastställt platser som stöds för dina resurser i din mall. Det enklaste sättet att ange det här värdet är att skapa en resursgrupp på en plats som har stöd för resurstyperna och inställd på varje plats `[resourceGroup().location]`. Du kan distribuera om mallen till resursgrupper på olika platser och ändras inte alla värden i den mall eller parametrar. 

I följande exempel visas ett lagringskonto som har distribuerats till samma plats som resursgruppen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Om du behöver att hårdkoda platsen i mallen kan du ange namnet på en av regionerna som stöds. I följande exempel visas ett lagringskonto som distribueras alltid till norra centrala USA:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>Taggar
[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

### <a name="add-tags-to-your-template"></a>Lägga till taggar i mallen

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Underordnade resurser

Du kan också definiera en uppsättning underordnade resurser inom vissa typer av resurser. Underordnade resurser finns resurser som finns bara inom ramen för en annan resurs. Till exempel kan inte en SQL-databas finnas utan en SQLServer så att databasen är en underordnad till servern. Du kan definiera databasen i definitionen för servern.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

När kapslade, vilken anges till `databases` men dess fullständiga resurstypen är `Microsoft.Sql/servers/databases`. Du inte anger `Microsoft.Sql/servers/` eftersom det antas från den överordnade resurstypen. Namnet på underordnade resursen är inställd `exampledatabase` men det fullständiga namnet innehåller namnet på överordnade. Du inte anger `exampleserver` eftersom det antas från den överordnade resursen.

Formatet för den underordnade resurstypen är: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

Formatet på namnet på underordnade resursen är: `{parent-resource-name}/{child-resource-name}`

Men du behöver definiera databasen på servern. Du kan definiera den underordnade resursen på den översta nivån. Du kan använda den här metoden om den överordnade resursen inte är distribuerat i samma mall, eller om vill använda `copy` och skapa flera underordnade resurser. Med den här metoden måste du ange fullständig resurstypen och inkludera namnet på överordnade resursen i namnet på underordnade resursen.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

När en fullständiga referens till en resurs, inte bara en sammanfogning av två den för att kombinera segment från typ och namn. Använd i stället en sekvens med efter namnområdet, *typnamn/* par från minst specifika att mest specifika:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Exempel:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` stämmer `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` stämmer inte

## <a name="recommendations"></a>Rekommendationer
Följande information kan vara till hjälp när du arbetar med resurser:

* För att hjälpa andra deltagare förstå syftet med resursen, ange **kommentarer** för varje resurs i mallen:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Om du använder en *offentlig slutpunkt* i mallen (till exempel ett Azure Blob storage offentlig slutpunkt), *gör inte hårdkoda* namnområdet. Använd den **referens** funktionen för att dynamiskt hämta namnområdet. Du kan använda den här metoden för att distribuera mallen till olika offentliga namnrymdsmiljöer utan att manuellt ändra slutpunkten i mallen. Ange API-versionen till samma version som du använder för storage-kontot i mallen:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Om lagringskontot har distribuerats i samma mall som du skapar, behöver du inte ange leverantörens namnområde när du refererar till resursen. I följande exempel visas syntaxen förenklade:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Om du har andra värden i mallen som är konfigurerade för att använda en offentlig namnrymd kan ändra dessa värden för att återspegla samma **referens** funktion. Du kan till exempel ange den **storageUri** egenskapen för diagnostikprofilen för virtuell dator:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Du kan också referera till ett befintligt lagringskonto som tillhör en annan resursgrupp:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Tilldela offentliga IP-adresser till en virtuell dator bara när ett program kräver. Använd inkommande NAT-regler, en virtuell nätverksgateway eller en jumpbox när du ansluter till en virtuell dator (VM) för felsökning eller för att hantera administrativa syften.
   
     Mer information om hur du ansluter till virtuella datorer finns:
   
   * [Köra virtuella datorer för en N-tier-arkitektur i Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Tillåt extern åtkomst till den virtuella datorn med hjälp av Azure portal](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Tillåt extern åtkomst till den virtuella datorn med hjälp av PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Tillåt extern åtkomst till din Linux-VM med hjälp av Azure CLI](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* Den **domainNameLabel** -egenskapen för offentliga IP-adresser måste vara unikt. Den **domainNameLabel** värdet måste vara mellan 3 och 63 tecken långt och följa de regler som anges av den här reguljärt uttryck: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Eftersom den **uniqueString** funktionen genererar en sträng som är 13 tecken lång, den **dnsPrefixString** parametern är begränsad till 50 tecken:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* När du lägger till ett lösenord för ett anpassat skripttillägg använder den **commandToExecute** -egenskapen i den **protectedSettings** egenskapen:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > För att säkerställa att hemligheter som krypteras när de skickas som parametrar till virtuella datorer och tillägg, använda den **protectedSettings** egenskapen för de relevanta tillägg.
   > 
   > 


## <a name="next-steps"></a>Nästa steg
* Om du vill visa kompletta mallar för många olika typer av lösningar kan du se [Azure-snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).
* Mer information om de funktioner du kan använda från inom en mall finns i [Azure Resource Manager-Mallfunktioner](resource-group-template-functions.md).
* Om du vill använda mer än en mall under distributionen, se [med länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).
* Du kan behöva använda resurser som finns i en annan resursgrupp. Det här scenariot är vanligt när du arbetar med lagringskonton eller virtuella nätverk som delas mellan flera resursgrupper. Mer information finns i den [resourceId funktionen](resource-group-template-functions-resource.md#resourceid).
* Information om begränsningar för resursen finns i [rekommenderade namnkonventioner för Azure-resurser](../guidance/guidance-naming-conventions.md).
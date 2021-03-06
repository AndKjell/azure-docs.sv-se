---
title: Distribuera resurser med REST API och en mall | Microsoft Docs
description: Använda Azure Resource Manager och Resource Manager REST API för att distribuera en resurser till Azure. Resurserna definieras i en Resource Manager-mall.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2018
ms.author: tomfitz
ms.openlocfilehash: ae2393d16d2c9c1000b00f5514e63c988303a83c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628519"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Distribuera resurser med Resource Manager-mallar och Resource Manager REST API

Den här artikeln förklarar hur du använder Resource Manager REST API med Resource Manager-mallar för att distribuera dina resurser till Azure.  

> [!TIP]
> Om du behöver hjälp med felsökning av ett fel under distributionen, se:
> 
> * [Visa distributionsåtgärder](resource-manager-deployment-operations.md) vill veta mer om att hämta information som hjälper till att du felsöker din fel
> * [Felsök vanliga fel när du distribuerar resurser till Azure med Azure Resource Manager](resource-manager-common-deployment-errors.md) och lär dig att lösa vanliga distributionsfel
> 
> 

Mallen kan vara en lokal fil eller en extern fil som är tillgänglig via en URI. När mallen finns i ett lagringskonto, kan du begränsa åtkomsten till mallen och ange en token för delad åtkomst (signatur) under distributionen.

## <a name="deploy-with-the-rest-api"></a>Distribuera med REST API
1. Ange [gemensamma parametrar och rubriker](/rest/api/azure/), inklusive autentiseringstoken.

2. Om du inte har en befintlig resursgrupp, skapa en resursgrupp. Ange ditt prenumerations-ID, namnet på den nya resursgruppen och platsen som du behöver för din lösning. Mer information finns i [skapa en resursgrupp](/rest/api/resources/resourcegroups/createorupdate).

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
  {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
  }
  ```

3. Verifiera distributionen innan du kör genom att köra den [Validera malldistributionen av en](/rest/api/resources/deployments/validate) igen. När du testar distributionen kan du ange parametrar, precis som när du genomför distributionen (visas i nästa steg).

4. Skapa en distribution. Ange ditt prenumerations-ID, namnet på resursgruppen, namnet på distributionen och en länk till mallen. Läs om hur mallfilen [parameterfilen](#parameter-file). Läs mer om REST-API för att skapa en resursgrupp, [skapar en för malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Observera den **läge** är inställd på **stegvis**. Om du vill köra en fullständig distribution **läge** till **Slutför**. Var försiktig när du använder det fullständiga läget som du kan oavsiktligt ta bort resurser som inte ingår i din mall.

  ```HTTP
  PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
  {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
  }
  ```

    Om du vill logga svarsinnehåll, begär innehåll eller både inkludera **debugSetting** i begäran.

  ```HTTP
  "debugSetting": {
    "detailLevel": "requestContent, responseContent"
  }
  ```

    Du kan ställa in ditt storage-konto du använder en token för delad åtkomst (signatur). Mer information finns i [delegera åtkomst med en signatur för delad åtkomst](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

5. Hämta status för malldistributionen. Mer information finns i [få information om en malldistributionen](/rest/api/resources/deployments/get).

  ```HTTP
  GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
  ```

## <a name="redeploy-when-deployment-fails"></a>Distribuera om när distributionen misslyckas

För distributioner som inte kan du ange att en tidigare distribution från distributionshistoriken automatiskt omdistribueras. Om du vill använda det här alternativet för måste dina distributioner ha unika namn så att de kan identifieras i historiken. Om du inte har unika namn, kan den aktuella misslyckad distributionen skriva över tidigare distributionen i historiken. Du kan bara använda det här alternativet med rot på distributioner. Distributioner från en kapslad mall är inte tillgängliga för omdistribution.

Om du vill distribuera om den senaste distributionen om den aktuella distributionen misslyckas, använder du:

```HTTP
"onErrorDeployment": {
  "type": "LastSuccessful",
},
```

Om du vill distribuera om en specifik distribution om den aktuella distributionen misslyckas, använder du:

```HTTP
"onErrorDeployment": {
  "type": "SpecificDeployment",
  "deploymentName": "<deploymentname>"
}
```

Den angivna distributionen måste ha lyckats.

## <a name="parameter-file"></a>Parameterfilen

Om du använder en parameterfil för att ange parametervärden under distributionen kan behöva du skapa en JSON-fil med ett format som liknar följande exempel:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Storleken på parameterfilen får inte vara mer än 64 KB.

Om du behöver ange något känsligt värde för en parameter (till exempel ett lösenord), lägger du till detta värde till ett nyckelvalv. Hämta nyckelvalvet under distributionen som du ser i exemplet ovan. Mer information finns i [skicka säkra värden under distributionen](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Nästa steg
* Om du vill ange hur du hanterar resurs som finns i resursgruppen men inte har definierats i mallen, se [distributionslägen i Azure Resource Manager](deployment-modes.md).
* Läs om hur du hanterar asynkrona REST-åtgärder i [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).
* Ett exempel för att distribuera resurser via .NET-klientbiblioteket finns i [distribuera resurser med hjälp av .NET-bibliotek och en mall](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* För att definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).
* Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](/azure/architecture/cloud-adoption-guide/subscription-governance).


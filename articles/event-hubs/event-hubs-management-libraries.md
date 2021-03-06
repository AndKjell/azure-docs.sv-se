---
title: Azure Event Hubs-hanteringsbibliotek | Microsoft Docs
description: Hantera Event Hubs-namnrymder och entiteter från .NET
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: 79cddcac4d469753bc39107e6db2d8ce901111d1
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746424"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs-hanteringsbibliotek

Du kan använda Azure Event Hubs-hanteringsbibliotek för att dynamiskt etablera Event Hubs-namnrymder och entiteter. Den här dynamiska utformningen kan komplexa distributioner och meddelandescenarier, så att du kan fastställa vilka entiteter för att etablera programmässigt. Dessa bibliotek är tillgängliga för .NET.

## <a name="supported-functionality"></a>Funktioner som stöds

* Skapa en Namespace-, update-, borttagning
* Skapa för Event Hubs-, update-, borttagning
* Skapa en konsumentgrupp-, update-, borttagning

## <a name="prerequisites"></a>Förutsättningar

Kom igång med Event Hubs-hanteringsbibliotek, måste du autentisera med Azure Active Directory (AAD). AAD kräver att du autentisera som ett huvudnamn för tjänsten som ger åtkomst till dina Azure-resurser. Information om hur du skapar ett tjänstens huvudnamn finns i någon av följande artiklar:  

* [Använd Azure-portalen för att skapa Active Directory-program och tjänstens huvudnamn som kan komma åt resurser](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Använd Azure PowerShell för att skapa ett huvudnamn för tjänsten för resursåtkomst](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Använd Azure CLI för att skapa ett huvudnamn för tjänsten för resursåtkomst](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

De här självstudierna får du en `AppId` (klient-ID), `TenantId`, och `ClientSecret` (autentiseringsnyckeln) som används för autentisering av hanteringsbiblioteken. Du måste ha **ägare** behörigheter för resursgruppen som du vill köra.

## <a name="programming-pattern"></a>Mönster för programmering

Mönster för att ändra alla Event Hubs-resurser följer ett gemensamt protokoll:

1. Hämta en token från AAD med den `Microsoft.IdentityModel.Clients.ActiveDirectory` biblioteket.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Skapa den `EventHubManagementClient` objekt.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Ange den `CreateOrUpdate` parametrar till dina angivna värden.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. Körning av anropet.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Nästa steg
* [Exempel på .NET-hantering](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub referens](/dotnet/api/Microsoft.Azure.Management.EventHub) 

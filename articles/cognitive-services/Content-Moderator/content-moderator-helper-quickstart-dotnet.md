---
title: 'Snabbstart: Content Moderator-SDK för .NET-hjälpmetoden'
titlesuffix: Azure Cognitive Services
description: Skicka tillbaka en Content Moderator-klient med hjälp av Azure Content Moderator-SDK för .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 0c167de3af8aea6820962e7cc8d0c21f5ba61807
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223278"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Snabbstart: Hjälpkod för att returnera en Content Moderator-klient

Den här artikeln innehåller information och kodexempel som hjälper dig att komma igång med Content Moderator-SDK:n för .NET, så att du kan skapa en Content Moderator-klient till din prenumeration.

Biblioteket används av andra snabbstarter i det här avsnittet.

Den här artikeln förutsätter att du redan är bekant med Visual Studio och C#.

> [!IMPORTANT]
> Klassbiblioteket innehåller kod som endast är avsedd att användas i demonstrationssyfte.
> Om du anpassar koden för användning i produktion, bör du använda en säker metod för att lagra och använda prenumerationsnyckeln till Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Registrera dig för Content Moderator-tjänster

Innan du kan använda Content Moderator-tjänster via REST-API:n eller SDK:n behöver du en prenumerationsnyckel.
Information om hur du hämtar nyckeln finns i [Snabbstart](quick-start.md).

## <a name="create-your-visual-studio-project"></a>Skapa ett Visual Studio-projekt

1. Skapa ett nytt projekt för **Klassbibliotek (.NET Framework)**.

   I exempelkoden har jag döpt projektet till **ModeratorHelper**.

1. Lägg till en referens i Framework-sammansättningen **System.Configuration**.

### <a name="install-required-packages"></a>Installera de paket som krävs

Installera följande NuGet-paket:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Skapa Content Moderator-klienten

Ersätt innehållet i ModeratorHelper.cs-filen med följande kod:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Uppdatera fälten **AzureRegion** och **CMSubscriptionKey** med värdena för din regionsidentifierare och prenumerationsnyckel.

Du har nu snabbt skapat en Content Moderator-klient för din prenumeration.

## <a name="next-steps"></a>Nästa steg

[Ladda ned Visual Studio-lösningen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) för den här och andra Content Moderator-snabbstarter för .NET och sätt igång med din integrering.

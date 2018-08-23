---
title: Anropa slutpunkten med C# - Bing Custom Search - Microsoft Cognitive Services
description: Den här snabbstarten visar hur du begär sökresultat från din anpassade Sökinstans med C# för att anropa anpassad sökning i Bing-slutpunkten.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 87970e1c5e8487f9afca2acc680bdfeb610dc89f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2018
ms.locfileid: "41987974"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Anropa anpassad sökning i Bing slutpunkt (C#)

Den här snabbstarten visar hur du begär sökresultat från din anpassade Sökinstans med C# för att anropa anpassad sökning i Bing-slutpunkten. 

## <a name="prerequisites"></a>Förutsättningar

-  En färdiga att använda anpassad Sökinstans. Se [skapar din första Bing Custom Search-instans](quick-start.md).
-  [.NET core](https://www.microsoft.com/net/download/core) installerad.
- En [Cognitive Services API-konto](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) med **API: er för Bing-sökresultat**. Den [kostnadsfri utvärderingsversion](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) är tillräcklig för den här snabbstarten. Du behöver åtkomstnyckel som tillhandahållits när du aktiverar din kostnadsfria utvärderingsversion eller du kan använda en betald prenumerationsnyckel från instrumentpanelen i Azure.  

  >[!NOTE]  
  >Befintliga Bing Custom Search-kunder som har en förhandsversion av nyckel som etablerats på eller före den 15 oktober 2017 kommer att kunna använda sina nycklar till den 30 November 2017 eller tills de har förbrukat det maximala antalet frågor tillåts. Därefter måste de migrera till den allmänt tillgängliga versionen på Azure. 
 
## <a name="run-the-code"></a>Kör koden

Följ dessa steg om du vill köra det här exemplet:

1. Skapa en mapp för din kod.
2. Navigera till mappen som du nyss skapade från en kommandotolk eller terminal.
3. Kör följande kommandon:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
   ```

4. Kopiera följande kod till Program.cs.
5. Ersätt **YOUR-SUBSCRIPTION-KEY** och **YOUR-anpassad-CONFIG-ID** med din nyckel och configuration-ID.

    ``` CSharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    
    namespace bing_custom_search_example_dotnet
    {
        class Program
        {
            static void Main(string[] args)
            {
                var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
                var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
                var searchTerm = args.Length > 0 ? args[0]: "microsoft";            
    
                var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                    "q=" + searchTerm +
                    "&customconfig=" + customConfigId;
    
                var client = new HttpClient();
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                var httpResponseMessage = client.GetAsync(url).Result;
                var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
                BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
                
                for(int i = 0; i < response.webPages.value.Length; i++)
                {                
                    var webPage = response.webPages.value[i];
                    
                    Console.WriteLine("name: " + webPage.name);
                    Console.WriteLine("url: " + webPage.url);                
                    Console.WriteLine("displayUrl: " + webPage.displayUrl);
                    Console.WriteLine("snippet: " + webPage.snippet);
                    Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
                    Console.WriteLine();
                }            
            }
        }
    
        public class BingCustomSearchResponse
        {        
            public string _type{ get; set; }            
            public WebPages webPages { get; set; }
        }
    
        public class WebPages
        {
            public string webSearchUrl { get; set; }
            public int totalEstimatedMatches { get; set; }
            public WebPage[] value { get; set; }        
        }
    
        public class WebPage
        {
            public string name { get; set; }
            public string url { get; set; }
            public string displayUrl { get; set; }
            public string snippet { get; set; }
            public DateTime dateLastCrawled { get; set; }
            public string cachedPageUrl { get; set; }
            public OpenGraphImage openGraphImage { get; set; }        
        }
        
        public class OpenGraphImage
        {
            public string contentUrl { get; set; }
            public int width { get; set; }
            public int height { get; set; }
        }
    }
    ```
6. Skapa programmet med följande kommando. Observera dll-sökväg som refereras av kommandoutdata.

    <pre>
    dotnet build 
    </pre>
7. Kör programmet med följande kommando ersätter **SÖKVÄGEN till utdata** med sökvägen refererar till ett byggsteg.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Nästa steg
- [Konfigurera din värdbaserade användargränssnitt](./hosted-ui.md)
- [Använda decoration markörer för att markera text](./hit-highlighting.md)
- [Sidan webbsidor](./page-webpages.md)

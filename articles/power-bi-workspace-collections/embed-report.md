---
title: Bädda in en rapport i Azure Power BI-Arbetsytesamlingar | Microsoft Docs
description: Lär dig hur du bäddar in en rapport som är i Power BI-Arbetsytesamlingar i ditt program.
services: power-bi-embedded
author: markingmyname
ROBOTS: NOINDEX
ms.assetid: ''
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.author: maghan
ms.openlocfilehash: 94476486ed87662f3d6b989b8d5360dd792f8824
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041188"
---
# <a name="embed-a-report-in-power-bi-workspace-collections"></a>Bädda in en rapport i Power BI-Arbetsytesamlingar

Lär dig hur du bäddar in en rapport som är i Power BI-Arbetsytesamlingar i ditt program.

> [!IMPORTANT]
> Power BI-arbetsytesamlingar fasas ut och är tillgänglig till juni 2018 eller det som anges i ditt avtal. Du uppmanas att planera migreringen till Power BI Embedded för att undvika avbrott i programmet. Information om hur du migrerar dina data till Power BI Embedded finns i [Migrera Power BI-arbetsytesamlingar till Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

Vi ska titta på hur du bäddar in en rapport i ditt program. Detta förutsatt att du redan har en rapport som finns i en arbetsyta i din arbetsytesamling. Om du inte gjort det steget ännu, se [Kom igång med Power BI-Arbetsytesamlingar](get-started.md).

Du kan använda .NET (C#) eller Node.js SDK tillsammans med JavaScript, för att enkelt skapa ditt program med Power BI-Arbetsytesamlingar.

## <a name="using-the-access-keys-to-use-rest-apis"></a>Använde åtkomstnycklarna för att använda REST API: er

Du kan skicka åtkomstnyckel som du kan hämta från Azure portal för en viss arbetsyta-samling för att kunna anropa REST API. Mer information finns i [Kom igång med Power BI-Arbetsytesamlingar](get-started.md).

## <a name="get-a-report-id"></a>Hämta ett rapport-ID

Varje åtkomst-token är baserad på en rapport. Du behöver att hämta viss rapport-id för den rapport som du vill bädda in. Detta kan göras baserat på anrop till den [hämta rapporter](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API. Detta returnerar rapport-id och bädda in URL: en. Detta kan göras med hjälp av Power BI .NET SDK eller anropa REST API direkt.

### <a name="using-the-power-bi-net-sdk"></a>Med Power BI .NET SDK

När du använder .NET SDK, som du behöver skapa en token autentiseringsuppgift som är baserat på åtkomstnyckeln som du får från Azure-portalen. Detta kräver att du installerar den [Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi).

**Installation av NuGet-paketet**

```
Install-Package Microsoft.PowerBI.Api
```

**C#-kod**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select the report that you want to work with from the collection of reports.
```

### <a name="calling-the-rest-api-directly"></a>Anropa REST API direkt

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response to get the report you want to work with.
    }

}
```

## <a name="create-an-access-token"></a>Skapa en åtkomsttoken

Power BI-Arbetsytesamlingar Använd inbäddningstokens som HMAC signeras JSON Web token. Token är signerade med åtkomstnyckeln från din Power BI-Arbetsytesamling. Inbäddningstokens som standard, som används för att ge skrivskyddad åtkomst till en rapport för att bädda in i ett program. Bädda in token utfärdas för en viss rapport och ska associeras med en inbäddad URL.

Åtkomsttoken ska skapas på servern som åtkomstnycklar används för att logga/kryptera token. Information om hur du skapar en åtkomst-token finns i [autentisering och auktorisering med Power BI-Arbetsytesamlingar](app-token-flow.md). Du kan också granska den [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod. Här är ett exempel på hur detta skulle se ut med .NET SDK för Power BI.

Du kan använda rapport-ID som du hämtade tidigare. När en inbäddningstoken har skapats använder du sedan snabbtangenten för att generera token som du kan använda javascript-perspektiv. Den *PowerBIToken klass* kräver att du installerar den [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**Installation av NuGet-paketet**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-kod**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-to-embed-tokens"></a>Att lägga till behörighetsomfattningar för att bädda in token

När du använder inbäddningstoken kan du begränsa användningen av de resurser som du ger åtkomst till. Därför kan du generera en token med begränsade behörigheter. Mer information finns i [omfång](app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Bädda in med hjälp av JavaScript

När du har den åtkomst-token och rapport-ID, kan vi bädda in rapporten med JavaScript. Detta kräver att du installerar NuGet [Power BI JavaScript-paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). EmbedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Du kan använda den [JavaScript Rapportinbäddningsexemplet](https://microsoft.github.io/PowerBI-JavaScript/demo/) att testa funktionerna. Du får också kodexempel för olika åtgärder som är tillgängliga.

**Installation av NuGet-paketet**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-kod**

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-the-size-of-embedded-elements"></a>Ange storleken på inbäddade elementen

Rapporten bäddas automatiskt baserat på storleken på dess behållare. Om du vill åsidosätta standardstorleken för det inbäddade objektet, lägger du till en CSS-klass-attributet eller infogade-format för bredd och höjd.

## <a name="see-also"></a>Se också

[Komma igång med exemplet](get-started-sample.md)  
[Autentisering och auktorisering i Power BI-arbetsytesamlingar](app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript-paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI-CSharp Git-lagringsplats](https://github.com/Microsoft/PowerBI-CSharp)  
[Power BI-noden Git-lagringsplats](https://github.com/Microsoft/PowerBI-Node)  

Fler frågor? [Försök med Power BI Community](http://community.powerbi.com/)

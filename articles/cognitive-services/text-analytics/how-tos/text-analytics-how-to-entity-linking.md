---
title: Använda igenkänning av entiteter med API för textanalys
titleSuffix: Azure Cognitive Services
description: Lär dig hur du känner igen entiteter med hjälp av den REST API för textanalys.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: b2916e5c414562c55c35c9c5e7ab378963e004be
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248082"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Hur du använder med namnet Entitetsidentifiering i Text Analytics (förhandsversion)

Den [entitet-API: T](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) tar ostrukturerad text och för varje JSON-dokument, returnerar du en lista över skiljas åt entiteter med länkar till mer information på webben (Wikipedia och Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Entitetslänkning och igenkänning av namngivna entiteter

Text Analytics `entities` endpoint supprts både med namnet entitetsidentifiering (NER) och entitetslänkning.

### <a name="entity-linking"></a>Entity Linking
Entitetslänkning är möjligheten att identifiera och disambiguate identiteten för en entitet som påträffats i texten (t.ex. avgöra om ”Mars” används som arrangemang eller latinska krigsguden). Den här processen kräver förekomsten av en knowledge base som känns igen entiteter är länkade – Wikipedia används som kunskapsbas för den `entities` endpoint textanalys.

I textanalys [Version 2.1-Preview](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634), endast entitetslänkning är tillgänglig.

### <a name="named-entity-recognition-ner"></a>Igenkänning av namngivna entiteter (NER)
Med namnet entitetsidentifiering är (NER) möjligheten att identifiera olika enheter i text och kategorisera dem i fördefinierade klasser. Klasserna stöds av entiteter visas nedan.

Text Analytics Version 2.1 förhandsversion (`https://[region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`), både entitetslänkning och igenkänning av namngivna entiteter (NER) är tillgängliga.

### <a name="language-support"></a>Stöd för språk

Med hjälp av entitetslänkning på olika språk kräver en motsvarande kunskapsbas på varje språk. För entitetslänkning i Text Analytics, det innebär att alla språk som stöds av den `entities` endpoint länkar till motsvarande Wikipedia-Kristi på det språket. Eftersom storleken på korpus varierar mellan olika språk, förväntas varierar även funktionens återkallande för entitetslänkning.

## <a name="supported-types-for-named-entity-recognition"></a>Typer som stöds för igenkänning av namngivna entiteter

| Typ  | Undertyp | Exempel |
|:-----------   |:------------- |:---------|
| Person        | EJ TILLÄMPLIGT\*         | ”Jeff”, ”Bill Gates”     |
| Plats      | EJ TILLÄMPLIGT\*         | ”Redmond, Washington”, ”Paris”  |
| Organisation  | EJ TILLÄMPLIGT\*         | ”Microsoft”   |
| Antal      | Tal        | ”6”, ”sex”     | 
| Antal      | Procent    | ”50%”, ”50 procent”| 
| Antal      | Ordningstal       | ”2”, ”andra”     | 
| Antal      | NumberRange   | ”4 till 8”     | 
| Antal      | Ålder           | ”90 dagar gamla”, ”30 år”    | 
| Antal      | Valuta      | ”$10,99”     | 
| Antal      | Dimension     | ”10 miles”, ”40 cm”     | 
| Antal      | Temperatur   | ”32 grader”    |
| DateTime      | EJ TILLÄMPLIGT\*         | ”18:30:00 den 4 februari 2012”      | 
| DateTime      | Date          | ”Den 2 maj 2017”, ”2017-05-02”   | 
| Tidpunkt     | Tid          | ”8 am”, ”8:00”  | 
| DateTime      | DateRange     | ”2 maj till 5 maj”    | 
| DateTime      | timeRange     | ”18: 00 till 19: 00”     | 
| DateTime      | Varaktighet      | ”1 minut och 45 sekunder”   | 
| DateTime      | Ange           | ”varje tisdag”     | 
| DateTime      | TimeZone      |    | 
| URL           | EJ TILLÄMPLIGT\*         | "http://www.bing.com"    |
| E-post         | EJ TILLÄMPLIGT\*         | "support@contoso.com" |
\* Beroende på indata- och extraherade entiteter, vissa entiteter kan utelämna den `SubType`.



## <a name="preparation"></a>Förberedelse

Du måste ha JSON-dokument i det här formatet: id-, text-, språk

Språk som stöds för närvarande finns i [listan](../text-analytics-supported-languages.md).

Dokumentstorlek måste vara under 5 000 tecken per dokument och du kan ha upp till 1 000 objekt (ID) per samling. Samlingen har skickats i brödtexten i begäran. I följande exempel är en illustration av innehåll som du kan skicka till länkramverk entitetsänden.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Steg 1: Strukturera begäran

Information om begäran-definition finns i [hur du anropar API för textanalys](text-analytics-how-to-call-api.md). Följande punkter har omarbetats för att underlätta:

+ Skapa en **POST** begäran. Läs API-dokumentationen för denna begäran: [API för Entity Linking](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Ange HTTP-slutpunkt för extrahering av diskussionsämne. Det måste innehålla den `/entities` resursen: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

+ Ange en rubrik för begäran att inkludera åtkomstnyckeln för Text Analytics-åtgärder. Mer information finns i [hur du hittar slutpunkter och åtkomstnycklar](text-analytics-how-to-access-key.md).

+ Ange JSON-dokument samlingen som du har förberett för den här analysen i begärandetexten,

> [!Tip]
> Använd [Postman](text-analytics-how-to-call-api.md) eller öppna den **API testkonsolen** i den [dokumentation](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) att strukturera en begäran och skicka den till tjänsten.

## <a name="step-2-post-the-request"></a>Steg 2: Publicera begäran

Analysen utförs vid mottagning av begäran. Tjänsten accepterar upp till 100 begäranden per minut. Varje begäran kan vara högst 1 MB.

Kom ihåg att tjänsten är tillståndslösa. Inga data lagras i ditt konto. Resultaten returneras omedelbart i svaret.

## <a name="step-3-view-results"></a>Steg 3: Visa resultat

Alla POST-förfrågningar returnerar en JSON-formaterad svar med ID: N och identifierade egenskaper.

Utdata returneras direkt. Du kan strömma resultaten till ett program som stöder JSON eller spara utdata till en fil på den lokala datorn och sedan importera den till ett program som gör att du kan sortera, söka och manipulera data.

Ett exempel på utdata för entitetslänkning visas nästa:

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>Sammanfattning

I den här artikeln beskrivs begrepp och arbetsflöde för entitetslänkning med textanalys i Cognitive Services. Sammanfattningsvis:

+ [Entiteter API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) är tillgänglig för valda språken.
+ JSON-dokument i begärandetexten innehålla ett id, text och språk-kod.
+ POST-begäran är att en `/entities` slutpunkten, med hjälp av en personligt anpassad [åtkomst till nyckeln och en slutpunkt](text-analytics-how-to-access-key.md) som är giltig för prenumerationen.
+ Svarsutdata som består av länkade entiteter (inklusive säker poäng, förskjutningar och webblänkar, för varje dokument ID) kan användas i alla program

## <a name="see-also"></a>Se också 

 [Översikt över Textanalys](../overview.md)  
 [Vanliga frågor och svar (FAQ)](../text-analytics-resource-faq.md)</br>
 [Text Analytics produktsida](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [API för textanalys](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)

---
title: Bild Moderering - Content Moderator
titlesuffix: Azure Cognitive Services
description: Använda bildmoderering för att ändra olämplig avbildningar
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: 6c5fed78c67f974a2af11efd133e9a79ec52124b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219659"
---
# <a name="image-moderation"></a>Bildmoderering

Använd Content Moderator datorstödd bildmoderering och [verktyg för mänsklig granskning](Review-Tool-User-Guide/human-in-the-loop.md) till måttlig avbildningar för vuxet eller olämpligt innehåll. Genomsök bilder för textinnehåll och extrahera texten och spåra ansikten. Du kan matcha avbildningar mot anpassade listor och vidta några ytterligare åtgärder.

## <a name="evaluating-for-adult-and-racy-content"></a>Utvärdera för vuxet eller olämpligt innehåll

Den **utvärdera** åtgärden returnerar ett förtroenderesultat mellan 0 och 1. Den också returnerar booleska data lika med SANT eller FALSKT. Dessa värden förutsäga om avbildningen innehåller potentiellt vuxet eller olämpligt innehåll. När du anropar API: et med din avbildning (fil eller URL), innehåller returnerade svaret följande information:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]

> - `isImageAdultClassified` representerar potentiella förekomsten av avbildningar som kan uppfattas som sexuellt explicit eller är olämpligt för barn i vissa situationer.
> - `isImageRacyClassified` representerar potentiella förekomsten av avbildningar som kan uppfattas som sexuellt något eller mogen i vissa situationer.
> - Poängen är mellan 0 och 1. Ju högre poäng desto högre modellen är att förutsäga att kategorin kan användas. Den här förhandsversionen är beroende av en statistisk modell i stället för manuellt kodade resultat. Vi rekommenderar att du testar med ditt eget innehåll att avgöra hur varje kategori stämmer överens med dina behov.
> - Booleska värden är true eller false beroende på den interna poängen tröskelvärden. Kunder bör utvärdera om du vill använda det här värdet eller välja anpassade tröskelvärden baserat på deras innehåll principer.
>

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Identifiera text med optisk teckenläsning (OCR)

Den **optisk teckenläsning (OCR)** igen kan du förutsäga förekomsten av textinnehåll i en bild och extraherar för textmoderering, bland annat. Du kan ange språket. Om du inte anger ett språk som standard identifieringen till engelska.

Svaret innehåller följande information:
- Den ursprungliga texten.
- Identifierade textelement med sina förtroende poäng.

Exempel extrahera:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Identifiera ansikten

Identifiera ansikten hjälper till att identifiera personligt identifierbar information (PII), till exempel ansikten i bilder. Du kan identifiera potentiella ansikten och antalet potentiella ansikten i varje avbildning.

Svaret innehåller den här informationen:

- Antal ansikten
- Lista över platser för ansikten har identifierats

Exempel extrahera:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Skapa och hantera anpassade listor

I många online communities, när användare ladda upp bilder eller annan typ av innehåll, kan stötande objekt hämta delade flera gånger över följande dagar, veckor och månader. Kostnaderna för upprepade gånger genomsökning och filtrera bort samma bild eller marginellt ändrade versioner av avbildningen från flera platser kan vara dyrt och felbenägna.

I stället för att kontrollera flera gånger för samma avbildning, du lägger till stötande bilder till din egen lista över blockerade innehåll. På så sätt kan systemet innehållsmoderering jämför inkommande bilder mot dina anpassade listor och stoppar vidare bearbetning.

> [!NOTE]
> Det finns en maxgräns på **5 bild listor** med varje lista till **inte överstiga 10 000 bilder**.
>

Content Moderator tillhandahåller ett komplett [Image listan Management API](try-image-list-api.md) med åtgärder för att hantera en lista över anpassade avbildningar. Börja med den [bild visar en lista över API-konsol](try-image-list-api.md) och använda REST API-kodexempel. Se även de [Snabbstart för avbildning lista .NET](image-lists-quickstart-dotnet.md) om du är bekant med Visual Studio och C#.

## <a name="matching-against-your-custom-lists"></a>Matchning mot dina anpassade listor

Matchning-åtgärden tillåter partiell matchning av inkommande bilder mot någon av dina anpassade listor, skapas och hanteras med hjälp av åtgärderna i listan.

Om en matchning hittas, returnerar åtgärden identifierare och moderering taggar för matchade avbildningen. Svaret innehåller den här informationen:

- Matcha poäng (mellan 0 och 1)
- Matchade bild
- Taggar (anges under föregående moderering)
- Bild-etiketter

Exempel extrahera:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>Verktyg för mänsklig granskning

För mer nyanserade fall använda Content Moderator [granskningsverktyget](Review-Tool-User-Guide/human-in-the-loop.md) och dess API till att visa resultaten och innehåll i granskningen för din mänskliga moderatorer. De granska datorn tilldelade taggar och bekräfta sina slutliga beslut.

![Bild granskning för mänskliga moderatorer](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Nästa steg

Testkör den [Bildmoderering API-konsol](try-image-api.md) och använda REST API-kodexempel. Se även de [bildmoderering .NET Snabbstart](image-moderation-quickstart-dotnet.md) om du är bekant med Visual Studio och C#.

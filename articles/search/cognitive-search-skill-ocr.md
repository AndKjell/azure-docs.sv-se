---
title: OCR kognitiv sökning färdighet (Azure Search) | Microsoft Docs
description: Extrahera text från bildfiler i en Azure Search berikande pipeline.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 97d594a232c3576d0a0163b2d6847f06328bcd7b
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167519"
---
# <a name="ocr-cognitive-skill"></a>OCR kognitiva kunskaper

Den **OCR** färdighet extraherar text från bildfiler. Filformat som stöds är:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF

> [!NOTE]
> Kognitiv sökning är tillgängligt som en förhandsversion. Kompetens körning och extrahering av avbildningen och normalisering är för närvarande erbjuds kostnadsfritt. Vid ett senare tillfälle meddelas priserna för dessa funktioner. 

## <a name="skill-parameters"></a>Färdighet parametrar

Parametrar är skiftlägeskänsliga.

| Parameternamn     | Beskrivning |
|--------------------|-------------|
| detectOrientation | Aktiverar automatisk igenkänning av bildorientering. <br/> Giltiga värden: true / false.|
|defaultLanguageCode | <p>  Språkkod för den inmatade texten. Språk som stöds: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>CS (Tjeckiska) <br/>da (danska) <br/>NL (nederländska) <br/>en (på engelska) <br/>Fi (finska)  <br/>fr (franska) <br/>  Tyskland (tyska) <br/>el (grekiska) <br/> HU (ungerska) <br/> den (italienska) <br/>  Ja (japanska) <br/> Ko (koreanska) <br/> NB (norska) <br/>   PL (polska) <br/> PT (brasiliansk) <br/>  RU (ryska) <br/>  ES (spanska) <br/>  SA (svenska) <br/>  TR (turkiska) <br/> Kundreskontra (arabiska) <br/> ro (rumänska) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  Sk (slovakiska). <br/>  UNK (okänd) <br/><br/> Om språkkoden är Ospecificerad eller null, är språket autodetected. </p> |
| textExtractionAlgorithm | ”ut” eller ”handskriven”. Algoritmen ”handskriven” text igenkänning av OCR förhandsvisas just nu och stöds endast på engelska. |

## <a name="skill-inputs"></a>Färdighet indata

| Indatanamnet      | Beskrivning                                          |
|---------------|------------------------------------------------------|
| image         | Komplexa typen. För närvarande bara fungerar med ”/ dokument/normalized_images” fältet genereras av Azure Blob-indexeraren när ```imageAction``` är inställd på ```generateNormalizedImages```. Se den [exempel](#sample-output) för mer information.|


## <a name="skill-outputs"></a>Färdighet utdata
| Namn på utdata     | Beskrivning                   |
|---------------|-------------------------------|
| text          | Oformaterad text som extraherats från avbildningen.   |
| layoutText    | Komplex typ som beskriver den extrahera texten samt den plats där texten hittades.|


## <a name="sample-definition"></a>Exempeldefinition

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Exempel på text och layoutText utdata

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Exempel: Sammanslagning av text som extraherats från inbäddade bilder med innehållet i dokumentet.

Ett vanligt användningsfall för Text fusion är möjligheten att slå samman textrepresentation av bilder (text från en OCR-kunskaper eller rubriken på en avbildning) i fältet content för ett dokument. 

Följande exempel kompetens skapar en *merged_text* fält som innehåller det faktiska innehållet i dokumentet, samt OCRed texten från var och en av avbildningarna som är inbäddad i detta dokument. 

#### <a name="request-body-syntax"></a>Begärandetextsyntax
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
Exemplet ovan kompetens förutsätter att det finns ett normaliserat avbildningar fält. Om du vill generera det här fältet, ange den *imageAction* konfiguration i din indexerarens definition till *generateNormalizedImages* enligt nedan:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Se också
+ [Fördefinierade kunskaper](cognitive-search-predefined-skills.md)
+ [TextMerger färdighet](cognitive-search-skill-textmerger.md)
+ [Hur du definierar en kompetens](cognitive-search-defining-skillset.md)
+ [Skapa indexerare (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

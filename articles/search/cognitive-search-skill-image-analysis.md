---
title: Bild Analysis kognitiv sökning färdighet (Azure Search) | Microsoft Docs
description: Extrahera semantiska text genom bildanalys med ImageAnalysis kognitiva kunskaper i en Azure Search berikande pipeline.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 8ec3b6c5dfdd63de45e287cf0b68e90c7b0cbbd8
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48829554"
---
#   <a name="image-analysis-cognitive-skill"></a>Bild Analysis kognitiva kunskaper

Den **bildanalys** färdighet extraherar en omfattande uppsättning visuella funktioner baserat på innehållet i. Du kan till exempel generera en etikett från en avbildning, generera taggar eller identifiera kändisar och landmärken.

> [!NOTE]
> Kognitiv sökning är tillgängligt som en förhandsversion. Kompetens körning och extrahering av avbildningen och normalisering är för närvarande erbjuds kostnadsfritt. Vid ett senare tillfälle meddelas priserna för dessa funktioner. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Färdighet parametrar

Parametrar är skiftlägeskänsliga.

| Parameternamn     | Beskrivning |
|--------------------|-------------|
| defaultLanguageCode   |  En sträng som anger språket som ska returneras. Tjänsten returnerar resultat i ett angivet språk. Om den här parametern inte anges är standardvärdet ”SV”. <br/><br/>Språk som stöds är: <br/>*en* -engelska (standard) <br/> *zh* -kinesiska (förenklad)|
|visualFeatures |   En matris med strängar som anger vilka visuell funktion som ska returneras. Giltigt visual funktionen typer är:  <ul><li> *kategorier* -kategoriserar bildinnehåll enligt en taxonomi som definierats i Cognitive Services [dokumentation](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *taggar* -taggar avbildningen med en detaljerad lista över ord som rör innehållet i.</li><li>*Beskrivning av* -beskriver bilden innehåll med en hel engelska mening.</li><li>*Ansikten* – identifierar om ansikten finns. Om det finns genererar koordinater, kön och ålder.</li><li> *ImageType* – identifierar om bild är clipart eller en Linjeteckning.</li><li>   *Färg* -anger accentfärg, dominant färg, och om en bild är svart och vit.</li><li>*Vuxet* – identifierar om avbildningen pornografiskt sin natur (visar nakenhet eller en kön act). Sexuellt något innehåll identifieras också.</li></ul> Namnen på visuella funktioner är skiftlägeskänsliga.|
| detaljer   | En matris med strängar som anger vilka domänspecifika information för att returnera. Giltigt visual funktionen typer är: <ul><li>*Kändisar* -identifierar kändisar om har identifierats i avbildningen.</li><li>*Landmärken* -identifierar landmärken om har identifierats i avbildningen.</li></ul>
 |

## <a name="skill-inputs"></a>Färdighet indata

| Indatanamnet      | Beskrivning                                          |
|---------------|------------------------------------------------------|
| image         | Komplexa typen. För närvarande bara fungerar med ”/ dokument/normalized_images” fältet genereras av Azure Blob-indexeraren när ```imageAction``` är inställd på ```generateNormalizedImages```. Se den [exempel](#sample-output) för mer information.|



##  <a name="sample-definition"></a>Exempeldefinition
```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "context": "/document/normalized_images/*",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
        }
    ]
}
```

##  <a name="sample-input"></a>Exempelindata

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {                
                "image":  {
                               "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                               "width": 500,
                               "height": 300,
                               "originalWidth": 5000,  
                               "originalHeight": 3000,
                               "rotationFromOriginal": 90,
                               "contentOffset": 500  
                           }
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Exempel på utdata

```json
{
    "values": [
      {
        "recordId": "1",
            "data": {
                "categories": [
           {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                "name": "people_",
                        "score": 0.83984375,
                "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceBoundingBox": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                    "faceBoundingBox": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBwImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
           }
      }
    ]
}
```


## <a name="error-cases"></a>Felhändelser
I följande fel fall extraheras inga element.

| Felkod | Beskrivning |
|------------|-------------|
| NotSupportedLanguage | Det angivna språket stöds inte. |
| InvalidImageUrl | Bild-URL är felaktigt formaterat eller kan inte nås.|
| InvalidImageFormat | Indata är inte en giltig bild. |
| InvalidImageSize | Inkommande bilden är för stor. |
| NotSupportedVisualFeature  | Typ av angivna funktionen är inte giltig. |
| NotSupportedImage | Stöds inte bilden, till exempel barnpornografi. |
| InvalidDetails | Domänspecifika-modell som inte stöds. |

## <a name="see-also"></a>Se också

+ [Fördefinierade kunskaper](cognitive-search-predefined-skills.md)
+ [Hur du definierar en kompetens](cognitive-search-defining-skillset.md)
+ [Skapa indexerare (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

---
title: Kategorisera bilder - visuellt innehåll
titleSuffix: Azure Cognitive Services
description: Begrepp för att kategorisera bilder med hjälp av den API för visuellt innehåll.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 602ea8028cf89b23df692d5c2fb9b781f64bcad4
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341692"
---
# <a name="categorizing-images"></a>Kategorisera bilder

Förutom taggning och beskrivningar returnerar för visuellt innehåll taxonomi-baserade kategorier som definierats i tidigare versioner. Kategorierna är ordnade som en taxonomi med överordnad/underordnad ärftliga hierarkier. Alla kategorier är på engelska. De kan användas fristående eller med vår nya taggning modeller.

## <a name="the-86-category-concept"></a>Konceptet 86-kategori

Baserat på en lista över 86 begrepp som visas i följande diagram, kan en bild kategoriseras sträcker sig från bred till specifika. Läs den fullständiga taxonomifält i textformat [Kategoritaxonomi](category-taxonomy.md).

![Analysera kategorier](./Images/analyze_categories.png)

## <a name="image-categorization-examples"></a>Bild kategorisering exempel

Följande JSON-svar visar vad för visuellt innehåll returnerar när kategorisera exempelbild baserat på dess visuella funktioner.

![Kvinna tak](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

I följande tabell visas en typisk bild-uppsättning och den kategori som returneras av visuellt innehåll för varje bild.

| Bild | Kategori |
|-------|----------|
| ![Programvarufamiljer foto](./Images/family_photo.png) | people_group |
| ![Söta hund](./Images/cute_dog.png) | animal_dog |
| ![Utomhus Mountain](./Images/mountain_vista.png) | outdoor_mountain |
| ![Visuellt innehåll analyserar matbröd](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Nästa steg

Lär dig begrepp [tagga bilder](concept-tagging-images.md) och [som beskriver avbildningar](concept-describing-images.md).
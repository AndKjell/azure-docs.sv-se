---
title: Slutpunkt för entitetsökning i Bing
titlesuffix: Azure Cognitive Services
description: Sammanfattning av Entitetssökning i Bing-slutpunkten.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: d781a4b3cd0119f5624b4dd20b514894ea339414
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816590"
---
# <a name="entity-search-endpoint"></a>Entiteten Search slutpunkt
Den **Entitetssökning** innehåller en slutpunkt som returnerar entiteter från webben baserat på en fråga.

## <a name="endpoint"></a>Slutpunkt
Att hämta entiteten resultat med hjälp av den **API för Bing**, skicka en `GET` begäran till följande slutpunkt. Använda rubriker och URL-parametrar för att definiera ytterligare specifikationer.

**Slutpunkt:** returnerar entiteter som är relevanta för användarens sökfråga som definieras av `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Mer information om huvuden, parametrar, marknaden koder, svarsobjekt, fel, o.s.v., se den [Entitetssökning i Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) referens.

## <a name="response-json"></a>Svaret JSON
Svaret på en entitet sökbegäran inkluderar resultat som JSON-objekt. Exempel på resultaten finns [börjar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Nästa steg
Den **Bing** API: er stöder sökåtgärder som returnerar resultat enligt typ. Alla Sök slutpunkter returnerar resultat som JSON-svar-objekt.  Alla slutpunkter stöd för frågor som returnerar ett visst språk och/eller plats genom att longitud, latitud och Sök radius.

Fullständig information om de parametrar som stöds av varje slutpunkt finns referenssidor för varje typ av.
Exempel med hjälp av API för entitetssökning finns [börjar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) och [ändrar storlek och beskärning miniatyrbilder](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).
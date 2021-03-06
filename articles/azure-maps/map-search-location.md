---
title: Visa sökresultat med Azure Maps | Microsoft Docs
description: Hur du utför en sökbegäran med Azure Maps och sedan visa resultaten på en Javascript-karta
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 78ffa42bcf57b7163afc13b2550abdbae240ef00
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729249"
---
# <a name="show-search-results-on-the-map"></a>Visa sökresultat på kartan

Den här artikeln visar hur du söker efter plats i närheten och visa sökresultatet på kartan.

Det finns två sätt att söka efter en plats i närheten. Ett sätt är att använda en tjänst-modul för att göra en sökbegäran. Det andra sättet är att göra en sökbegäran via en [XMLHttpRequest](https://xhr.spec.whatwg.org/) till [Azure Maps Fuzzy-sökning API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Båda metoderna beskrivs nedan.

## <a name="make-a-search-request-via-service-module"></a>Gör en sökbegäran via tjänstemodulen

<iframe height='500' scrolling='no' title='Visa sökresultat på en karta (Tjänstemodulen)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Se pennan <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>visa sökresultat på en karta (Tjänstemodulen)</a> genom Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) på <a href='https://codepen.io'>CodePen</a>.
</iframe>

Det första kodblocket konstruerar ett Kartobjekt och initierar klienttjänsten. Du kan se [skapa en karta](./map-create.md) anvisningar.

Andra kodblocket använder den [Azure Maps Fuzzy Search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) att söka efter orienteringspunkt. Fuzzy-sökning API kan hantera valfri kombination av fuzzy indata. Svaret från tjänsten fuzzy-sökning parsas sedan till GeoJSON-format med den [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) metod. De PIN-koderna läggs sedan till kartan för att visa orienteringspunkter på kartan.

Senaste kodblocket justerar kamera gränser för kartan med hjälp av kartans [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) egenskapen.

## <a name="make-a-search-request-via-xmlhttprequest"></a>Gör en sökbegäran via XMLHttpRequest

<iframe height='500' scrolling='no' title='Visa sökresultat på en karta' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Se pennan <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>visa sökresultat på en karta</a> genom Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) på <a href='https://codepen.io'>CodePen</a>.
</iframe>

Det första kodblocket skapar en Kartobjekt. Du kan se [skapa en karta](./map-create.md) anvisningar.

Andra kodblocket lägger till search resultat lager på kartan. Lagret för search-resultat visas sökresultaten som PIN-koder, på kartan. PIN-koder har lagts till med [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

Det tredje kodblocket skickar en [XMLHttpRequest](https://xhr.spec.whatwg.org/) till [Azure Maps Fuzzy-sökning API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) att söka efter orienteringspunkt. Fuzzy-sökning API kan hantera valfri kombination av fuzzy indata.

Senaste kodblocket Parsar svaret och anpassar den justerar kamera gränser för kartan med hjälp av kartans [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) att rendera resultatet PIN-koder.

## <a name="next-steps"></a>Nästa steg

Läs mer om **Fuzzy-sökning**:

> [!div class="nextstepaction"]
> [Azure Maps-API: T för Fuzzy Search](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Läs mer om de klasser och metoder som används i den här artikeln:

> [!div class="nextstepaction"]
> [Karta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Se följande artiklar för fullständig kodexempel:

> [!div class="nextstepaction"]
> [Hämta information från en koordinat](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Visa riktningar från A till B](./map-route.md)

---
title: Standardkonfigurationen för konversationen Learner - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Läs mer om Konversationsdeltagare standardkonfigurationen.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b048509f38356b19d52507f4dfb5bfa61122f9f6
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576433"
---
# <a name="default-values-and-boundaries"></a>Standardvärden och gränser

Det här dokumentet beskriver standardkonfigurationen av Konversationsdeltagare och tjänstens gränser.

## <a name="default-configuration"></a>Standardkonfiguration

Parameter | Standardvärde
--- | --- 
Tidsgräns för session standard | 30 minuter

## <a name="boundaries"></a>Gränser

Parameter | Gräns
--- | --- 
Redigera API max HTTP-anrop per månad | 5M
Redigerar API, max HTTP-anrop per sekund | 25
Sessionen API, max HTTP-anrop per månad | 500 000
Sessionen API, max HTTP-anrop per sekund | 10
Maxantal egna (icke-programmässiga) entiteter per modell | Se [LUIS gränser doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); i praktiken faktiska antalet kan vara något mindre
Maxantal färdiga entiteter per modell | Se [LUIS gränser dokument](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Maxantal entiteter (i totalt) per modell | 100
Maxantal åtgärder per modell | 32
Maxantal träna dialogrutor per modell | 1000
Maximalt antal användare aktiverar per träna dialogrutan | 100
Maxantal log dialogrutor per modell | Inga förinställda gränsen, men log dialogrutor behålls endast för en fast period innan det tas bort.  Dessutom visar konversationen Learner Användargränssnittet 100 log-dialogrutor i taget. 
Maxantal modeller per användare | Ingen förinställda gräns
Maxantal sekventiella-wait-åtgärder | 5 (*)

(*) Alla åtgärder som icke-wait maskeras efter 5 sekventiella-wait-åtgärder och Konversationsdeltagare väljer bland tillgängliga vänta åtgärder.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Kom igång med Konversationsdeltagare](./quickstart.md)

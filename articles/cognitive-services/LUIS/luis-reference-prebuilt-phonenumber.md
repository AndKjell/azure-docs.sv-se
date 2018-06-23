---
title: THOMAS färdiga entiteter phone nummer referens - Azure | Microsoft Docs
titleSuffix: Azure
description: Den här artikeln innehåller fördefinierade entitetsinformation om telefonnummer i språk förstå (THOMAS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 0f72b807b9b0ec110a80d67babb1c45902b8c810
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321885"
---
# <a name="phonenumber-entity"></a>PhoneNumber entitet
Den `phonenumber` entiteten extraherar olika telefonnummer inklusive landskoden. Eftersom den här entiteten har redan tränats, behöver du inte lägga till exempel utterances för programmet. Den `phonenumber` entitet stöds i `en-us` endast kultur. 

## <a name="types-of-phonenumber"></a>Typer av telefonnummer
PhoneNumber hanteras från den [identifierare text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) Github-lagringsplatsen

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Lösning för färdiga phonenumber entitet
I följande exempel visas upplösning på **builtin.phonenumber** entitet.

```JSON
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Nästa steg

Lär dig mer om den [procentandel](luis-reference-prebuilt-percentage.md), [nummer](luis-reference-prebuilt-number.md), och [temperatur](luis-reference-prebuilt-temperature.md) entiteter. 
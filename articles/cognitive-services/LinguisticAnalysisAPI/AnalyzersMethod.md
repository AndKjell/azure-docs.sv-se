---
title: 'Analyzer-metod - API: T med Lingistic analys'
titlesuffix: Azure Cognitive Services
description: 'Analysverktyg REST API innehåller en lista över analysverktyg som för närvarande stöds av API: T för Lingvistisk analys.'
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 762ebf50999a88251dcd05824f2ed450cec97f04
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237407"
---
# <a name="analyzers-method"></a>Analyzer-metod

> [!IMPORTANT]
> Förhandsversionen av Lingvistisk analys har ur den 9 augusti 2018. Vi rekommenderar att du använder [Azure Machine Learning modulerna för textanalys](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) för text-bearbetning och analys.

Den **analysverktyg** REST API innehåller en lista över analysverktyg som för närvarande stöds av tjänsten.
Svaret innehåller sina [namn](Analyzer-Names.md) och de språk som stöds av varje (till exempel ”SV” för engelska).

## <a name="request-parameters"></a>Begäranparametrar
Ingen

<br>

## <a name="response-parameters"></a>Svarsparametrar
Namn | Typ | Beskrivning
-----|------|--------------
Språk | lista med strängar | lista över två bokstaven ISO språkkoder som som den här analyzer kan användas.
id   | sträng | unikt ID för den här analyzer
typ | sträng | bred typ av analyzer här
Specifikationen | sträng | namnet på den specifikation som används för den här analyzer
Implementering | sträng | Beskrivning av modellen och/eller algoritmen bakom den här analyzer

<br>
## <a name="example"></a>Exempel
Hämta /analyzers

Svar: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3",
        "implementation": "SplitMerge"
    },
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags",
        "specification": "PennTreebank3",
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens",
        "specification":"PennTreebank3",
        "implementation": "regexes"
    }
]
```

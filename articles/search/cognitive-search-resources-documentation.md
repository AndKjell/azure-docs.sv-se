---
title: Dokumentation för kognitiva search (Azure Search) | Microsoft Docs
description: En kommenterade lista över artiklar, självstudier, exempel och blogg bokför rör kognitiva Sök arbetsbelastningar i Azure Search.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: a96b66f61b19d218c5708a0ce967e0033ba26627
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/07/2018
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>Dokumentation för kognitiva Sök arbetsbelastningar

Kognitiva Sök nu är tillgänglig som förhandsversion, ett nytt berikande lager Azure Sökindexeringen som söker efter Latenta information i icke-text källor och omvandla det till fulltext sökbara innehåll i Azure Search odifferentierad text.

I följande artiklar är komplett dokumentation för kognitiva sökning.

## <a name="getting-started"></a>Komma igång
+ [Vad är kognitiva sökningen?](cognitive-search-concept-intro.md)
+ [Snabbstart: Försök kognitiva sökning i portalen](cognitive-search-quickstart-blob.md)
+ [: Självstudier kognitiva sökningen API: er](cognitive-search-tutorial-blob.md)
+ [Exempel: skapa en anpassad kunskap](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Vägledning
+ [Hur du definierar en kunskaper](cognitive-search-defining-skillset.md)
+ [Hur du refererar till anteckningar i en kunskaper](cognitive-search-concept-annotations-syntax.md)
+ [Mappning till ett index](cognitive-search-output-field-mapping.md)
+ [Hur du extrahera informationen från bilder](cognitive-search-concept-image-scenarios.md)
+ [Hur du skapar ett Azure Search index](search-howto-reindex.md)
+ [Hur du definierar ett gränssnitt för egna kunskaper](cognitive-search-custom-skill-interface.md)
+ [Felsökningstips](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referens

+ [Fördefinierade kunskaper](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergerSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [Förhandsgranska REST API](search-api-2017-11-11-preview.md)
  + [Skapa kunskaper (api-version = 2017-11-11-Preview)](ref-create-skillset.md)
  + [Skapa indexerare (api-version = 2017-11-11-Preview)](ref-create-indexer.md)

## <a name="see-also"></a>Se också

+ [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexerare i Azure Search](search-indexer-overview.md)
+ [Vad är Azure Search?](search-what-is-azure-search.md)
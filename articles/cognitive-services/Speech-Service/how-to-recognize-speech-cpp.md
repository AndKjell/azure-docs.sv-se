---
title: Känna igen tal med hjälp av tal-SDK för C++
titleSuffix: Microsoft Cognitive Services
description: >
  Lär dig att känna igen tal (från en fil från en mikrofon, med en anpassad modell, kontinuerligt eller one-shot) med hjälp av tal-SDK för C++.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 133bce7443f8cd9fce41446fef9f9247355e1125
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886424"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-c"></a>Känna igen tal med hjälp av tal-SDK för C++

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!INCLUDE [Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-cpp[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

[!INCLUDE [Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-cpp[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

[!INCLUDE [Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-cpp[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionWithFile)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Leta efter den kod som används i den här artikeln i mappen samples/cpp/windows /-konsolen.

## <a name="next-steps"></a>Nästa steg

- [Förstå avsikter från tal](how-to-recognize-intents-from-speech-cpp.md)
- [Så här att översätta tal](how-to-translate-speech-cpp.md)


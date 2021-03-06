---
title: Skala bearbetning av Media översikt | Microsoft Docs
description: Det här avsnittet är en översikt över skalning bearbetning av Media med Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: juliako
ms.openlocfilehash: 698a85244d5341224dd9f513c5617b9086e36844
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033081"
---
# <a name="scaling-media-processing-overview"></a>Bearbetning av Media översikt över skalning
Den här sidan ger en översikt över hur och varför att skala mediebearbetning. 

## <a name="overview"></a>Översikt
Ett Media Services-konto är kopplat till en typ av reserverad enhet som bestämmer hur snabbt mediebearbetningsuppgifter ska bearbetas. Du kan välja mellan följande typer av reserverade enheter: **S1**, **S2** och **S3**. Samma kodningsjobb körs till exempel snabbare om du använder typen **S2** än om du använder typen **S1**. Mer information finns i den [reserverade enhetstyper](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Förutom att ange typ av reserverad enhet kan ange du att etablera ditt konto med reserverade enheter. Antalet etablerade reserverade enheter anger antalet medieuppgifter som kan bearbetas samtidigt i en viss konto. Till exempel om ditt konto har fem reserverade enheter, och sedan fem medieuppgifter körs samtidigt så länge som det finns aktiviteter som ska bearbetas. De återstående aktiviteterna ska vänta i kön och ska få hämtas för bearbetning av sekventiellt när en aktivitet är klar. Om ett konto inte har några mediereserverade enheter etablerade sedan hämtas uppgifter sekventiellt. I det här fallet beror väntetiden mellan en uppgift slutförs och nästa start på tillgängligheten för resurser i systemet.

## <a name="choosing-between-different-reserved-unit-types"></a>Välja mellan olika reserverade enhetstyper
Tabellen nedan hjälper dig att fatta ett beslut när du väljer mellan olika kodning hastigheter. Det ger också några benchmark-fall på [en video som du kan hämta](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) att utföra egna test:

| Scenarier | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Avsedda användning |Enkel bithastighet kodning. <br/>Filer på SD eller under lösningar, tid inte känsliga, låg kostnad. |Enkel bithastighet och flera bithastigheter kodning.<br/>Normal användning för både SD och HD encoding. |Enkel bithastighet och flera bithastigheter kodning.<br/>Fullständig HD och 4K högupplöst video. Tid känsliga och snabbare arbetet kodning. |
| Prestandamått för 7 minuter långa videon |Koda till en enda bithastighet MP4-fil med samma upplösning tar cirka 5 minuter. |Kodning med ”H264, enkel bithastighet, 720p” förinställda tar cirka åtta minuter.<br/><br/>Kodning med ”H264, flera bithastigheter, 720p” förinställning tar cirka 16,8 minuter. |Kodning med ”H264, enkel bithastighet, 1080p” förinställda tar cirka 4 minuter.<br/><br/>Kodning med ”H264 Multibithastighet 1080p” förinställning tar cirka 8 minuter. |


## <a name="considerations"></a>Överväganden
> [!IMPORTANT]
> Gå igenom överväganden som beskrivs i det här avsnittet.  
> 
> 

* För analys av ljud och Videoanalys jobb som utlöses av Media Services v3 eller Video Indexer, rekommenderas starkt S3 enhetstyp.
* Om du använder den delade poolen, det vill säga har utan att några mediereserverade enheter sedan koda aktiviteterna samma prestanda som med mediereserverade S1-enheter. Men det finns ingen övre gräns för den tid som dina aktiviteter kan åtgärder i kö och högst endast en aktivitet kommer att köras vid en given tidpunkt.

## <a name="billing"></a>Fakturering

Du debiteras efter faktiska använda minuter av mediereserverade enheter. En detaljerad förklaring finns i avsnittet vanliga frågor och svar i den [prissättning för Media Services](https://azure.microsoft.com/pricing/details/media-services/) sidan.   

## <a name="quotas-and-limitations"></a>Kvoter och begränsningar
Läs om hur kvoter och begränsningar och hur du öppnar ett supportärende [kvoter och begränsningar](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Nästa steg
Få skalning media bearbetning uppgiften med någon av dessa tekniker: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Information om hur du hämtar den senaste versionen av Java SDK och börjar utveckla med Java finns i [Komma igång med Java-klient-SDK för Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Om du vill hämta den senaste PHP SDK för Media Services, leta efter version 0.5.7 av Microsoft/WindowAzure-paketet i [Packagist-databasen](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]


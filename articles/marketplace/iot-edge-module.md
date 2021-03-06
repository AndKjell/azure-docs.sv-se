---
title: Azure IoT Edge-moduler
description: IoT Edge-modulen erbjudandet om den på Azure Marketplace för appen och tjänsten utgivare.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, IoT Edge module offer
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/22/2018
ms.author: qianw211
ms.openlocfilehash: ecc892a38d5e86a089085cd67a78ce7d00c86fd8
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181124"
---
# <a name="iot-edge-modules"></a>IoT Edge-moduler

Den [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) plattform backas upp av Azure-molnet.  Den här plattformen kan du distribuera molnarbetsbelastningar köra direkt i IoT-enheter.  En IoT Edge-modul kan köra offline arbetsbelastningar och analyserar lokalt. Den här erbjudandetypen kan spara bandbredd och skydda lokala och känsliga data och du får med låg latens svarstid.  Nu har du alternativ för att dra nytta av de här färdiga arbetsbelastningarna. Endast ett fåtal från första part lösningar från Microsoft fram till nu var tillgängliga.  När du var du tvungen att investera tid och resurser till att skapa anpassade IoT-lösningar.

Genom att introducera den [IoT Edge-moduler i Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1), nu har vi ett enda mål för utgivare att exponera och sälja sina lösningar för IoT-målgrupp. IoT-utvecklare kan slutligen söka efter och köpa funktioner för att påskynda utvecklingen av sin lösning.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Viktiga fördelar med IoT Edge-moduler i Azure Marketplace:

| **För utgivare**    | **För kunder (IoT utvecklare)**  |
| :------------------- | :-------------------|
| Nå miljontals utvecklare som vill skapa och distribuera IoT Edge-lösningar.  | Skapa en IoT Edge-lösning med hög exakthet för att använda säker och testade komponenter. |
| Publicera en gång och kör över alla IoT Edge-maskinvara som stöder behållare. | Minska tiden till marknaden genom att söka efter och distribuera 1 och 3 part IoT Edge-moduler för specifika behov. |
| Tjäna pengar med flexibla faktureringsalternativ <ul> <li> Kostnadsfria tjänster och Använd din egen licens (BYOL). </li> </ul> | Gör inköp med ditt val av faktureringsmodellerna. <ul> <li> Kostnadsfria tjänster och Använd din egen licens (BYOL). </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>Vad är en IoT Edge-modul?

Azure IoT Edge kan du distribuera och hantera affärslogik på gränsen i form av moduler. Azure IoT Edge-moduler är de minsta beräkning enheter som hanteras av IoT Edge och kan innehålla Microsoft-tjänster (till exempel Azure Stream Analytics), 3 part services eller din egen Lösningsspecifika kod. Läs mer om IoT Edge-moduler i [förstå Azure IoT Edge-moduler](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**Vad är skillnaden mellan en behållare erbjudandetypen och en IoT Edge-modulen avtalsform?**

Erbjudandetypen för IoT Edge-modul är en viss typ av behållare som körs på en IoT Edge-enhet. Den levereras med standardinställningarna för konfigurationen att köra i IoT Edge-kontexten och du kan också använder IoT Edge-modul SDK kan integreras med IoT Edge-körningen.

## <a name="publishing-your-iot-edge-module"></a>Publicera din IoT Edge-modul

**Att välja rätt storefront**

IoT Edge-moduler publiceras bara på Azure Marketplace, AppSource gäller inte.  Mer information om skillnader och målgrupp över butiker finns i [avgör publiceringsalternativ för din lösning](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Betalningsalternativ**

Marketplace har för närvarande stöd **kostnadsfri** och **Bring Your Own License (BYOL)** faktureringsalternativ för IoT Edge-moduler.
 
**Publiceringsalternativ**

I samtliga fall IoT Edge-moduler bör välja den **Transact** publicering av alternativet.  Se [väljer ett publiceringsalternativ](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) för mer information om hur du publicerar alternativ.  

## <a name="eligibility-criteria"></a>Kriterier för berättigande

Alla villkor i Microsoft Azure Marketplace-avtal och principer som gäller för IoT Edge-modulen erbjudanden.  Det finns dessutom krav och tekniska krav för IoT Edge-moduler.  

**Förutsättningar**

Om du vill publicera en IoT Edge-modul i Azure Marketplace, måste du uppfylla följande krav:

- Åtkomst till Cloud Partner Portal (CPP). Mer information finns i [publiceringsguide för Azure Marketplace och AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Vara värd för din IoT Edge-modul i ett Azure Container Registry. 
- Har din IoT Edge-modulen metadata redo, till exempel (ofullständig lista): 
    - En rubrik
    - En beskrivning (i HTML-format)
    - En logotyp (PNG-format och fast bildstorleken inklusive 40x40px, 90x90px, 115x115px, 255x115px)
    - En period om och sekretesspolicyn princip
    - Modulen standardkonfigurationen (route, enhetstvillingens egenskaper, createOptions, miljövariabler)
    - Dokumentation
    - Supportkontakter

**Tekniska krav**

De primära tekniska kraven för en IoT Edge-modul, att hämta certifierade och publicerats i Azure Marketplace, beskrivs i den [certifieringsprocessen för IoT Edge-modulen](https://cloudpartner.azure.com/#documentation/iot-edge-module-certification-process) på den [publicering till molnet Portalen](https://cloudpartner.azure.com/).  

## <a name="documentation-and-resources"></a>Dokumentation och resurser

I följande artiklar som är tillgängliga när du är inloggad på den [molnet Publiceringsportalen](https://cloudpartner.azure.com/):

- [Skapa ett erbjudande för IoT Edge-modulen](https://cloudpartner.azure.com/#documentation/create-iot-edge-module-offer) -– steg för att publicera en ny IoT Edge-modul erbjuder med molnet Publiceringsportalen.
- [IoT Edge-modulen certifieringsprocessen](https://cloudpartner.azure.com/#documentation/iot-edge-module-certification-process) – en sammanfattning av stegen och kraven för att certifiera en IoT Edge-modul.
- [IoT Edge-modul vanliga frågor och svar](https://cloudpartner.azure.com/#documentation/iot-edge-module-faq) – – en lista över vanliga frågor och svar som är relaterade till IoT Edge-moduler.

## <a name="next-steps"></a>Nästa steg

Om du inte redan gjort det,

- Registrera i den [Microsoft Partner Network](https://partner.microsoft.com/membership).
- Skapa en [Account](https://account.microsoft.com/account/) (krävs för Azure Marketplace transact erbjudanden; rekommenderas för andra).
- Skicka den [Marketplace registreringsformuläret](https://azuremarketplace.microsoft.com/sell/signup).

Om du är registrerad och skapar ett nytt erbjudande eller arbetar på en befintlig

- Logga in på [Cloud Partner Portal](https://cloudpartner.azure.com/) att skapa eller slutföra ditt erbjudande.

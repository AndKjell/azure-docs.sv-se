---
title: Azure IoT Edge-säkerhetshanteraren | Microsoft Docs
description: Hanterar den IoT Edge-enhet security effekten av åtgärder och integriteten för säkerhetstjänster.
services: iot-edge
keywords: säkerhet, säker element, enklaven, TEE, IoT Edge
author: eustacea
manager: timlt
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f0e548cdd1c59dc894899ddbac127dd76db7db26
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321197"
---
# <a name="azure-iot-edge-security-manager"></a>Azure IoT Edge-säkerhetshanteraren

Azure IoT Edge security manager är en väl begränsad säkerhet kärna för att skydda IoT Edge-enhet och alla dess komponenter genom att abstrahera säker silicon maskinvara. Det står i centrum för säkerhet och tillhandahåller teknik integrering till ursprungliga enhetstillverkare (OEM).

![Azure IoT Edge-säkerhetshanteraren](media/edge-security-manager/iot-edge-security-manager.png)

IoT Edge-säkerhetshanteraren syftar till att skydda integriteten för IoT Edge-enhet och alla åtgärder för inbyggd programvara.  Detta sker genom att övergå förtroende från underliggande maskinvara rot förtroende maskinvara (om tillgängligt) för att på ett säkert sätt kunna starta IoT Edge-körningen och fortsätta att övervaka integriteten hos åtgärderna.  IoT Edge-säkerhetshanteraren i princip består av programvara som arbetar tillsammans med säker silicon maskinvara där det är tillgängligt och aktiverat för att leverera de högsta säkerhetsgarantier som möjligt.  

Ansvaret för IoT Edge-säkerhetshanteraren omfattar (men inte begränsat till):

* Säker och mätt hur startas Azure IoT Edge-enhet.
* Enhetsetablering identitet och övergång av förtroende om tillämpligt.
* Vara värd för och skydda enhetskomponenter i molntjänster som Device Provisioning-tjänsten.
* På ett säkert sätt etablera IoT Edge-moduler med modulen unik identitet.
* Gatekeepern enhet maskinvara förtroenderoten via notarie-tjänster.
* Övervakar du integriteten hos IoT Edge-åtgärder vid körning.

IoT Edge-säkerhetshanteraren består av tre huvudkomponenter:

* Daemon för IoT Edge-säkerhet.
* Maskinvara security modulen plattform abstraction Layer (HSM PAL).
* Valfritt men rekommenderas maskinvara silicon roten för förtroende eller HSM.

## <a name="the-iot-edge-security-daemon"></a>Daemon för IoT Edge-säkerhet

Daemon för IoT Edge-säkerhet är ett program som är ansvarig för logiska driften av IoT Edge security manager.  Den består av en stor del av betrodda databehandling basen för IoT Edge-enhet. 

### <a name="design-principles"></a>Designprinciper

Syftet är att följa följande huvudprinciperna att på ett adekvat och praktiskt taget utföra dess ansvarsområden, IoT Edge security daemon:

#### <a name="maximize-operational-integrity"></a>Maximera operativa integritet

Daemon för IoT Edge-säkerhet är avsedda att fungera med högsta möjliga inom defense möjligheterna för alla angivna roten av förtroende maskinvara integriteten. Med rätt integrering roten av förtroende maskinvara mäter och övervakar daemonen security statiskt och vid körning för att motstå manipulation.

Fysisk åtkomst är alltid ett hot mot IoT-enheter i allmänhet.  Maskinvara förtroenderoten därför spelar en viktig roll i försvar integriteten hos daemonen IoT Edge-säkerhet.  Maskinvara förtroenderoten levereras i två varianter:

* säker element för skydd av känslig information som hemligheter och kryptografiska nycklar.
* säker enklaver för skydd av hemligheter som nycklar och känsliga arbetsbelastningar som mätning och fakturering.

Förekomsten av dessa två modeller för att använda maskinvara förtroenderoten ger upphov till två typer av körningsmiljöer:

* Standard- eller omfattande körningsmiljön (tre) som är beroende av säker element som skyddar känslig information.
* Betrodda körningsmiljön (TEE) som är beroende av säker enklav-teknik för att skydda känslig information och erbjuder skydd till programåtgärd.

För enheter som använder säker enklaver som maskinvara förtroenderoten, förväntas känsliga logik i IoT Edge security daemon finnas inom enklaven.  Icke-känsliga delar av daemonen säkerhet kan finnas utanför TEE.  I båda fallen förväntas av den ursprungliga designen tillverkare (ODM) och tillverkare (OEM) att utöka förtroende från sina HSM att mäta och skydda integriteten för Edge security daemon på Start- och runtime.

#### <a name="minimize-bloat-and-churn"></a>Minimera överdriven storlek och omsättning

En annan core-principen för daemonen Edge säkerhet är att minimera omsättning.  För den högsta nivån på förtroende, kan IoT Edge security daemon nära koppla samman med enhetens maskinvara förtroenderoten där det är tillgängligt och fungera som intern kod.  Det är vanligt för dessa typer av realizations att uppdatera daemon-programvara genom maskinvara roten för säker uppdatering förtroendevägar (i stället för OS tillhandahålls uppdateringsfunktioner), vilket kan vara svårt beroende på specifika maskinvaru- och scenario.  Security förnyelse är starkt rekommendation för IoT-enheter, står det att skäl att långa krav eller stor uppdatering nyttolaster kan expandera threat ytan på många sätt.  Exempel är hoppar över av uppdateringar för att maximera operativ tillgänglighet eller roten av förtroende maskinvara begränsad för att bearbeta stora uppdatering nyttolaster.  Därför utformningen av IoT Edge security daemon är ändå koncis för att hålla storleken och den betrodda databehandling grundläggande lilla och minimera krav.

### <a name="architecture-of-iot-edge-security-daemon"></a>Arkitektur för IoT Edge security daemon

![Daemon för Azure IoT Edge-säkerhet](media/edge-security-manager/iot-edge-security-daemon.png)

IoT Edge security daemon har byggts för att kunna utnyttja alla tillgängliga maskinvara roten av teknik för förtroende för säkerhet.  Mer så att det har byggts för att möjliggöra split-world-åtgärd mellan en Standard/omfattande körning miljö (tre) och en betrodd körning miljö (TEE) för att dra nytta av maskinvara tekniker som erbjuder betrodda körningsmiljöer (TEE).  Kärnan i arkitekturen för IoT Edge security daemon är rollspecifika gränssnitt för att aktivera samspelet mellan viktiga komponenter i Edge för att säkerställa integriteten hos IoT Edge-enhet och dess åtgärder.

#### <a name="cloud-interface"></a>Molngränssnitt

Cloud-gränssnittet kan IoT Edge security daemon att få åtkomst till molntjänster, t.ex en bonus molnet till enheten säkerheten med till exempel säkerhet förnyelse.  Till exempel IoT Edge security daemon för närvarande använder det här gränssnittet att få åtkomst till Azure IoT Hub [Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/) för identitetslivcykelhantering för enheten.  

#### <a name="management-api"></a>Hanterings-API

Daemon för IoT Edge-säkerhet erbjuder en hanterings-API, som anropas av Edge-agenten när du skapar/Starta/Stoppa/tar bort en edge-modul. IoT Edge security daemon lagrar ”registreringar” för alla aktiva moduler. Dessa registreringar mappa en modul identitet till vissa egenskaper för modulen. Några exempel för de här egenskaperna är process-ID (pid) för den process som körs i behållaren eller hashen för innehållet i docker-behållaren.

Dessa egenskaper som används av arbetsbelastningen API (beskrivs nedan) intyga att anroparen har behörighet att utföra en åtgärd.

Management-API är en privilegierad API anropningsbara endast från IoT Edge-agenten.  Eftersom IoT Edge security daemon startar och startar IoT Edge-agenten, kan det skapa en implicit registrering för IoT Edge-agenten när den har godkänt dem att IoT Edge-agenten inte har manipulerats. Samma attesteringsprocessen som arbetsbelastning API använder används för att begränsa åtkomsten till hanterings-API till endast IoT Edge-agenten.

#### <a name="container-api"></a>Behållaren API

Daemon för IoT Edge-säkerhet erbjuder container-gränssnittet för att interagera med behållarsystem som används som Moby och Docker för att modulen instansiering.

#### <a name="workload-api"></a>Arbetsbelastningen API

Arbetsbelastningen API är en IoT Edge på en security-daemon API som är tillgängliga för alla moduler, inklusive IoT Edge-agenten. Det ger identitetsbevis i form av en HSM rotad signerade token eller X509 certifikat och motsvarande förtroende paketet till en modul. Förtroende-paketet innehåller CA-certifikat för alla andra servrar som ska lita på moduler.

Daemon för IoT Edge-säkerhet använder en attesteringsprocessen för att skydda den här API: et. När en modul anropar den här API: et, försöker IoT Edge security daemon hitta en registrering för identiteten. Om detta lyckas använder egenskaperna för registreringen för att mäta modulen. Om resultatet av mätningen Processmatchningar registreringen, en ny HSM rotad signerade token eller X509 certifikatet genereras och motsvarande returneras CA-certifikat (förtroende bundle) i modulen.  Modulen använder det här certifikatet för att ansluta till andra moduler på IoT-hubb eller starta en server. När den signerade token eller certifikat snart upphör att gälla, är det ansvar att modulen kan begära ett nytt certifikat.  Ytterligare överväganden ingår i designen att göra processen för förnyelse av identitet så smidig som möjligt.

### <a name="integration-and-maintenance"></a>Integrering och underhåll

Microsoft underhåller huvudsakliga koden för den [IoT Edge security daemon på GitHub](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Installation och uppdateringar

Som en intern process, hanteras installation och uppdatering av IoT Edge security daemon via operativsystemets systemet för pakethantering.  Men det förväntas att IoT Edge-enheter med maskinvara förtroenderoten och ange ytterligare härdning att daemonen integritet genom att hantera livscykeln för via enheter säker start- och uppdateringar hanteringssystemen.  Det är värt enheter möjlighet att utforska dessa vägar i enlighet med deras respektive enhetsfunktioner.

#### <a name="versioning"></a>Versionshantering

IoT Edge-körningen spårar och rapporterar versionen av IoT Edge security daemon. Versionen rapporteras som den *runtime.platform.version* attribut från modulen Edge-agenten rapporterade egenskap.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Security module platform HAL (HSM PAL)

HSM-PAL avlägsnar alla roten av förtroende maskinvara för att isolera utvecklare eller användare av IoT Edge från deras komplexitet.  Den består av en kombination av Application Programming Interface (API) och trans domän kommunikation procedurer, till exempel kommunikation mellan en standard körningsmiljö och en säker enklaven.  Den faktiska implementeringen av HSM-PAL beror på den säkra maskinvaran används.  Dess finns kan du använda valfri säker silicon maskinvara i IoT-ekosystemet.

## <a name="secure-silicon-root-of-trust-hardware"></a>Säker silicon roten förtroende maskinvara

Säker silicon är nödvändigt att förtroendeankare förtroende i IoT Edge-enhet maskinvara.  Säker silicon finnas i olika att inkludera Trusted Platform Module (TPM), inbäddade säker Element (eSE), ARM TrustZone, Intel SGX och anpassade säker silicon tekniker.  Användning av säker silicon förtroenderoten i enheter rekommenderas starkt beroende hot som är associerade med fysiskt tillgängligheten för IoT-enheter.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>IoT Edge security manager-integrering och underhåll

Ett huvudsyfte av IoT Edge-säkerhetshanteraren är att identifiera och isolera de komponenter som har behörighet att försvara säkerheten och integriteten i Azure IoT Edge-plattformen för anpassade härdning.  Därför förväntas av tredje part som enhetstillverkare möjlighet att göra anpassade säkerhetsfunktionerna som är tillgängliga med sin maskinvara.  Se avsnittet nästa steg nedan på länkar till exempel på hur du kan skydda Azure IoT security manager med den Trusted Platform Module (TPM) på Linux och Windows-plattformar.  De här exemplen använder programvara eller virtuella TPM: er men tillämpas direkt med hjälp av diskreta TPM-enheter.  

## <a name="next-steps"></a>Nästa steg

Läs bloggen på [skydda en intelligent gräns](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

Skapa och etablera en Edge-enhet med en [virtuell TPM på en Linux-dator](how-to-auto-provision-simulated-device-linux.md).

Skapa och etablera en [simulerad TPM-Edge-enhet på Windows](how-to-auto-provision-simulated-device-windows.md).

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/

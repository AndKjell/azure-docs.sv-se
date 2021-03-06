---
title: Terminologi för Azure Service Fabric-nät | Microsoft Docs
description: Läs mer om vanliga termer för Azure Service Fabric-nät.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7c3ce5571c54d6c613114ea49999e450934c8ff4
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136206"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric-nät terminologi

Azure Service Fabric-nät är en fullständigt hanterad tjänst som ger utvecklare möjlighet att distribuera mikrotjänstprogram utan att hantera virtuella datorer, lagring och nätverk. Den här artikeln beskriver de termer som används av Azure Service Fabric nät för att förstå de termer som används i dokumentationen.

## <a name="service-fabric"></a>Service Fabric

Service Fabric är en distribuerad systemplattform för öppen källkod som gör det enkelt att paketera, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Service Fabric är orchestrator som används av Service Fabric-nät. Service Fabric tillhandahåller alternativ för hur du kan skapa och kör dina mikrotjänstprogram. Du kan använda valfritt ramverk för att skriva dina tjänster och välj var du vill köra programmet från miljön välja mellan flera alternativ.

## <a name="deployment-and-application-models"></a>Modeller för distribution och program 

Om du vill distribuera dina tjänster behöver du att beskriva hur de ska köras. Service Fabric har stöd för tre olika distributionsmodeller:

### <a name="resource-model"></a>Resursmodell
Resurserna är något som kan distribueras separat till Service Fabric, inklusive program, tjänster, nätverk och volymer. Resurserna definieras med hjälp av en YAML-fil eller en JSON-fil med hjälp av Azure Resource Manager-schemat.

Resurs-modellen är det enklaste sättet att beskriva dina Service Fabric-program. Dess huvudsakliga fokus ligger på enkel distribution och hantering av tjänster i behållare.

Resurser kan distribueras som helst Service Fabric kan köras.

### <a name="native-model"></a>Inbyggda datamodellen
Internt programmodellen innehåller dina program med fullständig lågnivå-åtkomst till Service Fabric. Program och tjänster definieras som registrerade typer i XML-manifest-filer.

Den inbyggda datamodellen har stöd för Reliable Services-ramverk, som ger åtkomst till Service Fabric-körningen API: er och hantering av API: er i C# och Java. Den inbyggda datamodellen stöder också godtyckliga behållare och körbara filer.

Den inbyggda datamodellen stöds inte i Service Fabric-nät-miljö.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) är en del av Docker-projektet. Service Fabric har begränsat stöd för distribution av program med hjälp av Docker Compose-modellen.

## <a name="environments"></a>Miljöer

Service Fabric är en teknik för öppen källkodsplattform som flera olika tjänster och produkter som är baserade på. Microsoft tillhandahåller följande alternativ:

 - **Service Fabric-nät**: en fullständigt hanterad tjänst för att köra Service Fabric-program i Microsoft Azure.
 - **Azure Service Fabric**: Azure Service Fabric-kluster värdtjänst. Det ger integrering mellan Service Fabric och Azure-infrastrukturen, tillsammans med uppgradering och konfiguration hantering av Service Fabric-kluster.
 - **Fristående Service Fabric**: en uppsättning verktyg för installation och konfiguration att [distribuera Service Fabric-kluster var som helst](/azure/service-fabric/service-fabric-deploy-anywhere) (lokalt eller på någon annan molnleverantör). Inte hanteras av Azure.
 - **Service Fabric-kluster för utveckling**: tillhandahåller en lokal utveckling på Windows, Linux eller Mac för utveckling av Service Fabric-program.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Supportmatris för miljön, ramverk och distributionsmodellen
Olika miljöer har olika stöd för ramverk och distributionsmodeller. I följande tabell beskrivs stöds framework och distribution av modellen kombinationer.

| Typ av program | Beskrivningen av | Azure Service Fabric-nät | Azure Service-kluster (alla OS)| Lokala kluster – Windows | Lokala kluster – Linux | Lokalt kluster - Mac | Fristående kluster (Windows)
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric-nät program | Resursmodell (YAML & JSON) | Stöds |Stöds inte | Stöds |Stöds inte | Stöds inte | Stöds inte |
|Interna Service Fabric-program | Internt programmodell (XML) | Stöds inte| Stöds|Stöds|Stöds|Stöds|Stöds|

I följande tabell beskrivs de olika program-modellerna och verktyg som finns för dem mot Service Fabric.

| Typ av program | Beskrivningen av | Visual Studio 2017 | Visual Studio 2015 | Eclipse | VS-kod | SFCTL | AZ-CLI | PowerShell
|---|---|---|---|---|---|---|---|---|---|
| Service Fabric-nät program | Resursmodell (YAML & JSON) | Stöds |Stöds inte |Stöds inte |Stöds inte |Stöds inte | Stöd för – nät-miljö | Stöds inte
|Interna Service Fabric-program | Internt programmodell (XML) | Stöds| Stöds|Stöds|Stöds|Stöds|Stöds|Stöds|

## <a name="next-steps"></a>Nästa steg

Läs översikten om du vill veta mer om Service Fabric-nät kan:
- [Service Fabric-nät översikt](service-fabric-mesh-overview.md)

---
title: Komma igång med HTTP-begäranden för Azure Relay-hybridanslutningar i Node | Microsoft Docs
description: Skriva ett Node.js-konsolprogram för HTTP-begäranden för Azure Relay-hybridanslutningar i Node.
services: service-bus-relay
documentationcenter: node
author: clemensv
manager: timlt
editor: ''
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 05/02/2018
ms.author: clemensv
ms.openlocfilehash: 2bc923650425c76562161dd6f44f3a5722b5cefe
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38630454"
---
# <a name="get-started-with-relay-hybrid-connections-http-requests-in-node"></a>Komma igång med HTTP-begäranden för Relay-hybridanslutningar i Node

[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

I den här självstudien får du en introduktion till HTTP-begäranden för [Azure Relay-hybridanslutningar](relay-what-is-it.md#hybrid-connections). Vi visar också hur du använder Node.js till att skapa ett klientprogram som skickar meddelanden till motsvarande lyssnarprogram.

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras

Eftersom hybridanslutningar kräver både en klient och en serverkomponent skapar du två konsolprogram i den här självstudien. Här är stegen:

1. Skapa ett Relay-namnområde med Azure Portal.
2. Skapa en hybridanslutning med Azure Portal.
3. Skriv ett serverkonsolprogram för att ta emot meddelanden.
4. Skriv ett klientkonsolprogram för att ta emot meddelanden.

## <a name="prerequisites"></a>Nödvändiga komponenter

1. [Node.js](https://nodejs.org/en/).
2. En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Skapa ett namnområde med Azure Portal

Om du redan har skapat ett Relay-namnområde, går du vidare till avsnittet [Skapa en hybridanslutning med Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Skapa en hybridanslutning med Azure Portal

Om du redan har skapat en hybridanslutning, går du vidare till avsnittet [skapa ett serverprogram](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Skapa ett serverprogram (lyssnare)

För att lyssna på och ta emot meddelanden från Relay skriver du ett Node.js-konsolprogram.

[!INCLUDE [relay-hybrid-connections-node-get-started-server](../../includes/relay-hybrid-connections-http-requests-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Skapa ett klientprogram (avsändare)

För att skicka meddelanden till Relay kan du använda en HTTP-klient eller skriva ett Node.js-konsolprogram.

[!INCLUDE [relay-hybrid-connections-node-get-started-client](../../includes/relay-hybrid-connections-http-requests-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Köra programmen

1. Kör serverprogrammet: i en Node.js-kommandotolk skriver du in `node listener.js`.
2. Kör klientprogrammet: från en Node.js-kommandotolken skriver du in `node sender.js` och anger lite text.
3. Se till att serverprogramkonsolen matar ut den text som angavs i klientprogrammet.

Grattis, du har skapat ett hybridanslutningsprogram från slutpunkt till slutpunkt med hjälp av Node.js!

## <a name="next-steps"></a>Nästa steg

* [Vanliga frågor och svar om Relay](relay-faq.md)
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)


---
title: Anslut till Twilio från Azure Logic Apps | Microsoft Docs
description: Automatisera uppgifter och arbetsflöden som hanterar globala SMS och MMS IP-meddelanden via ditt Twilio-konto med hjälp av Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: db7677042737ea1377af54cc02ee1c82c05435c8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047582"
---
# <a name="manage-messages-in-twilio-with-azure-logic-apps"></a>Hantera meddelanden i Twilio med Azure Logic Apps

Du kan skapa automatiserade uppgifter och arbetsflöden som hämta, skicka och lista meddelanden i Twilio, bland annat globala SMS och MMS IP-meddelanden med Azure Logic Apps och Twilio-anslutningen. Du kan använda de här åtgärderna för att utföra uppgifter med ditt Twilio-konto. Du kan också ha andra åtgärder som använder utdata från Twilio-åtgärder. När ett nytt meddelande anländer, kan du till exempel skicka meddelandet innehåll med Slack-anslutningsprogrammet. Om du är nybörjare till logic apps, granska [vad är Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Förutsättningar

* En Azure-prenumeration. Om du heller inte har någon Azure-prenumeration kan du <a href="https://azure.microsoft.com/free/" target="_blank">registrera ett kostnadsfritt Azure-konto</a>. 

* Från [Twilio](https://www.twilio.com/): 

  * Twilio konto-ID och [autentiseringstoken](https://support.twilio.com/hc/en-us/articles/223136027-Auth-Tokens-and-How-to-Change-Them), som du hittar på din Twilio-instrumentpanel

    Dina autentiseringsuppgifter för tillåta din logikapp för att skapa en anslutning och komma åt ditt Twilio-konto från din logikapp. 
    Om du använder ett Twilio konto, kan du skicka SMS endast *verifierat* telefonnummer.

  * En verifierad Twilio-telefonnummer som kan skicka SMS

  * En verifierad Twilio-telefonnummer som kan ta emot SMS

* Grundläggande kunskaper om [hur du skapar logikappar](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Logikappen där du vill komma åt ditt Twilio-konto. Om du vill använda en Twilio-åtgärd, starta din logikapp med en annan utlösare, till exempel, **upprepning** utlösaren.

## <a name="connect-to-twilio"></a>Anslut till Twilio

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Logga in på den [Azure-portalen](https://portal.azure.com), och öppna logikappen i Logic App Designer, om inte redan är öppna.

1. Välj en sökväg: 

     * Under det sista steget där du vill lägga till en åtgärd, väljer **nytt steg**. 

       ELLER

     * Mellan stegen där du vill lägga till en åtgärd, flyttar du pekaren över pilen mellan stegen. 
     Välj plustecknet (**+**) som visas och välj sedan **Lägg till en åtgärd**.
     
       I sökrutan anger du ”twilio” som filter. 
       Välj vilken åtgärd du önska under åtgärder.

1. Ange informationen som krävs för anslutningen och välj sedan **skapa**:

   * Namnet som ska användas för anslutningen
   * Twilio konto-ID 
   * Din Twilio-åtkomsttoken (autentisering)

1. Ange informationen som krävs för din valda åtgärd och fortsätt att utveckla logikappens arbetsflöde.

## <a name="connector-reference"></a>Referens för anslutningsapp

Teknisk information om utlösare, åtgärder och begränsningar som beskrivs av anslutningsappens OpenAPI (tidigare Swagger) beskrivning, granska kopplingens [referenssida](/connectors/twilio/).

## <a name="get-support"></a>Få support

* Om du har frågor kan du besöka [forumet för Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Om du vill skicka in eller rösta på förslag på funktioner besöker du [webbplatsen för Logic Apps-användarfeedback](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

* Läs mer om andra [Logic Apps-anslutningsprogram](../connectors/apis-list.md)
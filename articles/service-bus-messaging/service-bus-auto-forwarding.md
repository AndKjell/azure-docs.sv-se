---
title: Automatisk vidarebefordring Azure Service Bus-meddelandeentiteter | Microsoft Docs
description: Hur du kopplar en Service Bus-kö eller prenumeration till en annan kö eller ett ämne.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2018
ms.author: spelluru
ms.openlocfilehash: 608510f76d54cc5f3e10587a6f9d1306612672ad
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391116"
---
# <a name="chaining-service-bus-entities-with-autoforwarding"></a>Länkning av Service Bus-entiteter med autoforwarding

Service Bus *autoforwarding* funktionen kan du länka en kö eller prenumeration till en annan kö eller ett ämne som ingår i samma namnområde. När autoforwarding aktiveras, tar bort meddelanden som är placerade i första kö eller prenumeration (källa) automatiskt i Service Bus och placerar dem i andra kön eller ämnet (mål). Det är fortfarande möjligt att skicka ett meddelande till målentitet direkt. Dessutom går det inte att länka en underkö, till exempel en kö för obeställbara, till en annan kö eller ett ämne.

## <a name="using-autoforwarding"></a>Med hjälp av autoforwarding

Du kan aktivera autoforwarding genom att ange den [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] eller [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] Egenskaper för den [QueueDescription] [ QueueDescription] eller [SubscriptionDescription] [ SubscriptionDescription] objekt till källan, som i den följande exempel:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Mål-entiteten måste finnas på gång källentiteten har skapats. Om mål-entiteten inte finns, returnerar ett undantag när du uppmanas att skapa källentiteten i Service Bus.

Du kan använda autoforwarding för att skala ut enskilda avsnitt. Service Bus-begränsningar i [antalet prenumerationer på ett visst ämne](service-bus-quotas.md) till 2 000. Du kan hantera ytterligare prenumerationer genom att skapa andra nivån ämnen. Även om du inte är bunden av Service Bus-begränsning på antalet prenumerationer, kan lägga till en andra nivå av ämnen förbättra hela dataflödet för ditt ämne.

![Scenario med automatisk vidarebefordran][0]

Du kan också använda autoforwarding för att frikoppla avsändare från mottagare. Anta exempelvis att ett ERP-system som består av tre moduler: order bearbetning, lagerhantering och kundrelationshantering. Var och en av dessa moduler genererar meddelanden som är i kö till ett motsvarande ämne. Alice och Bob är säljare som är intresserade av alla meddelanden som är relaterade till sina kunder. Om du vill ta emot meddelanden, skapar Alice och Bob du en personlig kö och en prenumeration på var och en av de ERP-avsnitten som automatiskt vidarebefordrar alla meddelanden till deras köomfång.

![Scenario med automatisk vidarebefordran][1]

Om Alice går på semester, sin personliga kön i stället ERP-avsnittet, blir fullt. I detta scenario eftersom en säljare inte har fått några meddelanden nå ingen av ERP-ämnen någonsin kvot.

## <a name="autoforwarding-considerations"></a>Autoforwarding överväganden

Om målentitet ackumulerar för många meddelanden och överskrider kvoten eller mål-entiteten är inaktiverad, källentiteten lägger till meddelanden till dess [obeställbara meddelanden](service-bus-dead-letter-queues.md) tills det finns utrymme i målet (eller entiteten aktiveras på nytt). Meddelandena fortsätta att finnas i kön för obeställbara meddelanden, så du måste uttryckligen tar emot och bearbetar dem från kön för obeställbara meddelanden.

När länkning tillsammans enskilda avsnitt för att få ett sammansatt ämne med många prenumerationer, rekommenderar vi att du har ett Måttligt antal prenumerationer på första nivån ämnet och många prenumerationer på andra nivån ämnen. Till exempel kan ett första nivån ämne med 20 prenumerationer, var och en av dem har länkats till ett andra nivån ämne med 200 prenumerationer för högre dataflöde än ett första nivån ämne med 200 prenumerationer, var och en har länkats till ett andra nivån ämne med 20 prenumerationer.

Service Bus debiterar en åtgärd för varje vidarebefordrade meddelandet. Till exempel debiteras skickar ett meddelande till ett ämne med 20 prenumerationer, var och en av dem har konfigurerats för att autoforward meddelanden till en annan kö eller ett ämne, som 21 operations om alla prenumerationer på första nivån får en kopia av meddelandet.

Om du vill skapa en prenumeration som går att härleda till en annan kö eller ämne skaparen av prenumerationen måste ha **hantera** behörigheter på både käll- och mål-entiteten. Skicka meddelanden till avsnittet om källan kräver endast **skicka** behörigheter på käll-avsnittet.

## <a name="next-steps"></a>Nästa steg

Detaljerad information om autoforwarding finns i följande referensavsnitt:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Läs mer om Service Bus prestandaförbättringar i 

* [Metodtips för prestandaförbättringar med hjälp av Service Bus-meddelanden](service-bus-performance-improvements.md)
* [Partitionerade meddelandeentiteter][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md

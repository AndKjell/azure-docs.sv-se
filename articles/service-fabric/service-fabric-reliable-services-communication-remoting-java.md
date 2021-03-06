---
title: Tjänsten fjärrkommunikation använder Java i Azure Service Fabric | Microsoft Docs
description: Service Fabric-fjärrkommunikation kan klienter och tjänster att kommunicera med Java-tjänster med hjälp av ett fjärranrop.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 3215ee4adf907524626b4919b637ce23b9e0e782
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750188"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Tjänsten fjärrkommunikation i Java med Reliable Services
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java i Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

För tjänster som inte är kopplad till en viss kommunikationsprotokoll eller stacken, till exempel WebAPI, Windows Communication Foundation (WCF) eller andra, ger Reliable Services framework dig möjlighet att snabbt och enkelt konfigurera RPC-anrop för fjärrkommunikation tjänster.  Den här artikeln beskrivs hur du ställer in RPC-anrop för tjänster som skrivits med Java.

## <a name="set-up-remoting-on-a-service"></a>Ställa in fjärrstyrning för en tjänst
Ställa in fjärrstyrning för en tjänst görs i två enkla steg:

1. Skapa ett gränssnitt för tjänsten att implementera. Det här gränssnittet definierar metoder som är tillgängliga för remote procedure call på din tjänst. Metoderna måste vara åtgärdsreturnerande asynkrona metoder. Gränssnittet måste implementera `microsoft.serviceFabric.services.remoting.Service` att signalera att tjänsten har ett gränssnitt för fjärrkommunikation.
2. Använd en lyssnare för fjärrkommunikation i din tjänst. Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation. `FabricTransportServiceRemotingListener` kan användas för att skapa en lyssnare för fjärrkommunikation med transportprotokollet standard fjärrkommunikation.

Till exempel exponerar följande tillståndslösa tjänsten en metod för att få ”Hello World” över ett fjärranrop.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> Argumenten och returtyper i gränssnittet service kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras.
>
>

## <a name="call-remote-service-methods"></a>Anropa fjärrtjänsten metoder
Anropar metoder för en tjänst med hjälp av fjärrkommunikation stacken görs med hjälp av en lokal proxyserver till tjänsten via den `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klass. Den `ServiceProxyBase` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som tjänsten implementerar. Med denna proxy, kan du bara fjärranropa metoder i gränssnittet.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Fjärr-framework sprider undantag på tjänsten till klienten. Så undantagshantering logik på klienten med hjälp av `ServiceProxyBase` direkt kan hantera undantag som genereras av tjänsten.

## <a name="service-proxy-lifetime"></a>Tjänsten Proxylivslängden
ServiceProxy skapas en enkel åtgärd så att du kan skapa så många som du behöver. Tjänstproxy instanser kan återanvändas när de behövs. Du kan återanvända samma instans som proxy även om ett fjärranrop genererar ett undantag. Varje ServiceProxy innehåller en klient för kommunikation som används för att skicka meddelanden via kabel. Vid anrop fjärranrop utförs interna kontroller för att fastställa om kommunikation klienten är giltig. Baserat på resultatet av dessa kontroller, återskapas kommunikation klienten om det behövs. Därför om ett undantag inträffar kan du inte behöver återskapa `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory livslängd
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt. Om du använder API `ServiceProxyBase.create` för att skapa proxy, framework skapar sedan en `FabricServiceProxyFactory`.
Det är praktiskt att skapa ett manuellt när du måste åsidosätta [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) egenskaper.
Fabriken är en kostsam åtgärd. `FabricServiceProxyFactory` underhåller cache för klienter för kommunikation.
Bästa praxis är att cacheminnet `FabricServiceProxyFactory` så länge som möjligt.

## <a name="remoting-exception-handling"></a>Fjärrkommunikation undantagshantering
Alla fjärranslutna undantag som uppstått i tjänsten API, skickas tillbaka till klienten som RuntimeException eller FabricException.

ServiceProxy hanterar alla Failover-undantag för service-partition som skapats för. Slutpunkterna löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker anropa med korrekt slutpunkt. Antal återförsök för redundans undantag är obegränsad.
Vid TransientExceptions, endast ett nytt försök anropet.

Försök standardparametrarna är etablerar av [OperationRetrySettings]. (https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Du kan konfigurera dessa värden genom att skicka OperationRetrySettings objekt till ServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Nästa steg
* [Att säkra kommunikationen för Reliable Services](service-fabric-reliable-services-secure-communication-java.md)

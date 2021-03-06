---
title: Hantera fel i varaktiga funktioner – Azure
description: Lär dig mer om att hantera fel i tillägget varaktiga funktioner för Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: azfuncdf
ms.openlocfilehash: 6bf9eb2cd2ebdf5f6d53e00923146bab49a142bf
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377913"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Hantera fel i varaktiga funktioner (Azure Functions)

Hållbar funktionen orkestreringar implementeras i kod och kan använda funktionerna för hantering av fel i programmeringsspråket. Med detta i åtanke, det verkligen inte finns några nya begrepp som du behöver mer information om när du använder felhantering och ersättning i din orkestreringar. Det finns dock några beteenden som du bör känna till.

## <a name="errors-in-activity-functions"></a>Fel i Aktivitetsfunktioner

Alla undantag som genereras i en aktivitet funktionen ordnas tillbaka till orchestrator-funktion och genereras som en `FunctionFailedException`. Du kan skriva felhantering och kompensation felkoden som passar dina behov i orchestrator-funktion.

Anta exempelvis att följande orchestrator-funktion som överför pengar från ett konto till en annan:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        { 
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

Om anropet till den **CreditAccount** misslyckas åtgärden för mål-konto, orchestrator-funktion kompenserar för det här genom kreditering pengar tillbaka till källkontot.

## <a name="automatic-retry-on-failure"></a>Automatiska återförsök vid fel

När du anropar Aktivitetsfunktioner eller underordnade orchestration-funktioner, kan du ange en automatisk återförsöksprincip. I följande exempel försöker anropa en funktion upp till tre gånger och väntar 5 sekunder mellan varje nytt försök:

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);
    
    // ...
}
```

Den `CallActivityWithRetryAsync` API tar en `RetryOptions` parametern. Suborchestration anrop med hjälp av den `CallSubOrchestratorWithRetryAsync` API kan använda dessa samma principer för återförsök.

Det finns flera alternativ för att anpassa automatisk återförsöksprincipen. Dessa inkluderar:

* **Maxantal försök**: det maximala antalet nya försök.
* **Första återförsöket**: hur lång tid innan det första återförsöket försöka.
* **Backoff koefficienten**: koefficienten används för att fastställa ökningstakt för backoff. Standardvärdet är 1.
* **Max återförsöksintervallet**: längsta tid som ska förflyta mellan försöken.
* **Nya försök**: längsta tid för gör ett nytt försök görs. Standardinställningen är att försöka igen på obestämd tid.
* **Hantera**: en användardefinierad motringning kan anges som bestämmer huruvida ett funktionsanrop ska göras.

## <a name="function-timeouts"></a>Funktionen tidsgränser

Du kanske vill lämna ett funktionsanrop inom en orchestrator-funktion om det tar för lång tid att slutföra. Det korrekta sättet att göra detta idag är genom att skapa en [varaktiga timer](durable-functions-timers.md) med `context.CreateTimer` tillsammans med `Task.WhenAny`, som i följande exempel:

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!NOTE]
> Den här mekanismen avslutar inte faktiskt pågående aktivitetskörning av funktion. Det helt enkelt står orchestrator-funktion att ignorera resultatet och gå vidare. Mer information finns i den [Timers](durable-functions-timers.md#usage-for-timeout) dokumentation.

## <a name="unhandled-exceptions"></a>Ett ohanterat undantag

Om en orchestrator-funktion misslyckas med ett ohanterat undantag, loggas information om undantaget och instansen är klar med en `Failed` status.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Lär dig att diagnostisera problem](durable-functions-diagnostics.md)

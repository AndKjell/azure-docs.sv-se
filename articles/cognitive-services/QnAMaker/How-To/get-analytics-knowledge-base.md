---
title: Analys av knowledgebase
titleSuffix: Azure Cognitive Services
description: QnA Maker lagrar alla chattloggarna och annan telemetri om du har aktiverat App Insights under genereringen av QnA Maker-tjänsten. Kör Exempelfrågor för att få din chattloggarna från App Insights.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim88
ms.openlocfilehash: dc363a3ba0d809a3307a6803993bdf500da45f1b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035427"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Få analyser om din kunskapsbas

QnA Maker lagrar alla chattloggarna och annan telemetri om du har aktiverat App Insights under den [skapandet av QnA Maker-tjänsten](./set-up-qnamaker-service-azure.md). Kör Exempelfrågor för att få din chattloggarna från App Insights.

1. Gå till din App Insights-resurs.

    ![Välj application insights-resurs](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Välj **Analytics**. En ny fönster öppnas där du kan fråga QnA Maker telemetri.

    ![Välj Analytics](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Klistra in följande fråga och kör den.

    ```query
        requests
        | where url endswith "generateAnswer"
        | project timestamp, id, name, resultCode, duration
        | parse name with *"/knowledgebases/"KbId"/generateAnswer"
        | join kind= inner (
        traces | extend id = operation_ParentId
        ) on id
        | extend question = tostring(customDimensions['Question'])
        | extend answer = tostring(customDimensions['Answer'])
        | project KbId, timestamp, resultCode, duration, question, answer
    ```

    Välj **kör** att köra frågan.

    ![Kör frågan](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Köra frågor för andra analyser i kunskapsbasen QnA Maker

### <a name="total-90-day-traffic"></a>Total trafik för 90 dagar

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
``` 

### <a name="total-question-traffic-in-a-given-time-period"></a>Totalt antal fråga trafik i en viss tidsperiod

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Trafik för användare

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse name with *"/knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Fördelning av svarstid för frågor

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse name with *"/knowledgebases/"KbId"/generateAnswer" 
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Hantera nycklar](./key-management.md)

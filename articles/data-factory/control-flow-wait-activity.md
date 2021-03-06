---
title: Vänta aktivitet i Azure Data Factory | Microsoft Docs
description: Aktiviteten vänta pausar körningen av pipelinen för den angivna perioden.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 74a5d687535915fab7d518faaf916b98ab262c4b
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053906"
---
# <a name="wait-activity-in-azure-data-factory"></a>Vänta aktivitet i Azure Data Factory
När du använder en Wait-aktivitet i en pipeline väntar pipelinen under den angivna tidsperioden innan körningen återupptas med alla efterföljande aktiviteter. 

## <a name="syntax"></a>Syntax

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Typegenskaper

Egenskap  | Beskrivning | Tillåtna värden | Krävs
-------- | ----------- | -------------- | --------
namn | Namnet på den `Wait` aktivitet. | Sträng | Ja
typ | Måste anges till **vänta**. | Sträng | Ja
waitTimeInSeconds | Antal sekunder som pipelinen ska vänta innan du fortsätter med bearbetning. | Integer | Ja

## <a name="example"></a>Exempel

> [!NOTE]
> Det här avsnittet innehåller definitioner av JSON och exempel PowerShell-kommandon för att köra pipelinen. En genomgång med stegvisa anvisningar för att skapa Data Factory-pipelinen med hjälp av Azure PowerShell-och JSON finns [Självstudier: skapa en datafabrik med hjälp av Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>Pipeline med väntaktiviteter
I det här exemplet har två aktiviteter i pipeline: **tills** och **vänta**. Aktiviteten vänta är konfigurerad för att vänta på en sekund. Pipelinen körs aktiviteten webbprogram i en slinga med en sekund väntetiden mellan varje körning. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
    }
}

```

## <a name="next-steps"></a>Nästa steg
Se annan kontrollflödesaktiviteter som stöds av Data Factory: 

- [If-villkorsaktivitet](control-flow-if-condition-activity.md)
- [Execute Pipeline-aktivitet](control-flow-execute-pipeline-activity.md)
- [För varje aktivitet](control-flow-for-each-activity.md)
- [GetMetadata-aktivitet](control-flow-get-metadata-activity.md)
- [Lookup-aktivitet](control-flow-lookup-activity.md)
- [Webbaktivitet](control-flow-web-activity.md)
- [Until-aktivitet](control-flow-until-activity.md)

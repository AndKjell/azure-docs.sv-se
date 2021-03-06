---
title: Parameterisera länkade tjänster i Azure Data Factory | Microsoft Docs
description: Lär dig mer om att Parameterisera länkade tjänster i Azure Data Factory och skicka dynamiska värden vid körning.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: 287dcdedede5cab575aa0b9a73ec3e122556dc93
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900731"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parameterisera länkade tjänster i Azure Data Factory

Du kan nu Parameterisera en länkad tjänst och skicka dynamiska värden vid körning. Om du vill ansluta till olika databaser på samma Azure SQL Database-server kan du nu Parameterisera namnet på databasen i länkade tjänstedefinition. Detta gör att du slipper skapa en länkad tjänst för varje databas i Azure SQL database-server. Du kan Parameterisera andra egenskaper i den länkade tjänstdefinitionen samt – till exempel *användarnamn.*

Du kan använda Data Factory-Användargränssnittet på Azure-portalen eller ett programmeringsgränssnitt för att Parameterisera länkade tjänster.

> [!TIP]
> Vi rekommenderar inte för att Parameterisera lösenord eller hemligheter. I stället Store alla anslutningssträngar i Azure Key Vault och Parameterisera den *hemligt namn*.

## <a name="supported-data-stores"></a>Lagrar data som stöds

För närvarande stöds länkade tjänsten parameterisering i Användargränssnittet för Data Factory i Azure portal för följande datalager. För alla andra datalager du Parameterisera den länkade tjänsten genom att välja den **kod** ikonen på pipelinefliken och använder JSON-redigerare.
- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server
- Oracle
- Cosmos DB
- Amazon Redshift
- MySQL
- Azure Database for MySQL

## <a name="data-factory-ui"></a>Data Factory-användargränssnitt

![Lägg till dynamiskt innehåll till länkade tjänstens definition](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Skapa en ny parameter](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "value": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
                "type": "SecureString"
            }
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```

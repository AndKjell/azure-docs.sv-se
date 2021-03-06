---
title: Hantera Azure Redis Cache med Azure PowerShell | Microsoft Docs
description: Lär dig mer om att utföra administrativa uppgifter för Azure Redis Cache med hjälp av Azure PowerShell.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: 11b399bdbbbe92d1e1544f4cbae0824140ce52c1
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364514"
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Hantera Azure Redis Cache med Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Det här avsnittet visar dig hur du utför vanliga uppgifter som att skapa, uppdatera och skala dina Azure Redis Cache-instanser, hur du återskapar åtkomstnycklarna och hur du visar information om dina cacheminnen. En fullständig lista över Azure Redis Cache PowerShell-cmdlets, se [Azure Redis Cache-cmdletar](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Läs mer om den klassiska distributionsmodellen [Azure Resource Manager och klassisk distribution: distributionsmodeller och dina resursers tillstånd](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Förutsättningar
Om du redan har installerat Azure PowerShell måste du ha Azure PowerShell, version 1.0.0 eller senare. Du kan kontrollera versionen av Azure PowerShell som du har installerat med det här kommandot i Azure PowerShell-Kommandotolken.

    Get-Module azure | format-table version


Först måste måste du logga in på Azure med det här kommandot.

    Connect-AzureRmAccount

Ange e-postadressen till ditt Azure-konto och lösenordet i dialogrutan för Microsoft Azure.

Därefter om du har flera Azure-prenumerationer kan behöva du ange dina Azure-prenumeration. Kör det här kommandot om du vill se en lista över dina aktuella prenumerationer.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Kör följande kommando för att ange prenumerationen. I följande exempel prenumerationens namn är `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Innan du kan använda Windows PowerShell med Azure Resource Manager, behöver du följande:

* Windows PowerShell, Version 3.0 eller 4.0. Om du vill ta reda på vilken version av Windows PowerShell skriver du:`$PSVersionTable` och kontrollera värdet för `PSVersion` 3.0 eller 4.0. Om du vill installera en kompatibel version, se [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) eller [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Använd cmdleten Get-Help för att få detaljerad hjälp för alla cmdletar som du ser i den här självstudien.

    Get-Help <cmdlet-name> -Detailed

Till exempel för att få hjälp med den `New-AzureRmRedisCache` cmdlet, typ:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Hur du ansluter till andra moln
Som standard Azure miljö är `AzureCloud`, som representerar den globala Azure-moln-instansen. Om du vill ansluta till en annan instans den `Connect-AzureRmAccount` med den `-Environment` eller -`EnvironmentName` kommandoradsväxeln med önskad miljö eller miljönamn.

Om du vill se en lista över tillgängliga miljöer, kör den `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Att ansluta till Azure Government-molnet
Använd någon av följande kommandon för att ansluta till Azure Government-molnet.

    Connect-AzureRmAccount -EnvironmentName AzureUSGovernment

eller

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Använd någon av följande platser för att skapa ett cacheminne i Azure Government-molnet.

* USGov Virginia
* USGov Iowa

Läs mer om Azure Government-molnet, [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) och [Utvecklarguide för Microsoft Azure Government](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Att ansluta till Azure Kina-molnet
Använd någon av följande kommandon för att ansluta till Azure Kina-molnet.

    Connect-AzureRmAccount -EnvironmentName AzureChinaCloud

eller

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Använd någon av följande platser för att skapa ett cacheminne i Azure Kina-molnet.

* Östra Kina
* Norra Kina

Läs mer om Azure Kina-molnet, [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Att ansluta till Microsoft Azure Tyskland
Använd någon av följande kommandon för att ansluta till Microsoft Azure Tyskland.

    Connect-AzureRmAccount -EnvironmentName AzureGermanCloud


eller

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Använd någon av följande platser för att skapa ett cacheminne i Microsoft Azure Tyskland.

* Centrala Tyskland
* Nordöstra Tyskland

Läs mer om Microsoft Azure Germany [Microsoft Azure Germany](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Egenskaper som används för Azure Redis Cache PowerShell
I följande tabell innehåller egenskaperna och beskrivningar för vanliga parametrar när du skapar och hanterar din Azure Redis Cache-instanser med Azure PowerShell.

| Parameter | Beskrivning | Standard |
| --- | --- | --- |
| Namn |Namnet på cachen | |
| Plats |Platsen för cachen | |
| ResourceGroupName |Resursgruppens namn som skapas i cachen | |
| Storlek |Storleken på cacheminnet. Giltiga värden är: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |Antalet partitioner skapa när du skapar en premium-cache med klustring aktiverat. Giltiga värden är: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Anger SKU för cachen. Giltiga värden är: Basic, Standard, Premium |Standard |
| RedisConfiguration |Anger inställningar för Redis. Information om varje inställning finns i följande [RedisConfiguration egenskaper](#redisconfiguration-properties) tabell. | |
| enableNonSslPort |Anger om icke-SSL-porten är aktiverad. |False |
| MaxMemoryPolicy |Den här parametern är inaktuell – Använd RedisConfiguration istället. | |
| StaticIP |När du har ditt cacheminne i ett virtuellt nätverk, anger du en unik IP-adress i undernätet för cachen. Om du inte är något valt från undernätet. | |
| Undernät |När du har ditt cacheminne i ett virtuellt nätverk, anger du namnet på undernätet där du kan distribuera cacheminnet. | |
| VirtualNetwork |När du har ditt cacheminne i ett virtuellt nätverk, anger du resurs-ID för det virtuella nätverket där du kan distribuera cacheminnet. | |
| KeyType |Anger vilka åtkomstnyckel att återskapa när du förnyar åtkomstnycklar. Giltiga värden är: primär, sekundär | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration egenskaper
| Egenskap  | Beskrivning | Prisnivåer |
| --- | --- | --- |
| RDB-säkerhetskopiering – aktiverat |Om [Redis-datapersistens](cache-how-to-premium-persistence.md) är aktiverat |Endast Premium |
| RDB-storage-connection-string |Anslutningssträngen till lagringskontot för [Redis-datapersistens](cache-how-to-premium-persistence.md) |Endast Premium |
| RDB-säkerhetskopieringsfrekvens |Säkerhetskopieringsfrekvensen för [Redis-datapersistens](cache-how-to-premium-persistence.md) |Endast Premium |
| maxmemory-reserverade |Konfigurerar den [minne reserverade](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för icke-cache-processer |Standard och Premium |
| princip för max. minne |Konfigurerar den [avlägsningsprincipen](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) för cache |Alla prisnivåer |
| meddela-keyspace-händelser |Konfigurerar [keyspace-meddelanden](cache-configure.md#keyspace-notifications-advanced-settings) |Standard och Premium |
| hash-max-ziplist-transaktioner |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| hash-max-ziplist-value |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| set-max-intset-transaktioner |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| zset-max-ziplist-transaktioner |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| zset / max-ziplist-värde |Konfigurerar [minnesoptimering](http://redis.io/topics/memory-optimization) för små sammanställd datatyper |Standard och Premium |
| databaser |Konfigurerar hur många databaser. Den här egenskapen kan endast konfigureras hos skapa cache. |Standard och Premium |

## <a name="to-create-a-redis-cache"></a>Skapa en Redis-Cache
Nya Azure Redis Cache-instanser har skapats med hjälp av den [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) cmdlet.

> [!IMPORTANT]
> Första gången du skapar en Redis-cache i en prenumeration med Azure-portalen på portalen registrerar den `Microsoft.Cache` namnrymd för den prenumerationen. Om du försöker skapa den första Redis-cachen i en prenumeration med hjälp av PowerShell, måste du först registrera det namnområdet med följande kommando; Annars cmdletar som `New-AzureRmRedisCache` och `Get-AzureRmRedisCache` misslyckas.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Att se en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Kör följande kommando för att skapa ett cacheminne med standardparametrar.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, och `Location` är obligatoriska parametrar, men resten är valfria och har standardvärden. Kör föregående kommando skapar en Standard-SKU Azure Redis Cache-instans med det angivna namnet, plats och resursgrupp som är 1 GB i storlek med icke-SSL-porten inaktiverad.

Ange en storlek på P1 (6 GB – 60 GB), P2 (13 GB 130 GB), om du vill skapa en premium-cache P3 (26 GB 260 GB), eller P4 (53 GB 530 GB). Om du vill aktivera klustring, ange en shard antal med hjälp av den `ShardCount` parametern. I följande exempel skapas en P1 premium-cache med 3 shards. Premium P1-cache är 6 GB i storlek och eftersom vi har angett tre shards den totala storleken är 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Att ange värden för den `RedisConfiguration` parametern och ange värden i `{}` som nyckel/värde-par som `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. I följande exempel skapas en standard 1 GB-cache med `allkeys-random` maxmemory-principen och keyspace-meddelanden som konfigurerats med `KEA`. Mer information finns i [Keyspace-meddelanden (avancerade inställningar)](cache-configure.md#keyspace-notifications-advanced-settings) och [minne principer](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Konfigurera databaserna konfigurera under Skapa cache
Den `databases` inställningen konfigureras bara när cache skapas. I följande exempel skapas en premium P3 (26 GB)-cache med 48 databaser med hjälp av den [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCache?view=azurermps-6.6.0) cmdlet.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Mer information om den `databases` egenskap, finns i [standard Azure Redis Cache-serverkonfiguration](cache-configure.md#default-redis-server-configuration). Mer information om hur du skapar en cache med hjälp av den [New-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/new-azurermrediscache?view=azurermps-6.6.0) cmdlet, finns i den tidigare [att skapa en Redis-Cache](#to-create-a-redis-cache) avsnittet.

## <a name="to-update-a-redis-cache"></a>Uppdatera en Redis-cache
Azure Redis Cache-instanser har uppdaterats med den [Set-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Set-AzureRmRedisCache?view=azurermps-6.6.0) cmdlet.

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Set-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Den `Set-AzureRmRedisCache` cmdlet kan användas för att uppdatera egenskaperna som `Size`, `Sku`, `EnableNonSslPort`, och `RedisConfiguration` värden. 

Följande kommando uppdaterar principen för max. minne för Redis-Cache med namnet myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a>Skala en Redis-cache
`Set-AzureRmRedisCache` kan användas för att skala en Azure Redis cache när den `Size`, `Sku`, eller `ShardCount` egenskaper har ändrats. 

> [!NOTE]
> Skala en cache med hjälp av PowerShell är föremål för samma begränsningar och riktlinjer som när du skalar en cache från Azure-portalen. Du kan skala till en annan prisnivå med följande begränsningar.
> 
> * Du kan skala från en högre prisnivå till en lägre prisnivå.
> * Du kan skala från en **Premium** cachelagra ned till en **Standard** eller en **grundläggande** cache.
> * Du kan skala från en **Standard** cachelagra ned till en **grundläggande** cache.
> * Du kan skala från en **grundläggande** cachelagra en **Standard** cache, men du kan inte ändra storlek på samma gång. Om du behöver en annan storlek kan göra du en efterföljande skalning åtgärd till önskad storlek.
> * Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache. Skalar från **grundläggande** till **Standard** i en enda skalning åtgärd och sedan från **Standard** till **Premium** i ett efterföljande skalning åtgärden.
> * Du kan skala från en större storlek ned till den **C0 (250 MB)** storlek.
> 
> Mer information finns i [så skala Azure Redis Cache](cache-how-to-scale.md).
> 
> 

I följande exempel visas hur du skalar en cache med namnet `myCache` ett 2,5 GB cacheminne. Observera att det här kommandot fungerar för både en grundläggande eller en Standard-cache.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

När det här kommandot har utfärdats, returneras status på cacheminnet (liknar anropa `Get-AzureRmRedisCache`). Observera att den `ProvisioningState` är `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

När du skalar åtgärden har slutförts, den `ProvisioningState` ändras till `Succeeded`. Om du behöver göra en efterföljande skalning åtgärden, till exempel byta från Basic till Standard och sedan ändra storlek, måste du vänta tills den föregående åtgärden har slutförts eller du får ett felmeddelande som liknar följande.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Att få information om en Redis-cache
Du kan hämta information om en cache med hjälp av den [Get-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/get-azurermrediscache?view=azurermps-6.6.0) cmdlet.

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

För att returnera information om alla cacheminnen i den aktuella prenumerationen, kör `Get-AzureRmRedisCache` utan några parametrar.

    Get-AzureRmRedisCache

För att returnera information om alla cacheminnen i en specifik resursgrupp, kör `Get-AzureRmRedisCache` med den `ResourceGroupName` parametern.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

För att returnera information om en specifik cache, köra `Get-AzureRmRedisCache` med den `Name` parameter som innehåller namnet på cachen och `ResourceGroupName` parameter med resursgruppen som innehåller detta cache.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Att komma åt åtkomstnycklarna för Redis-cache
Du kan använda för att hämta åtkomstnycklarna för ditt cacheminne i [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-6.6.0) cmdlet.

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Get-AzureRmRedisCacheKey`, kör du följande kommando.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hämta nycklarna för din cachelagring genom att anropa den `Get-AzureRmRedisCacheKey` cmdlet och skicka namnet på din cache namnet på resursgruppen som innehåller cachen.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Återskapa åtkomstnycklar för Redis-cache
Du kan använda för att återskapa åtkomstnycklarna för ditt cacheminne i [New AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/New-AzureRmRedisCacheKey?view=azurermps-6.6.0) cmdlet.

Att se en lista över tillgängliga parametrar och deras beskrivningar för `New-AzureRmRedisCacheKey`, kör du följande kommando.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Om du vill återskapa den primära eller sekundära nyckeln för ditt cacheminne, anropa den `New-AzureRmRedisCacheKey` cmdleten och ange namn, resursgrupp och ange antingen `Primary` eller `Secondary` för den `KeyType` parametern. I följande exempel återskapas den sekundära åtkomstnyckeln för cache.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Att ta bort en Redis-cache
Ta bort en Redis-cache genom att använda den [Remove-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/remove-azurermrediscache?view=azurermps-6.6.0) cmdlet.

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Remove-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

I följande exempel cachen med namnet `myCache` tas bort.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Importera en Redis-cache
Du kan importera data till en Azure Redis Cache-instans med den `Import-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export är endast tillgänglig för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Läs mer om importera/exportera [importera och exportera data i Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Import-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Följande kommando importerar data från blob som anges av SAS-uri i Azure Redis Cache.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Så här exporterar du en Redis-cache
Du kan exportera data från en Azure Redis Cache-instans med den `Export-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Import/Export är endast tillgänglig för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Läs mer om importera/exportera [importera och exportera data i Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Export-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Följande kommando exporterar data från en Azure Redis Cache-instans i den behållare som angavs av SAS-URI: n.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Starta om en Redis-cache
Du kan starta om din Azure Redis Cache-instans med den `Reset-AzureRmRedisCache` cmdlet.

> [!IMPORTANT]
> Omstart är endast tillgänglig för [premiumnivån](cache-premium-tier-intro.md) cachelagrar. Mer information om hur du startar om din cache finns i [cachelagra administration – starta om](cache-administration.md#reboot).
> 
> 

Att se en lista över tillgängliga parametrar och deras beskrivningar för `Reset-AzureRmRedisCache`, kör du följande kommando.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Följande kommando startar om båda noderna i den angivna cachen.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Nästa steg
Mer information om hur du använder Windows PowerShell med Azure finns i följande resurser:

* [Azure Redis Cache-cmdlet-dokumentationen på MSDN](https://docs.microsoft.com/powershell/module/azurerm.rediscache/?view=azurermps-6.6.0)
* [Azure Resource Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Lär dig att använda cmdletarna i modulen Azure Resource Manager.
* [Hantera Azure-resurser med hjälp av resursgrupper](../azure-resource-manager/resource-group-template-deploy-portal.md): Lär dig hur du skapar och hanterar resursgrupper i Azure-portalen.
* [Azure-bloggen](https://azure.microsoft.com/blog/): Lär dig mer om nya funktioner i Azure.
* [Windows PowerShell-blogg](http://blogs.msdn.com/powershell): Lär dig mer om nya funktioner i Windows PowerShell.
* [”Hey, Scripting Guy”! Blogg](http://blogs.technet.com/b/heyscriptingguy/): få verkliga tips och tricks från Windows PowerShell-communityn.


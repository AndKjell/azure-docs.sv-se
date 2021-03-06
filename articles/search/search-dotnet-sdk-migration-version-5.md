---
title: Uppgradera till Azure Search .NET SDK version 5 | Microsoft Docs
description: Uppgradera till Azure Search .NET SDK version 5
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: brjohnst
ms.openlocfilehash: b08507d7685ce87a4c176385f750a72d6ae51ba3
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091148"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Uppgradera till Azure Search .NET SDK version 5
Om du använder version 4.0-förhandsversion eller äldre av den [Azure Search .NET SDK](https://aka.ms/search-sdk), den här artikeln hjälper dig att uppgradera programmet att använda version 5.

En mer allmän genomgång av SDK inklusive exempel finns i [hur du använder Azure Search från .NET-program](search-howto-dotnet-sdk.md).

Version 5 av Azure Search .NET SDK innehåller vissa ändringar från tidigare versioner. Det här är främst mindre, så ändra din kod kräver bara minimal ansträngning. Se [stegen för att uppgradera](#UpgradeSteps) anvisningar om hur du ändrar din kod till den nya versionen av SDK.

> [!NOTE]
> Om du använder version 2.0 preview eller äldre, bör du uppgradera till version 3 först och sedan uppgraderar till version 5. Se [uppgradering till Azure Search .NET SDK version 3](search-dotnet-sdk-migration.md) anvisningar.
>
> Din Azure Search-tjänstinstans har stöd för flera REST API-versioner, inklusive det senaste. Du kan fortsätta att använda en version när den inte längre det senaste, men vi rekommenderar att du migrerar din kod för att använda den senaste versionen. När du använder REST API, måste du ange API-versionen i varje begäran via parametern api-versionen. När du använder .NET SDK, avgör version av SDK: N som du använder motsvarande version av REST API. Om du använder en äldre SDK kan fortsätta du att köra koden utan ändringar, även om tjänsten uppgraderas för att stödja en nyare API-version.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Vad är nytt i version 5
Azure Search .NET SDK version 5 riktar sig mot senast allmänt tillgängliga versionen av Azure Search-REST-API, särskilt 2017-11-11. Detta gör det möjligt att använda nya funktioner i Azure Search från .NET-program, inklusive följande:

* [Synonymer](search-synonyms.md).
* Du kan nu via programmering komma åt varningar i indexeraren körningstiden (finns i den `Warning` egenskapen för `IndexerExecutionResult` i den [.NET-referens](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) för mer information).
* Stöd för .NET Core 2.
* Ny paketet struktur stöder bara delarna av SDK: N som du behöver (se [större ändringar i version 5](#ListOfChanges) information).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Steg för att uppgradera
Först uppdatera dina NuGet-referens för `Microsoft.Azure.Search` med hjälp av NuGet Package Manager-konsolen eller genom att högerklicka på projektreferenserna och välja ”hantera NuGet-paket...” i Visual Studio.

Återskapa ditt projekt när NuGet har laddats ned nya paket och deras beroenden. Om du inte använder en förhandsgranskningsfunktion som inte är i det nya GA-SDK, det bör återskapa har (om inte, låt oss veta på [GitHub](https://github.com/azure/azure-sdk-for-net/issues)). I så, fall är du redo att sätta igång!

Tänk på att på grund av ändringar i Azure Search .NET SDK förpackningen, måste du bygga upp ditt program för att kunna använda version 5. Dessa ändringar finns beskrivna i [större ändringar i version 5](#ListOfChanges).

Du kan se ytterligare build-varningar relaterade till föråldrade metoder eller egenskaper. Varningar innehåller information om vad du ska använda i stället för föråldrad funktion. Exempel: om programmet använder den `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` metoden bör du får ett varningsmeddelande som säger ”det här beteendet är nu aktiverad som standard så anropa den här metoden är inte längre nödvändigt”.

När du har lösts alla build-varningar kan du ändra ditt program att kunna utnyttja nya funktioner om du vill. Nya funktioner i SDK finns beskrivna i [Nyheter i version 5](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Större ändringar i version 5
Den mest betydande stora ändringen i version 5 är att den `Microsoft.Azure.Search` sammansättning och dess innehåll har delats upp i fyra separata sammansättningar som nu har distribuerats som fyra separata NuGet-paket:

 - `Microsoft.Azure.Search`: Det här är ett meta-paket som innehåller alla andra Azure Search-paketen som beroenden. Om du uppgraderar från en tidigare version av SDK: N bör borde bara uppgradera det här paketet och att bygga om räcka att börja använda den nya versionen.
 - `Microsoft.Azure.Search.Data`: Använd det här paketet om du utvecklar ett .NET-program som använder Azure Search och du behöver bara frågor eller uppdaterar dokument i ditt index. Om du behöver också att skapa eller uppdatera index, synonymmappningar eller andra resurser på tjänstenivå, använda den `Microsoft.Azure.Search` paketet i stället.
 - `Microsoft.Azure.Search.Service`: Använd det här paketet om du utvecklar automatisering i .NET för att hantera Azure Search-index, synonymmappningar, indexerare, datakällor eller andra resurser för servicenivå. Om du bara behöver fråga eller uppdatera dokument i ditt index kan du använda den `Microsoft.Azure.Search.Data` paketet i stället. Om du behöver alla funktioner i Azure Search kan du använda den `Microsoft.Azure.Search` paketet i stället.
 - `Microsoft.Azure.Search.Common`: Vanliga typer som krävs av Azure Search .NET-bibliotek. Du behöver inte använda det här paketet direkt i dina program. Det är endast avsett att användas som ett beroende.
 
Den här ändringen tekniskt sett större eftersom många typer flyttades mellan sammansättningar. Det är därför återskapa ditt program krävs för att uppgradera till version 5 av SDK: N.

Ett litet antal andra avgörande förändringar i version 5 som kan kräva kod ändrar det förutom återskapa ditt program.

### <a name="removed-obsolete-members"></a>Ta bort föråldrade medlemmar

Du kan se Skapa fel som rör metoder eller egenskaper som har markerats som föråldrad i tidigare versioner och därefter tas bort i version 5. Om du stöter på sådana fel är här hur du löser dem:

- Om du använde den `IndexingParametersExtensions.IndexStorageMetadataOnly` metod, Använd `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` i stället.
- Om du använde den `IndexingParametersExtensions.SkipContent` metod, Använd `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` i stället.

### <a name="removed-preview-features"></a>Borttagna förhandsversionsfunktioner

Om du uppgraderar från version 4.0-förhandsversion till version 5, Tänk på att JSON-matris och CSV parsning stöd för Blob-indexerare har tagits bort eftersom dessa funktioner är fortfarande i förhandsversion. Mer specifikt kan följande metoder i den `IndexingParametersExtensions` klassen har tagits bort:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Om ditt program har ett fast beroende på de här funktionerna kan du inte uppgradera till version 5 av Azure Search .NET SDK. Du kan fortsätta att använda version 4.0-förhandsversion. Men. Tänk på att **rekommenderar vi inte använder förhandsversionen av SDK: er i produktionsprogram**. Funktioner i förhandsversion är för utvärdering endast och kan ändras.

## <a name="conclusion"></a>Sammanfattning
Om du behöver mer information om hur du använder Azure Search .NET SDK kan du läsa den [.NET How-to](search-howto-dotnet-sdk.md).

Vi uppskattar din feedback om SDK. Om du får problem kan passa på att be om hjälp oss på den [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Om du har hittat ett programfel kan du rapportera problemet i den [Azure .NET SDK på GitHub-lagringsplatsen](https://github.com/Azure/azure-sdk-for-net/issues). Se till att din Ärenderubrik med ”[Azure Search]”-prefix.

Tack för att använda Azure Search!

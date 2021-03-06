---
title: Skriva uttryck för attributmappningar i Azure Active Directory | Microsoft Docs
description: Lär dig hur du använder uttrycksmappningar för att omvandla attributvärden till ett acceptabelt format vid automatisk etablering av objekt för SaaS-app i Azure Active Directory.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.openlocfilehash: cf0e94b859b49eddb8e2471d5319b3cc4a2c17ba
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720301"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Skriva uttryck för attributmappningar i Azure Active Directory
När du konfigurerar etablering till ett SaaS-program, är en av typerna av attributmappningar som du kan ange mappningen för en uttryck. För dessa, måste du skriva ett skript-liknande uttryck som hjälper dig att omvandla dina användares data till format som kan användas mer för SaaS-program.

## <a name="syntax-overview"></a>Översikt över syntax
Syntaxen för uttryck för attributmappningar är påminner om Visual Basic för Applications (VBA).

* Hela uttrycket måste definieras när det gäller funktioner, som består av ett namn följt av argument inom parentes: <br>
  *FunctionName (<< argument 1 >>, <<argument N>>)*
* Du kan kapsla funktioner i varandra. Exempel: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* Du kan skicka tre olika typer av argument funktioner:
  
  1. Attribut måste omges av hakparenteser. Till exempel: [attributeName]
  2. Strängkonstanter måste vara inom dubbla citattecken. Till exempel: ”USA”
  3. Andra funktioner. Till exempel: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* För strängkonstanter, om du behöver ett omvänt snedstreck (\) eller citattecken (”) i strängen är måste den föregås symbolen omvänt snedstreck (\). Till exempel ”: företagets namn: \"Contoso\"”

## <a name="list-of-functions"></a>Lista över funktioner
[Lägg till](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [ansluta](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [NormalizeDiacritics](#normalizediacritics) [inte](#not) &nbsp; &nbsp; &nbsp; &nbsp; [Ersätt](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Växel](#switch)

- - -
### <a name="append"></a>Lägg till
**Funktionen:**<br> Append(Source, suffix)

**Beskrivning:**<br> Tar ett strängvärde för källa och lägger till suffixet i slutet av den.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |Vanligtvis namnet på attributet från källobjektet |
| **suffix** |Krävs |Sträng |Den sträng som du vill lägga till i slutet av värdet för datakällan. |

- - -
### <a name="formatdatetime"></a>formatDateTime
**Funktionen:**<br> FormatDateTime (källa, inputFormat, outputFormat)

**Beskrivning:**<br> Tar en datumsträng från ett format och konverterar den till ett annat format.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |Vanligtvis namnet på attributet från källobjektet. |
| **inputFormat** |Krävs |Sträng |Förväntade format för värdet för datakällan. Format som stöds, se [ http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Krävs |Sträng |Formatet för Utdatadatum. |

- - -
### <a name="join"></a>Slå ihop
**Funktionen:**<br> Ansluta till (avgränsare, källa1, källa2...)

**Beskrivning:**<br> JOIN() liknar Append(), förutom att det kan kombinera flera **källa** sträng värden till en sträng och varje värde skiljs åt av en **avgränsare** sträng.

Om en av källvärdena är ett attribut med flera värden och sedan varje värde i attributet kommer att anslutas tillsammans, avgränsade avgränsare värdet.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **avgränsare** |Krävs |Sträng |Strängen används för att avgränsa källvärdena när de sammanfogas till en sträng. Kan vara ”” om det krävs ingen avgränsare. |
| ** källa1... Källan ** |Krävs, variabeln antal gånger |Sträng |Sträng värden kopplas. |

- - -
### <a name="mid"></a>Mid
**Funktionen:**<br> MID (källa, start, length)

**Beskrivning:**<br> Returnerar en understräng av värdet för datakällan. En understräng är en sträng som innehåller bara en del av tecken från Källsträngen.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |Vanligtvis namnet på attributet. |
| **start** |Krävs |heltal |Index i den **källa** sträng som var delsträngen ska börja. Första tecknet i strängen har index 1, andra tecknet ska ha index 2 och så vidare. |
| **Längd** |Krävs |heltal |Delsträngens längd. Om längden slutar utanför den **källa** sträng, returnerar funktionen delsträngen från **starta** indexet till slutet av **källa** sträng. |

- - -
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Funktionen:**<br> NormalizeDiacritics(source)

**Beskrivning:**<br> Kräver ett strängargument. Returnerar strängen, men med eventuella diakritiska tecken ersätts med motsvarande icke-diakritiska tecken. Normalt används för att konvertera förnamn och efternamn som innehåller diakritiska tecken (accenttecken) i juridiska värden som kan användas i olika användaridentifierare, t.ex användarhuvudnamn SAM-kontonamn och e-postadresser.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng | Vanligtvis ett förnamn eller senaste name-attribut |

- - -
### <a name="not"></a>inte
**Funktionen:**<br> Not(Source)

**Beskrivning:**<br> Vänder booleskt värde för den **källa**. Om **källa** värdet är ”*SANT*”, returnerar ”*FALSKT*”. I annat fall returnerar ”*SANT*”.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Booleska sträng |Förväntat **källa** värden är ”True” eller ”False”... |

- - -
### <a name="replace"></a>Ersätt
**Funktionen:**<br> Ersätt (källa, oldValue, regexPattern, regexGroupName, ersättningsvärde, replacementAttributeName, mall)

**Beskrivning:**<br>
Ersätter värden i en sträng. Den fungerar på olika sätt beroende på parametrarna som anges:

* När **oldValue** och **ersättningsvärde** tillhandahålls:
  
  * Ersätter alla förekomster av oldValue i källan med ersättningsvärde
* När **oldValue** och **mall** tillhandahålls:
  
  * Ersätter alla förekomster av den **oldValue** i den **mall** med den **källa** värde
* När **regexPattern**, **regexGroupName**, **ersättningsvärde** tillhandahålls:
  
  * Ersätter alla värden som matchar oldValueRegexPattern i Källsträngen med ersättningsvärde
* När **regexPattern**, **regexGroupName**, **replacementPropertyName** tillhandahålls:
  
  * Om **källa** har inte något värde **källa** returneras
  * Om **källa** har ett värde, använder **regexPattern** och **regexGroupName** att extrahera ersättningsvärdet från egenskapen med **replacementPropertyName** . Ersättningsvärdet returneras som ett resultat

**Parametrar:**<br> 
| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |Vanligtvis namnet på attributet från källobjektet. |
| **oldValue** |Valfri |Sträng |Värdet som ska ersättas i **källa** eller **mallen**. |
| **regexPattern** |Valfri |Sträng |Regex-mönster för det värde som ska ersättas i **källa**. Eller, om du använder replacementPropertyName, mönster och få värdefull information från egendom. |
| **regexGroupName** |Valfri |Sträng |Namnet på gruppen i **regexPattern**. Endast när replacementPropertyName används, ska vi extrahera värdet för den här gruppen som ersättningsvärde från egendom. |
| **Ersättningsvärde** |Valfri |Sträng |Nytt värde som ersätter gamla med. |
| **replacementAttributeName** |Valfri |Sträng |Namnet på attributet som ska användas för ersättning, när källan har inget värde. |
| **mall** |Valfri |Sträng |När **mall** värde anges, vi söker efter **oldValue** i mallen och Ersätt den med värdet för datakällan. |

- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Funktionen:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Beskrivning:**<br> Kräver ett strängargument. Returnerar strängen, men med några diakritiska tecken repalced med motsvarande icke-diakritiska tecken.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Krävs |Sträng |**[appRoleAssignments]**  objekt. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Funktionen:**<br> StripSpaces(source)

**Beskrivning:**<br> Tar bort alla blanksteg (””) tecken från strängen källa.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |**källan** värde att uppdatera. |

- - -
### <a name="switch"></a>Växel
**Funktionen:**<br> Växel (källa, standardvärde, key1, value1, key2, value2,...)

**Beskrivning:**<br> När **källa** värdet matchar en **nyckel**, returnerar **värdet** för som **nyckeln**. Om **källa** värdet matchar inte några nycklar, returnerar **defaultValue**.  **Nyckeln** och **värdet** parametrar måste komma i par. Funktionen förväntar alltid ett jämnt antal parametrar.

**Parametrar:**<br> 

| Namn | Obligatoriskt / upprepande | Typ | Anteckningar |
| --- | --- | --- | --- |
| **Källa** |Krävs |Sträng |**källan** värde att uppdatera. |
| **Standardvärde** |Valfri |Sträng |Standardvärde som ska användas när källan inte matchar några nycklar. Kan vara tom sträng (””). |
| **nyckel** |Krävs |Sträng |**Nyckeln** att jämföra **källa** värde med. |
| **värde** |Krävs |Sträng |Ersättningsvärdet för den **källa** matchar nyckeln. |

## <a name="examples"></a>Exempel
### <a name="strip-known-domain-name"></a>Remsans kända domännamn
Du måste ta bort ett känt domännamn från en användares e-post att hämta ett användarnamn. <br>
Till exempel om domänen är ”contoso.com”, kan du använda följande uttryck:

**Uttryck:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Exempel på indata / utdata:** <br>

* **INDATA** (e-post) ”:john.doe@contoso.com”
* **UTDATA**: ”john.doe”

### <a name="append-constant-suffix-to-user-name"></a>Lägg till konstant suffix till användarnamn
Om du använder en Salesforce-Sandbox kan du behöva lägga till en ytterligare suffix till alla användare innan du synkroniserar dem.

**Uttryck:** <br>
`Append([userPrincipalName], ".test")`

**Exempel indata/utdata:** <br>

* **INDATA**: (userPrincipalName) ”:John.Doe@contoso.com”
* **UTDATA**”:John.Doe@contoso.com.test”

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generera användaralias genom att sammanfoga delar av förnamn, efternamn
Du måste du generera en användare alias genom att först 3 bokstäverna i användarens förnamn och 5 första bokstäverna i användarens efternamn.

**Uttryck:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Exempel indata/utdata:** <br>

* **INDATA** (givenName): ”John”
* **INDATA** (efternamn): ”Berg”
* **UTDATA**: ”JohDoe”

### <a name="remove-diacritics-from-a-string"></a>Ta bort diakritiska tecken från en sträng
Du måste ersätta tecken med accenter med motsvarande tecken som inte innehåller accenttecken.

**Uttryck:** <br>
NormalizeDiacritics([givenName])

**Exempel indata/utdata:** <br>

* **INDATA** (givenName): ”Zoë”
* **UTDATA**: ”Zoe”

### <a name="output-date-as-a-string-in-a-certain-format"></a>Utdatadatum som en sträng i ett visst format
Du vill skicka datum till ett SaaS-program i ett visst format. <br>
Exempelvis kan du formatera datum för ServiceNow.

**Uttryck:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Exempel indata/utdata:**

* **INDATA** (extensionAttribute1): ”20150123105347.1Z”
* **UTDATA**: ”2015-01-23”

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Ersätt ett värde baserat på fördefinierade uppsättning med alternativ
Du behöver definiera tidszonen för användaren baserat på delstatskod som lagras i Azure AD. <br>
Om delstatskod inte matchar någon av de fördefinierade alternativ, använder du standardvärdet ”Australien/Sydney”.

**Uttryck:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Exempel indata/utdata:**

* **INDATA** (tillstånd): ”QLD”
* **UTDATA**: ”Australien/Brisbane”

## <a name="related-articles"></a>Relaterade artiklar
* [Automatisera användaren etablering/avetablering för SaaS-appar](user-provisioning.md)
* [Anpassa attributmappningar för etableringen av användare](customize-application-attributes.md)
* [Omfångsfilter för etableringen av användare](define-conditional-rules-for-provisioning-user-accounts.md)
* [Använda SCIM för att aktivera automatisk etablering av användare och grupper från Azure Active Directory till program](use-scim-to-provision-users-and-groups.md)
* [Kontoetableringsmeddelanden](user-provisioning.md)
* [Lista över guider om hur du integrerar SaaS-appar](../saas-apps/tutorial-list.md)


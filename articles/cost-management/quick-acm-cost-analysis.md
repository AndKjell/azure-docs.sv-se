---
title: Snabbstart – Utforska Azure-kostnader med kostnadsanalys | Microsoft Docs
description: Den här snabbstarten hjälper dig att använda kostnadsanalys för att utforska och analysera dina Azure-organisationskostnader.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/10/2018
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 12b7a605350b07565660e9e4d1334b286aa5ac00
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079114"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Snabbstart: Utforska och analysera kostnader med kostnadsanalys

För att kunna kontrollera och optimera dina Azure-kostnader på rätt sätt behöver du förstå var kostnaderna har sitt ursprung i organisationen. Det är även bra att veta hur mycket pengar dina tjänster kostar och vilka miljöer och system de stödjer. Insyn i hela spektrumet av kostnader är mycket viktigt för kunna förstå organisationens utgiftsmönster. Utgiftsmönster kan användas för att tillämpa kostnadskontrollmekanismer, till exempel budgetar.

I den här snabbstarten använder du kostnadsanalys för att utforska och analysera dina organisationskostnader. Du kan visa aggregerade kostnader efter organisation för att se var kostnader sker över tid och för att identifiera utgiftstrender. Du kan se ackumulerade kostnader över tid för att beräkna månatliga, kvartalsvisa eller årliga kostnadstrender mot en budget. En budget gör det lättare att hålla sig till ekonomiska begränsningar. En budget används även till att visa dagliga och månatliga kostnader för att isolera oregelbundenheter vad gäller utgifter. Dessutom kan du ladda ned den aktuella rapportens data för ytterligare analys eller för användning i ett externt system.

I den här snabbstarten lär du dig att:

- Granska kostnader i kostnadsanalys
- Anpassa kostnadsvyer
- Ladda ned kostnadsanalysdata


## <a name="prerequisites"></a>Nödvändiga komponenter

Kostnadsanalys är tillgängligt för alla [Enterprise-avtalskunder (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). Du måste minst ha skrivskyddad åtkomst till ett eller flera av följande omfång för att visa kostnadsdata.

- *Faktureringskontots* omfattning definieras vid https://ea.azure.com, och företagsadministratörsbehörighet krävs. Ingen EA-förinställning krävs. Faktureringsinformation i kostnadsanalys sammanställs för alla prenumerationer i Enterprise-avtalet. Faktureringskontot kallas ofta *Företagsavtal* eller *Registrering*.

- Omfånget *avdelning* definieras vid https://ea.azure.com och kräver avdelningsadministratörsbehörighet. Inställningen för **DA-visningsdebitering** i EA-portalen måste aktiveras. Faktureringsinformation i kostnadsanalys sammanställs för alla prenumerationer som hör till det registreringskonto som är kopplat till avdelningen.

- Omfånget för *registreringskontot* definieras vid https://ea.azure.com och kontoinnehavarbehörighet krävs. Inställningen **AO-visningsdebitering** i EA-portalen måste aktiveras. Faktureringsinformation i kostnadsanalys sammanställs för alla prenumerationer som hör till registreringskontot. Registreringskontot kallas ofta *kontoägare*.

- Omfånget *hanteringsgrupp* definieras vid https://portal.azure.com, och behörighetsnivån för Cost Management Reader (eller Reader) krävs. Inställningen **AO-visningsdebitering** i EA-portalen måste aktiveras. Faktureringsinformation i kostnadsanalys sammanställs för alla prenumerationer under hanteringsgruppen.

- Omfånget för *prenumeration* definieras vid https://portal.azure.com, och behörighetsnivån för Cost Management Reader (eller Reader) krävs. Inställningen **AO-visningsdebitering** i EA-portalen måste aktiveras. Faktureringsinformation i kostnadsanalys sammanställs för alla resurser och resursgrupper i prenumerationen.

- Omfånget *resursgrupp* definieras vid https://portal.azure.com, och behörighetsnivån Cost Management Reader (eller Reader) krävs. Inställningen **AO-visningsdebitering** i EA-portalen måste aktiveras. Faktureringsinformation i kostnadsanalys sammanställs för alla resurser och i resursgruppen.



Mer information om hur du konfigurerar inställningar för **DA-visningsdebitering** och **AO-visningsdebitering** finns i [Aktivera åtkomst till kostnader](../billing/billing-enterprise-mgmt-grp-troubleshoot-cost-view.md#enabling-access-to-costs).

## <a name="sign-in-to-azure"></a>Logga in på Azure

- Logga in på Azure Portal på http://portal.azure.com.

## <a name="review-costs-in-cost-analysis"></a>Granska kostnader i kostnadsanalys

Om du vill granska dina kostnader med kostnadsanalys går du i Azure-portalen till **Kostnadshantering + fakturering** &gt; **Kostnadshantering** &gt; **Ändra omfång**, väljer ett omfång och klickar sedan på **Välj**.

Det omfång som du väljer används i hela Cost Management för att ge datakonsolidering och styra åtkomsten till kostnadsinformation. När du använder omfång så använder du inte flerval för dem. I stället väljer du ett större omfång som andra ackumuleras till, och sedan filtrerar du ned till det du vill. Detta är viktigt att förstå eftersom vissa användare inte ska ha åtkomst till ett överordnat omfång som underordnade omfång ackumuleras till.

Klicka på **Öppna kostnadsanalys**.

Den initiala kostnadsanalysvyn innehåller följande områden:

**Total** (Totalt) – visar den totala kostnaden för aktuell månad.

**Budget** – visar den planerade utgiftsgränsen för det valda omfånget, om en sådan är tillgänglig.

**Accumulated cost** (Ackumulerad kostnad) – visar de totala ackumulerade dagliga utgifterna med start i början av månaden. När du har [skapat en budget](tutorial-acm-create-budgets.md) för ditt faktureringskonto eller din prenumeration kan du snabbt se din utgiftstrend jämfört med budgeten. Hovra över ett datum om du vill visa den ackumulerade kostnaden för den dagen.

**Pivotdiagram (ringdiagram)** – visar dynamiska pivoter som delar upp den totala kostnaden enligt en gemensam uppsättning standardegenskaper. De visar den högsta till minsta kostnaden som ackumulerats för den aktuella månaden. Du kan ändra pivotdiagram när som helst genom att välja en annan pivot. Kostnaderna kategoriseras efter: tjänst (mätarkategori), plats (region) och underordnat omfång som standard. Exempel: registreringskonton under faktureringskonton, resursgrupper under prenumerationer och resurser under resursgrupper.

![Initial vy över kostnadsanalys](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Anpassa kostnadsvyer

Standardvyn ger snabba svar på vanliga frågor som:

- Hur mycket spenderade jag?
- Följer jag budgeten?

Det finns dock många fall där du behöver djupare analys. Anpassning startar överst på sidan med valet av datum.

Kostnadsanalys visar data för den aktuella månaden som standard. Använd datumväljaren för att snabbt växla till: föregående månad, denna månad, detta kalenderkvartal, detta kalenderår eller ett eget datumintervall som du väljer. Att välja föregående månaden är det snabbaste sättet att analysera din senaste Azure-faktura och enkelt stämma av avgifter. Alternativen för aktuellt kvartal och år är till hjälp för att spåra kostnader mot mer långsiktiga budgetar. Du kan även välja ett annat datumintervall. Till exempel kan du välja en enstaka dag, de senaste sju dagarna eller något annat så långt tillbaka som ett år före den aktuella månaden.

![Datumväljare](./media/quick-acm-cost-analysis/date-selector.png)

Kostnadsanalysen visar **ackumulerade** kostnader som standard. Ackumulerade kostnader omfattar alla kostnader för varje dag plus föregående dagar, för en ständigt växande vy över dina dagliga ackumulerade kostnader. Den här vyn är optimerad för att visa hur du trendar mot en budget för det valda tidsintervallet.

Det finns även den **dagsvyn**, som visar kostnaderna för varje dag. Dagsvyn visar inte någon tillväxttrend. Vyn har utformats för att visa oregelbundenheter såsom toppar eller dalar i utgifter från dag till dag. Om du har valt en budget visar dagsvyn även en uppskattning av hur din dagliga budget kan se ut. När dina dagliga kostnader konsekvent är över den beräknade dagliga budgeten kan du förvänta dig att du överskrider din månatliga budget. Den beräknade dagliga budgeten är bara ett sätt att visualisera din budget på en lägre nivå. När det förekommer variationer i de dagliga kostnaderna är jämförelsen mellan den uppskattade dagliga budgeten och den månatliga budgeten mindre exakt.

![Dagsvy](./media/quick-acm-cost-analysis/daily-view.png)

Du kan **gruppera efter** för att välja en gruppkategori och ändra data som visas i diagrammet med total area längst upp. Med gruppering kan du snabbt se hur dina utgifter kategoriseras efter resurstyp. Här är en vy över Azure-tjänstkostnaderna för en vy över föregående månad.

![Grupperad daglig ackumulerad vy](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Pivotdiagram under vyn Totalt längst upp visar vyer för olika grupperings- och filtreringskategorier. När du väljer en gruppkategori visas den fullständiga datamängden för vyn Totalt längst ned i vyn. Här är ett exempel för resursgrupper.

![Fullständiga data för aktuell vy](./media/quick-acm-cost-analysis/full-data-set.png)

I föregående bild visas resursgruppnamn. Visning av taggar för resurser är inte tillgängligt i någon av vyerna, filtren eller grupperna för kostnadsanalys.

När du grupperar kostnader efter ett specifikt attribut visas de tio viktigaste kostnadsfaktorerna från högsta till lägsta. Om det finns fler än tio grupper visas de nio viktigaste grupperna plus gruppen **Others** (Övriga), där alla övriga grupper ingår.

*Klassiska* virtuella datorer (Azure Service Management eller ASM), nätverk och lagringsresurser delar inte detaljerad faktureringsinformation. De slås samman som **klassiska tjänster** när du grupperar kostnader.


## <a name="download-cost-analysis-data"></a>Ladda ned kostnadsanalysdata

Du kan **ladda ned** information från kostnadsanalys att generera en CSV-fil för alla data som för närvarande visas i Azure-portalen. Alla filter eller grupper som du tillämpar ingår i filen. Underliggande data för totaldiagrammet längst upp som inte visas aktivt ingår i CSV-filen.

## <a name="next-steps"></a>Nästa steg

Gå vidare till den första självstudien om du vill lära dig att skapa och hantera budgetar.

> [!div class="nextstepaction"]
> [Skapa och hantera budgetar](tutorial-acm-create-budgets.md)

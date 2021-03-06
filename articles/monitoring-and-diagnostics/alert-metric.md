---
title: Skapa, visa och hantera mått aviseringar med Azure Monitor
description: Lär dig hur du använder Azure-portalen eller CLI för att skapa, visa och hantera måttaviseringsregler.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: a6c38ce465b8e4e68694f2a6cc0d467e34a09852
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388179"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Skapa, visa och hantera aviseringar för mått med Azure Monitor

Måttaviseringar i Azure Monitor är ett sätt att få ett meddelande när en av dina mått överskrider ett tröskelvärde. Måttaviseringar fungerar på flera olika flerdimensionella plattform mått, anpassade mått, Application Insights standard och anpassade mått. I den här artikeln beskriver vi hur du skapar, visa och hantera måttaviseringsregler via Azure portal och Azure CLI. Du kan också skapa måttaviseringsregler med hjälp av Azure Resource Manager-mallar som beskrivs i [en separat artikel](monitoring-enable-alerts-using-template.md).

Du kan lära dig mer om hur mått aviseringar fungerar från [översikt över aviseringar i mått](alert-metric-overview.md).

## <a name="create-with-azure-portal"></a>Skapa med Azure portal

Följande procedur beskriver hur du skapar en måttaviseringsregel i Azure-portalen:

1. I [Azure-portalen](https://portal.azure.com), klicka på **övervakaren**. Monitor-bladet konsoliderar alla dina övervakningsinställningar och -data i en vy.

2. Klicka på **aviseringar** klickar **+ ny aviseringsregel**.

    > [!TIP]
    > De flesta resursbladen har också **aviseringar** i sina resursmenyn under **övervakning**, du kan skapa aviseringar därifrån också.

3. Klicka på **Välj mål**, Välj en målresurs som du vill ändra i fönstret kontext som läser in. Använd **prenumeration** och **resurstyp** listrutor för att hitta resursen som du vill övervaka. Du kan också använda sökfältet för att hitta din resurs.

4. Om den valda resursen har mått som du kan skapa aviseringar, **tillgängliga signaler** på nederkant höger innehåller mått. Du kan visa en fullständig lista över resurstyper som stöds för måttaviseringar i den här [artikel](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported)

5. När du har valt en målresurs, klickar du på **lägga till villkor**

6. Du kommer se en lista över signaler som stöds för resursen, Välj det mått som du vill skapa en avisering på.

7. Du ser ett diagram för måttet för de senaste 6 timmarna. Definiera den **Period**, **frekvens**, **operatorn** och **tröskelvärdet**, det avgör den logik som måttaviseringsregel utvärdera.

8. Med hjälp av måttdiagram bestämma du vad kan vara en rimlig tröskelvärdet.

9. Du kan också om måttet har dimensioner kan visas dimensioner tabellen som visas. Välj ett eller flera värden per dimension. Metrisk varning körs utvärdera villkoret för alla kombinationer av värden som väljs. [Mer information om hur aviseringar på flerdimensionella mått fungerar](alert-metric-overview.md). Du kan också **Välj \***  för någon av dimensionerna. **Välj \***  kommer dynamiskt skala markeringen till alla nuvarande och framtida värden för en dimension.

10. Klicka på **Klar**

11. Du kan också lägga till ytterligare villkor om du vill övervaka en komplex aviseringsregel

12. Fyll i **aviseringsinformation** som **avisering Regelnamn**, **beskrivning** och **allvarlighetsgrad**

13. Lägg till en åtgärdsgrupp till aviseringen genom att välja en befintlig åtgärdsgrupp eller skapa en ny åtgärdsgrupp.

14. Klicka på **klar** att spara måttaviseringsregel.

> [!NOTE]
> Metrisk varning regler som skapats via portalen skapas i samma resursgrupp som målresursen.

## <a name="view-and-manage-with-azure-portal"></a>Visa och hantera med Azure portal

Du kan visa och hantera måttaviseringsregler med hjälp av bladet hantera regler under aviseringar. Proceduren nedan visar hur du visar dina måttaviseringsregler och redigera en av dem.

1. I Azure-portalen går du till **Övervakare**

2. Klicka på **aviseringar** och **hantera regler**

3. I den **hantera regler** bladet, du kan visa alla dina aviseringsregler mellan prenumerationer. Du kan filtrera regler med ytterligare **resursgrupp**, **resurstyp** och **Resource**. Om du vill se bara måttaviseringar väljer **signalera typ** som mått.

    > [!TIP]
    > I den **hantera regler** bladet, du kan välja flera Varningsregler och aktivera/inaktivera dem. Detta kan vara användbart när vissa målresurser behöver placeras under Underhåll

4. Klicka på namnet på måttet varningsregeln som du vill redigera

5. I den Redigera regeln klickar du på den **Avisera kriterier** du vill redigera. Du kan ändra måttet, tröskelvärde och andra fält som krävs

    > [!NOTE]
    > Du kan inte redigera den **målresurs** och **avisering Regelnamn** när metrisk varning har skapats.

6. Klicka på **klar** att spara dina ändringar.

## <a name="with-azure-cli"></a>Med Azure CLI

I föregående avsnitt beskrivs hur du skapar, visa och hantera måttaviseringsregler med hjälp av Azure portal. Det här avsnittet beskrivs hur du gör samma sak med hjälp av plattformsoberoende [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Snabbaste sättet att börja använda Azure CLI är via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). I den här artikeln ska vi använda cloudshell.

1. Gå till Azure portal, klickar du på **Cloud shell**.

2. Du kan använda kommandon i Kommandotolken ``--help`` alternativet om du vill veta mer om kommandot och hur du använder den. Till exempel visar följande kommando listan över kommandon som är tillgängliga för att skapa, visa och hantera aviseringar för mått

    ```azurecli
    az monitor metrics alert --help
    ```

3. Du kan skapa en enkel måttaviseringsregel som övervakar om den genomsnittliga procent CPU på en virtuell dator är större än 70

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90"
    ```

4. Du kan visa alla måttaviseringar i en resursgrupp med hjälp av följande kommando

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Du kan se information om en viss måttaviseringsregel med namn eller resurs-ID för regeln.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. Du kan inaktivera en måttaviseringsregel med följande kommando.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} -enabled false
    ```

7. Du kan ta bort en måttaviseringsregel med följande kommando.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} -enabled false
    ```

## <a name="next-steps"></a>Nästa steg

- [Skapa måttaviseringar med hjälp av Azure Resource Manager-mallar](monitoring-enable-alerts-using-template.md).
- [Förstå hur mått aviseringar work](alert-metric-overview.md).
- [Förstå web hook schemat för måttaviseringar](monitoring-near-real-time-metric-alerts.md#payload-schema)

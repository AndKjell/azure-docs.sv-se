---
title: Självstudie – Skala en skalningsuppsättning automatiskt med Azure CLI | Microsoft Docs
description: Läs hur du använder Azure CLI för att automatiskt skala en VM-skalningsuppsättning allteftersom CPU-kraven varierar
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/18/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7c3d536cd4fb99d6d83b973989279d289e8434a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995401"
---
# <a name="tutorial-automatically-scale-a-virtual-machine-scale-set-with-the-azure-cli"></a>Självstudie: Skala en VM-skalningsuppsättning automatiskt med Azure CLI

När du skapar en skalningsuppsättning, definierar du antalet virtuella datorinstanser som du vill köra. När ditt program behöver ändras, kan du automatiskt öka eller minska antalet virtuella datorinstanser. Möjligheten att skala automatiskt låter dig hålla dig uppdaterad med kundernas behov eller svara på ändringar i programprestandan under hela livscykeln för din app. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Använd automatisk skalning med en skalningsuppsättning
> * Skapa och använd regler för automatisk skalning
> * Belastningstesta virtuella datorinstanser och utlös regler för automatisk skalning
> * Skala tillbaka automatiskt när efterfrågan minskar

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt krävs Azure CLI version 2.0.32 eller senare i de här självstudierna. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI]( /cli/azure/install-azure-cli).

## <a name="create-a-scale-set"></a>Skapa en skalningsuppsättning

Skapa en resursgrupp med [az group create](/cli/azure/group#create) enligt följande:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Skapa nu en skalningsuppsättning för en virtuell dator med [az vmss create](/cli/azure/vmss#create). Följande exempel skapar en skalningsuppsättning instansantalet *2* och genererar SSH-nycklar om de inte redan finns:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="define-an-autoscale-profile"></a>Definiera en autoskalningsprofil

Om du vill aktivera autoskalning för en skalningsuppsättning börjar du med att definiera en autoskalningsprofil. Den här profilen definierar skalningsuppsättningens förvalda, lägsta och högsta kapacitet. Med hjälp av dessa restriktioner kan du begränsa kostnaderna genom att inte skapa VM-instanser kontinuerligt, samtidigt som du kan balansera godtagbara prestanda med minsta antal instanser som bevaras vid en nedskalning. Skapa en autoskalningsprofil med [az monitor autoscale create](/cli/azure/monitor/autoscale#az-monitor-autoscale-create). Följande exempel anger standard- och minimumkapacitet på *2* virtuella datorinstanser och högst *10*:

```azurecli-interactive
az monitor autoscale create \
  --resource-group myResourceGroup \
  --resource myScaleSet \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --name autoscale \
  --min-count 2 \
  --max-count 10 \
  --count 2
```

## <a name="create-a-rule-to-autoscale-out"></a>Skapa en regel för att automatiskt skala ut

Om dina programkrav ökar, ökar även belastningen på de virtuella datorinstanserna i din skalningsuppsättning. Om den här ökade belastningen är konsekvent istället för bara en kortsiktig efterfrågan, kan du konfigurera regler för automatisk skalning för att öka antalet virtuella datorinstanser i skalningsuppsättningen. När dessa virtuella datorinstanser skapas och dina program distribueras, börjar skalningsuppsättningen att distribuera trafik till dem via lastbalanseraren. Du kan styra vilka mått som ska övervakas, som CPU eller disk, hur länge programbelastningen måste uppfylla ett visst tröskelvärde och hur många virtuella datorinstanser som ska läggas till skalningsuppsättningen.

Nu ska vi skapa en regel med [az monitor autoscale rule create](/cli/azure/monitor/autoscale/rule#az-monitor-autoscale-rule-create) som ökar antalet VM-instanser i en skalningsuppsättning när den genomsnittliga CPU-belastningen är större än 70 % under en 5-minutersperiod. När regeln utlöses, ökar antalet virtuella datorinstanser med tre.

```azurecli-interactive
az monitor autoscale rule create \
  --resource-group myResourceGroup \
  --autoscale-name autoscale \
  --condition "Percentage CPU > 70 avg 5m" \
  --scale out 3
```

## <a name="create-a-rule-to-autoscale-in"></a>Skapa en regel för att automatiskt skala in

På kvällar eller helger, kan efterfrågan på ditt program minska. Om den här minskade belastningen är konsekvent över en tidsperiod, kan du konfigurera regler för automatisk skalning för att minska antalet virtuella datorinstanser i skalningsuppsättningen. Den här åtgärden för skala in minskar kostnaden för att köra din skalningsuppsättningen eftersom du bara köra de antal instanser som krävs för att uppfylla den aktuella efterfrågan.

Skapa en till regel med [az monitor autoscale rule create](/cli/azure/monitor/autoscale/rule#az-monitor-autoscale-rule-create) som minskar antalet VM-instanser i en skalningsuppsättning när den genomsnittliga CPU-belastningen faller under 30 % under en 5-minutersperiod. Följande exempel definierar regeln för att skala ned antalet VM-instanser med en:

```azurecli-interactive
az monitor autoscale rule create \
  --resource-group myResourceGroup \
  --autoscale-name autoscale \
  --condition "Percentage CPU < 30 avg 5m" \
  --scale in 1
```

## <a name="generate-cpu-load-on-scale-set"></a>Generera CPU-belastning på skalningsuppsättningen

Om du vill testa reglerna för automatisk skalning kan du generera lite CPU-belastning på de virtuella datorinstanserna i skalningsuppsättningen. Den här simulerade CPU-belastningen gör att de automatiska skalningarna skalar ut och ökar antalet virtuella datorinstanser. När den simulerade CPU-belastningen sedan minskar så skalar reglerna för automatisk skalning in och minskar antalet virtuella datorinstanser.

Först, listar du adressen och portarna för att ansluta till virtuella datorinstanser i en skalningsuppsättning med [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info):

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```

Följande exempelutdata visar instansnamnet, den offentliga IP-adressen för lastbalanseraren och portnummer som NAT (Network Address Translation)-regler vidarebefordrar trafik till:

```json
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

SSH till din första virtuella datorinstans. Ange din egen offentliga IP-adress och portnummer med parametern `-p`, som det visas i föregående kommando:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

När du är inloggad, installerar du **stress**-verktyget. Starta *10* **stress**-arbetare som genererar CPU-belastning. De här arbetarna kör i *420* sekunder, vilket räcker för att få reglerna för automatisk skalning att implementera den önskade åtgärden.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

När **stress** visar utdata som liknar *stress: info: [2688] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, trycker du på *Retur* för att återgå till prompten.

Kontrollera att **stress** genererar CPU-belastning genom att granska den aktiva systembelastningen med **top**-verktyget:

```azuecli-interactive
top
```

Avsluta **top**, stäng sedan anslutningen till den virtuella datorinstansen. **stress** fortsätter att köras på den virtuella datorinstansen.

```azurecli-interactive
Ctrl-c
exit
```

Anslut till en andra virtuell datorinstans med det portnummer som listas från den föregående [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info):

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50003
```

Installera och kör **stress** och starta sedan tio arbetare på den här andra virtuella datorinstansen.

```azurecli-interactive
sudo apt-get -y install stress
sudo stress --cpu 10 --timeout 420 &
```

När **stress** återigen visar utdata som liknar *stress: info: [2713] dispatching hogs: 10 cpu, 0 io, 0 vm, 0 hdd*, trycker du på *Retur* för att återgå till prompten.

Stäng din anslutning till den andra virtuella datorinstansen. **stress** fortsätter att köras på den virtuella datorinstansen.

```azurecli-interactive
exit
```

## <a name="monitor-the-active-autoscale-rules"></a>Övervaka de aktiva reglerna för automatisk skalning

Du övervakar antalet virtuella datorinstanser i din skalningsuppsättning med **watch**. Det tar fem minuter innan autoskalningsreglerna börjar utskalningen som svar på CPU-belastningen som genereras av **stress** på var och en av VM-instanserna:

```azurecli-interactive
watch az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

När CPU-tröskelvärdet har uppnåtts, ökar reglerna för automatisk skalning antalet virtuella datorinstanser i skalningsuppsättningen. Följande utdata visar tre virtuella datorer som skapats när skalningsuppsättningen skalar ut automatiskt:

```bash
Every 2.0s: az vmss list-instances --resource-group myResourceGroup --name myScaleSet --output table

  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            myResourceGroup  4f92f350-2b68-464f-8a01-e5e590557955
           2  True                  eastus      myScaleSet_2  Succeeded            myResourceGroup  d734cd3d-fb38-4302-817c-cfe35655d48e
           4  True                  eastus      myScaleSet_4  Creating             myResourceGroup  061b4c90-0d73-49fc-a066-19eab0b3d95c
           5  True                  eastus      myScaleSet_5  Creating             myResourceGroup  4beff8b9-4e65-40cb-9652-43899309da27
           6  True                  eastus      myScaleSet_6  Creating             myResourceGroup  9e4133dd-2c57-490e-ae45-90513ce3b336
```

När **stress** stoppar på den första virtuella datorn, återgår den genomsnittliga CPU-belastningen till normal. Efter 5 minuter till, skalar reglerna för automatisk skalning in antalet virtuella datorinstanser. Skalan in-åtgärder tar först bort de virtuella datorinstanserna med de högsta ID. När en skalningsuppsättning använder tillgänglighetsuppsättningar eller tillgänglighetszoner fördelas skala in-åtgärder jämnt över dessa VM-instanser. Följande exempelutdata visar en VM-instans som tas bort eftersom skalningsuppsättningen skalar in automatiskt:

```bash
           6  True                  eastus      myScaleSet_6  Deleting             myResourceGroup  9e4133dd-2c57-490e-ae45-90513ce3b336
```

Avsluta *watch* med `Ctrl-c`. Skalningsuppsättningen fortsätter att skala in var 5:e minut och tar bort en VM-instans tills att det lägsta instansantalet på två har uppnåtts.

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill ta bort din skalningsuppsättning och ytterligare resurser så tar du bort resursgruppen och alla dess resurser med [az group delete](/cli/azure/group#az_group_delete). Parametern `--no-wait` återför kontrollen till kommandotolken utan att vänta på att uppgiften slutförs. Parametern `--yes` bekräftar att du vill ta bort resurserna utan att tillfrågas ytterligare en gång.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nästa steg

I den här självstudien läste du om hur du automatiskt kan skala in eller ut en skalningsuppsättning med Azure CLI:

> [!div class="checklist"]
> * Använd automatisk skalning med en skalningsuppsättning
> * Skapa och använd regler för automatisk skalning
> * Belastningstesta virtuella datorinstanser och utlös regler för automatisk skalning
> * Skala tillbaka automatiskt när efterfrågan minskar

Fler exempel på VM-skalningsuppsättningar i praktiken finns i följande exempelskript för Azure CLI:

> [!div class="nextstepaction"]
> [Skriptexempel på skalningsuppsättningar för Azure CLI](cli-samples.md)

---
title: Självstudie – Skapa och hantera en VM-skalningsuppsättning i Azure | Microsoft Docs
description: Läs hur du använder Azure CLI för att skapa en VM-skalningsuppsättning, tillsammans med vissa vanliga hanteringsuppgifter, till exempel att starta och stoppa en instans, eller ändra kapaciteten för en skalningsuppsättning.
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
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f00f7934bfb78c16f9e93dc2fcd94025e5f806a1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993303"
---
# <a name="tutorial-create-and-manage-a-virtual-machine-scale-set-with-the-azure-cli"></a>Självstudie: Skapa och hantera en VM-skalningsuppsättning med Azure CLI
Med en VM-skalningsuppsättning kan du distribuera och hantera en uppsättning identiska, virtuella datorer med automatisk skalning. Under livscykeln för en VM-skalningsuppsättning kan du behöva köra en eller flera hanteringsuppgifter. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa och anslut till en VM-skalningsuppsättning
> * Välja och använda VM-avbildningar
> * Visa och använd specifika virtuella datorstorlekar
> * Skala en skalningsuppsättning automatiskt
> * Utför vanliga hanteringsåtgärder för skalningsuppsättningar

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt, måste du köra Azure CLI version 2.0.29 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Skapa en resursgrupp
En Azure-resursgrupp är en logisk container där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas före en VM-skalningsuppsättning. Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#az_group_create). I det här exemplet skapas en resursgrupp med namnet *myResourceGroup* i regionen *eastus*. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Resursgruppens namn anges när du skapar eller ändrar en skalningsuppsättning under den här självstudien.


## <a name="create-a-scale-set"></a>Skapa en skalningsuppsättning
Du skapar en VM-skalningsuppsättning med kommandot [az vmss create](/cli/azure/vmss#az_vmss_create). I följande exempel skapas en skalningsuppsättning med namnet *myScaleSet* som genererar SSH-nycklar om det inte redan finns:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

Det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser. För att distribuera trafik till flera virtuella datorinstanser så skapas även en lastbalanserare.


## <a name="view-the-vm-instances-in-a-scale-set"></a>Visa de virtuella datorinstanserna i en skalningsuppsättning
Om du vill visa en lista med virtuella datorinstanser i en skalningsuppsättning, använder du [az vmss list-instances](/cli/azure/vmss#az_vmss_list_instances) på följande sätt:

```azurecli-interactive
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

Följande exempelutdata visar två virtuella datorinstanser i skalningsuppsättningen:

```bash
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup    VmId
------------  --------------------  ----------  ------------  -------------------  ---------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUP  c059be0c-37a2-497a-b111-41272641533c
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUP  ec19e7a7-a4cd-4b24-9670-438f4876c1f9
```


Den första kolumnen i utdata visar en *InstanceId*. Om du vill visa mer information om en specifik VM-instans, lägger du till parametern `--instance-id` till [az vmss get-instance-view](/cli/azure/vmss#az_vmss_get_instance_view). Följande exempel visar information om den virtuella datorinstansen *1*:

```azurecli-interactive
az vmss get-instance-view \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --instance-id 1
```


## <a name="list-connection-information"></a>Lista anslutningsinformation
En offentlig IP-adress är tilldelad till lastbalanseraren som dirigerar trafik till de enskilda virtuella datorinstanserna. Som standard läggs NAT (Network Address Translation)-regler till Azure-lastbalanseraren som vidarebefordrar fjärranslutningstrafik till varje virtuell dator på en viss port. Om du vill ansluta till de virtuella datorinstanserna i en skalningsuppsättning, kan du skapa en fjärranslutning till en tilldelad offentlig IP-adress och portnummer.

Lista adressen och portarna för att ansluta till virtuella datorinstanser i en skalningsuppsättning med [az vmss list-instance-connection-info](/cli/azure/vmss#az_vmss_list_instance_connection_info):

```azurecli-interactive
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```

Följande exempelutdata visar instansnamnet, den offentliga IP-adressen för lastbalanseraren och portnummer som NAT-regler vidarebefordrar trafik till:

```bash
{
  "instance 1": "13.92.224.66:50001",
  "instance 3": "13.92.224.66:50003"
}
```

SSH till din första virtuella datorinstans. Ange din offentliga IP-adress och portnummer med parametern `-p`, som det visas i föregående kommando:

```azurecli-interactive
ssh azureuser@13.92.224.66 -p 50001
```

Efter att du loggat in på den virtuella datorinstansen kan du utföra vissa manuella konfigurationsändringar efter behov. För tillfället stänger du SSH-sessionen som vanligt:

```bash
exit
```


## <a name="understand-vm-instance-images"></a>Förstå avbildningar av virtuella datorinstanser
När du skapade en skalningsuppsättning i början av självstudien, angavs en `--image` av *UbuntuLTS* för de virtuella datorinstanserna. Azures marknadsplats inkluderar flera avbildningar som kan användas för att skapa virtuella datorinstanser. Du kan lista de vanligaste avbildningarna med kommandot [az vm image list](/cli/azure/vm/image#az_vm_image_list).

```azurecli-interactive
az vm image list --output table
```

Följande visas exempelutdata visar de vanligaste virtuella datoravbildningarna i Azure. *UrnAlias* kan användas för att ange en av dessa vanliga avbildningar när du skapar en skalningsuppsättning.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
```

Om du vill visa en fullständig lista, lägger du till `--all`-argumentet. Listan med avbildningar kan även filtreras efter `--publisher` eller `–-offer`. I följande exempel är listan filtrerad så att den visar alla avbildningar med ett erbjudande som överensstämmer med *CentOS*:

```azurecli-interactive
az vm image list --offer CentOS --all --output table
```

Följande komprimerade utdata visar några av de tillgängliga avbildningarna för CentOS 7.3:

```azurecli-interactive 
Offer    Publisher   Sku   Urn                                 Version
-------  ----------  ----  ----------------------------------  -------------
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20161221   7.3.20161221
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170421   7.3.20170421
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170517   7.3.20170517
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170612   7.3.20170612
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170707   7.3.20170707
CentOS   OpenLogic   7.3   OpenLogic:CentOS:7.3:7.3.20170925   7.3.20170925
```

Om du vill distribuera en virtuell dator med en viss avbildning, använder du värdet i *Urn*-kolumnen. När du anger avbildningen så kan avbildningens versionsnummer ersättas med *latest*, vilket väljer den senaste versionen av distributionen. Följande exempel använder argumentet `--image` för att ange den senaste versionen av en CentOS 7.3-avbildning.

Eftersom det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser så behöver du inte distribuera följande skalningsuppsättning:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSetCentOS \
  --image OpenLogic:CentOS:7.3:latest \
  --admin-user azureuser \
  --generate-ssh-keys
```


## <a name="understand-vm-instance-sizes"></a>Förstå storlekar för virtuella datorinstanser
Storleken på en virtuell datorinstans, eller *SKU*, fastställer mängden beräkningsresurser som CPU, GPU och minne som görs tillgängliga för den virtuella datorinstansen. Virtuella datorinstanser i en skalningsuppsättning måste ha lämplig storlek för förväntad arbetsbelastning.

### <a name="vm-instance-sizes"></a>Storlekar på virtuella datorinstanser
Följande tabell kategoriserar vanliga virtuella datorstorlekar i användningsfall.

| Typ                     | Normala storlekar           |    Beskrivning       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Generellt syfte](../virtual-machines/linux/sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Balanserat förhållande mellan processor och minne. Perfekt för utveckling eller test samt små till medelstora lösningar för program och data.  |
| [Beräkningsoptimerad](../virtual-machines/linux/sizes-compute.md)   | Fs, F             | Högt förhållande mellan processor och minne. Bra för program med medelhög trafik, nätverkstillämpningar och batchprocesser.        |
| [Minnesoptimerad](../virtual-machines/linux/sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Högt förhållande mellan minne och kärna. Utmärkt för relationsdatabaser, mellanstora till stora cacheminnen och minnesinterna analyser.                 |
| [Lagringsoptimerad](../virtual-machines/linux/sizes-storage.md)      | Ls                | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser.                                                         |
| [GPU](../virtual-machines/linux/sizes-gpu.md)          | NV, NC            | Virtuella specialdatorer som är avsedda för tung grafisk rendering och videoredigering.       |
| [Höga prestanda](../virtual-machines/linux/sizes-hpc.md) | H, A8-11          | Virtuella datorer med de kraftfullaste processorerna och nätverksgränssnitt för stora dataflöden (RDMA). 

### <a name="find-available-vm-instance-sizes"></a>Hitta tillgängliga storlekar för virtuella datorinstanser
Om du vill se en lista med storlekar för virtuella datorinstanser som finns tillgängliga i en viss region, kan du använda kommandot [az vm list-sizes](/cli/azure/vm#az_vm_list_sizes).

```azurecli-interactive
az vm list-sizes --location eastus --output table
```

Utdata liknar följande komprimerade exempel som visar resurserna som tilldelats varje virtuell datorstorlek:

```azurecli-interactive
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
[...]
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
[...]
                 4          2048  Standard_F1                           1           1047552                   16384
                 8          4096  Standard_F2                           2           1047552                   32768
[...]
                24         57344  Standard_NV6                          6           1047552                   38912
                48        114688  Standard_NV12                        12           1047552                  696320
```

### <a name="create-a-scale-set-with-a-specific-vm-instance-size"></a>Skapa en skalningsuppsättning med en virtuell datorinstans av en viss storlek
När du skapade en skalningsuppsättning i början av självstudien, angavs en standard virtuell dator-SKU på *Standard_D1_v2* för de virtuella datorinstanserna. Du kan ange en annan virtuell datorinstans-storlek baserat på utdata från [az vm list-sizes](/cli/azure/vm#az_vm_list_sizes). Följande exempel skapar en skalningsuppsättning med parametern `--vm-sku` för att ange en virtuell datorinstans-storlek på *Standard_F1*. Eftersom det tar några minuter att skapa och konfigurera alla skalningsuppsättningsresurser och virtuella datorinstanser så behöver du inte distribuera följande skalningsuppsättning:

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSetF1Sku \
  --image UbuntuLTS \
  --vm-sku Standard_F1 \
  --admin-user azureuser \
  --generate-ssh-keys
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ändra kapaciteten för en skalningsuppsättning
När du skapade en skalningsuppsättning i början av självstudien, distribuerades två virtuella datorinstanser som standard. Du kan ange parametern `--instance-count` med [az vmss create](/cli/azure/vmss#az_vmss_create) för att ändra antalet instanser som skapas med en skalningsuppsättning. Om du vill öka eller minska antalet virtuella datorinstanser i din befintliga skalningsuppsättning, kan du manuellt ändra kapaciteten. Skalningsuppsättningen skapar eller tar bort antalet virtuella datorinstanser som krävs och konfigurerar sedan lastbalanseraren att distribuera trafiken.

Om du vill öka eller minska antalet virtuella datorinstanser manuellt i skalningsuppsättningen, använder du [az vmss scale](/cli/azure/vmss#az_vmss_scale). Följande exempel anger antalet virtuella datorinstanser i din skalningsuppsättning till *3*:

```azurecli-interactive
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 3
```

Det tar några minuter att uppdatera kapaciteten för din skalningsuppsättning. Om du vill se antalet instanser som du har i skalningsuppsättningen för tillfället, använder du [az vmss show](/cli/azure/vmss#az_vmss_show) och frågar på *sku.capacity*:

```azurecli-interactive
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```


## <a name="common-management-tasks"></a>Vanliga administrativa uppgifter
Du kan nu skapa en skalningsuppsättning, lista anslutningsinformationen och ansluta till virtuella datorinstanser. Du har lärt dig hur du kan använda en annan OS-avbildning för dina virtuella datorinstanser, välja en annan virtuell datorstorlek eller manuellt skala antalet instanser manuellt. Som en del av den dagliga hanteringen, kan du behöva stoppa, starta eller starta om de virtuella datorinstanserna i en skalningsuppsättning.

### <a name="stop-and-deallocate-vm-instances-in-a-scale-set"></a>Stoppa och frigöra virtuella datorinstanser i en skalningsuppsättning
Om du vill stoppa en eller flera virtuella datorinstanser, använder du [az vmss stop](/cli/azure/vmss#az_vmss_stop). Parametern `--instance-ids` låter dig ange en eller flera virtuella datorinstanser att stoppa. Om du inte anger ett instans-ID, stoppas alla virtuella datorinstanser i skalningsuppsättningen. Följande exempel stoppar instansen *1*:

```azurecli-interactive
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

Stoppade virtuella datorinstanser är fortfarande allokerade och fortsätter att kosta beräkningsavgifter. Om du istället vill att de virtuella datorinstanserna ska frigöras och endast kosta lagringsavgifter, använder du [az vmss deallocate](/cli/azure/vmss#az_vmss_deallocate). Följande exempel stoppar och frigör instansen *1*:

```azurecli-interactive
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

### <a name="start-vm-instances-in-a-scale-set"></a>Starta virtuella datorinstanser i en skalningsuppsättning
Om du vill starta en eller flera virtuella datorinstanser, använder du [az vmss start](/cli/azure/vmss#az_vmss_start). Parametern `--instance-ids` låter dig ange en eller flera virtuella datorinstanser att starta. Om du inte anger ett instans-ID, startas alla virtuella datorinstanser i skalningsuppsättningen. Följande exempel startar instansen *1*:

```azurecli-interactive
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```

### <a name="restart-vm-instances-in-a-scale-set"></a>Starta om virtuella datorinstanser i en skalningsuppsättning
Om du vill starta om en eller flera virtuella datorinstanser, använder du [az vmss restart](/cli/azure/vmss#az_vm_restart). Parametern `--instance-ids` låter dig ange en eller flera virtuella datorinstanser att starta om. Om du inte anger ett instans-ID, startas alla virtuella datorinstanser i skalningsuppsättningen om. Följande exempel startar om instansen *1*:

```azurecli-interactive
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 1
```


## <a name="clean-up-resources"></a>Rensa resurser
Om en resursgrupp tas bort, tas även alla resurser som ingår i gruppen bort, som de virtuella datorinstanserna, det virtuella nätverket och diskarna tas också bort. Parametern `--no-wait` återför kontrollen till kommandotolken utan att vänta på att uppgiften slutförs. Parametern `--yes` bekräftar att du vill ta bort resurserna utan att tillfrågas ytterligare en gång.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Nästa steg
I den här självstudien beskrivs hur du utför vissa grundläggande skapande och hanteringsåtgärder för skalningsuppsättningar med Azure CLI:

> [!div class="checklist"]
> * Skapa och anslut till en VM-skalningsuppsättning
> * Välja och använda VM-avbildningar
> * Visa och använda specifika VM-storlekar
> * Skala en skalningsuppsättning automatiskt
> * Utför vanliga hanteringsåtgärder för skalningsuppsättningar

Gå vidare till nästa självstudie om du vill lära dig mer om diskar i skalningsuppsättningar.

> [!div class="nextstepaction"]
> [Använda datadiskar med skalningsuppsättningar](tutorial-use-disks-cli.md)

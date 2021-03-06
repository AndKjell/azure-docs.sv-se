---
title: Spara en avbildning av en Linux VM | Microsoft Docs
description: Lär dig mer om att hämta en avbildning av en Linux-baserade Azure-dator (VM) skapas med den klassiska distributionsmodellen.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: ae87af45ffc442c0de6c7f703694a994e536cdb8
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929220"
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Spara en klassisk virtuell Linux-dator som en avbildning
> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln beskriver den klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen. Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Den här artikeln visar hur du spara en klassisk Azure-dator (VM) som kör Linux som en bild för att skapa andra virtuella datorer. Den här avbildningen innehåller operativsystemdisken och datadiskar som är kopplade till den virtuella datorn. De omfattar inte konfiguration av nätverk, så du måste konfigurera som när du skapar den andra virtuella datorn från avbildningen.

Azure lagrar avbildningen under **avbildningar**, tillsammans med alla avbildningar som du har överfört. Mer information om avbildningar finns i [About VM Images in Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Innan du börjar
Dessa instruktioner förutsätter att du redan har skapat en Azure-dator med hjälp av den klassiska distributionsmodellen och konfigurerat operativsystem, inklusive koppla eventuella datadiskar. Om du vill skapa en virtuell dator kan du läsa [så här skapar du en Linux-dator][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>Avbilda den virtuella datorn
1. [Ansluta till den virtuella datorn](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med hjälp av en SSH-klient för ditt val.
2. Skriv följande kommando i SSH-fönstret. Utdata från `waagent` kan variera något beroende på vilken version av det här verktyget:

    ```bash
    sudo waagent -deprovision+user
    ```

    Föregående kommando försöker Rensa systemet och gör den lämplig för reprovisioning. Den här åtgärden utför följande uppgifter:

   * Tar bort SSH-värdnycklar (om Provisioning.RegenerateSshHostKeyPair är ”y” i konfigurationsfilen)
   * Tar bort namnserverkonfigurationen i /etc/resolv.conf
   * Tar bort den `root` användarlösenord från/etc/shadow (om Provisioning.DeleteRootPassword är ”y” i konfigurationsfilen)
   * Tar bort cachelagrade lån för DHCP-klienter
   * Återställer värdnamnet till localhost.localdomain
   * Tar bort sist etablerats användarkonto (hämtades från /var/lib/waagent) **och tillhörande data**.

     > [!NOTE]
     > Avetablering tar bort filer och data till ”generalisera” avbildningen. Endast köra kommandot på en virtuell dator som du vill spara som en ny bild-mall. Det garanterar inte att avbildningen är avmarkerad av all känslig information eller lämpar sig för Vidaredistribution till tredje part.

3. Typ **y** att fortsätta. Du kan lägga till den `-force` parametern för att undvika det här steget för bekräftelse.
4. Typ **avsluta** att Stäng SSH-klienten.

   > [!NOTE]
   > De återstående steg förutsätts att du redan har [installerat Azure CLI](../../../cli-install-nodejs.md) på klientdatorn. Alla följande steg kan också göras den [Azure-portalen](http://portal.azure.com).

5. Öppna Azure CLI och logga in på Azure-prenumerationen från klientdatorn. Mer information finns [Anslut till en Azure-prenumeration från Azure CLI](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > Logga in på portalen i Azure-portalen.

6. Kontrollera att du är i Service Management-läge:

    ```azurecli
    azure config mode asm
    ```

7. Stäng av den virtuella dator som redan avetableras. I följande exempel stänger av den virtuella datorn med namnet `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Om det behövs kan du visa en lista alla de virtuella datorerna skapas i din prenumeration med hjälp av `azure vm list`

   > [!NOTE]
   > Om du använder Azure portal, Välj den virtuella datorn och klickar på **stoppa** att stänga av den virtuella datorn.

8. Fånga avbildningen när den virtuella datorn har stoppats. I följande exempel samlar in den virtuella datorn med namnet `myVM` och skapar en generaliserad avbildning med namnet `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    Den `-t` underkommandot tar bort den ursprungliga virtuella datorn.

    > [!NOTE]
    > I Azure-portalen kan du göra en avbildning genom att välja **bild** från navmenyn. Du måste ange följande information för avbildningen: namn, resursgrupp, plats, typ av operativsystem och sökväg till blob storage.

9. Den nya avbildningen är nu tillgänglig i listan över avbildningar som kan användas för att konfigurera nya virtuella datorer. Du kan visa den med kommandot:

   ```azurecli
   azure vm image list
   ```

   På den [Azure-portalen](http://portal.azure.com), den nya avbildningen visas i den **avbildningar av Virtuella datorer (klassisk)** som tillhör den **Compute** tjänster. Du kan komma åt **avbildningar av Virtuella datorer (klassisk)** genom att klicka på **alla tjänster** tjänsten listan överst i Azure och sedan söka den **Compute** tjänster.   

   ![Avbildningen lyckades](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Nästa steg
Bilden är redo att användas för att skapa virtuella datorer. Du kan använda Azure CLI-kommando `azure vm create` och ange avbildningens namn som du skapade. Mer information finns i [med Azure CLI med klassisk distributionsmodell](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Du kan också använda den [Azure-portalen](http://portal.azure.com) att skapa en anpassad virtuell dator med hjälp av den **bild** metoden och välja den bild som du skapade. Mer information finns i [så här skapar du en anpassad virtuell dator][How to Create a Custom Virtual Machine].

**Se även:** [användarhandboken för Azure Linux-Agent](../../extensions/agent-linux.md)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md

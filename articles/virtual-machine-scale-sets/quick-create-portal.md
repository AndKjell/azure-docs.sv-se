---
title: Snabbstart – Skapa en VM-skalningsuppsättning i Azure Portal | Microsoft Docs
description: Lär dig hur du snabbt skapar en VM-skalningsuppsättning i Azure Portal
keywords: VM-skalningsuppsättningar
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: cynthn
ms.openlocfilehash: fb3a3e1cec0d6ec15495e677e7bead1c02445803
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38680966"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Snabbstart: Skapa en VM-skalningsuppsättning med Azure Portal
Med en VM-skalningsuppsättning kan du distribuera och hantera en uppsättning identiska, virtuella datorer med automatisk skalning. Du kan skala antalet virtuella datorer i skalningsuppsättningen manuellt eller definiera regler för automatisk skalning baserat på resursanvändning som CPU, minnesefterfrågan eller nätverkstrafik. En Azure-belastningsutjämnare distribuerar sedan trafiken till de virtuella datorinstanserna i skalningsuppsättningen. I den här snabbstarten skapar du en VM-skalningsuppsättning i Azure Portal.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.


## <a name="log-in-to-azure"></a>Logga in på Azure
Logga in på Azure Portal på http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Skapa VM-skalningsuppsättningar
Du kan distribuera en skalningsuppsättning som anges med en Windows Server-avbildning eller en Linux-avbildningen som RHEL, CentOS, Ubuntu eller SLES.

1. Klicka på **Skapa en resurs** längst upp till vänster i Azure Portal.
2. Sök efter *skalningsuppsättning*, välj först **VM-skalningsuppsättning**och sedan **Skapa**.
3. Ange ett namn för skalningsuppsättningen, t.ex. *myScaleSet*.
4. Välj önskad typ av operativsystem, t.ex. *Windows Server 2016 Datacenter*.
5. Ange önskat resursgruppsnamn, t.ex *myResourceGroup*, och plats, t.ex *USA, östra*.
6. Ange önskat användarnamn och välj den autentiseringstyp du föredrar.
    - Ett **lösenord** måste innehålla minst 12 tecken och måste uppfylla tre av följande fyra komplexitetskrav: en gemen, en versal, en siffra och ett specialtecken. Mer information finns i [kraven om användarnamn och lösenord](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Om du väljer en Linux OS-diskavbildning kan du istället välja **offentlig SSH-nyckel**. Du behöver bara ange din offentliga nyckel, t.ex. *~/.ssh/id_rsa.pub*. Du kan använda Azure Cloud Shell från portalen för att [skapa och använda SSH-nycklar](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Ange ett **offentligt IP-adressnamn**, t.ex. *myPublicIP*.
8. Ange en unik **domännamnsetikett**, t.ex. *myuniquedns*. Det här DNS-etikettformuläret utgör grunden för belastningsutjämnarens FQDN framför skalningsuppsättningen.
9. Bekräfta skalningsuppsättningsalternativen genom att välja **Skapa**.

    ![Skapa en VM-skalningsuppsättning i Azure Portal](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Anslut till en virtuell dator i skalningsuppsättningen
När du skapar en skalningsuppsättning i portalen skapas en belastningsutjämnare. Regler för Network Address Translation (NAT) används för att distribuera trafiken till skalningsuppsättningsinstanserna för fjärranslutningar, t.ex. RDP och SSH.

Om du vill visa dessa NAT-regler och anslutningsinformationen för dina skalningsuppsättningsinstanser gör du så här:

1. Välj den resursgrupp som du skapade i föregående steg, t.ex. *myResourceGroup*.
2. Välj **belastningsutjämnare** i listan över resurser, t.ex. *myScaleSetLab*.
3. Välj **Inkommande NAT-regler** på menyn till vänster i fönstret.

    ![Med inkommande NAT-regler kan du ansluta till instanser av VM-skalningsuppsättningar](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Du kan ansluta till varje virtuell dator i skalningsuppsättningen med dessa NAT-regler. Varje VM-instans listar en IP-måladress och ett TCP-portvärde. Om IP-måladressen t.ex. är *104.42.1.19* och TCP-porten är *50001* ansluter du till VM-instansen på följande sätt:

- För en Windows-skalningsuppsättning ansluter du till VM-instansen med RDP på `104.42.1.19:50001`
- För en Windows-skalningsuppsättning ansluter du till VM-instansen med RDP på `ssh azureuser@104.42.1.19 -p 50001`

Ange, när du uppmanas till detta, de autentiseringsuppgifter du angav i föregående steg när du skapade skalningsuppsättningen. Skalningsuppsättningsinstanserna är vanliga virtuella datorer som du kan interagera med som vanligt. Mer information om hur du distribuerar och kör program på dina skalningsuppsättningsinstanser finns i [Distribuera dina program på VM-skalningsuppsättningar](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Rensa resurser
Ta bort resursgruppen, skalningsuppsättningen och alla relaterade resurser när de inte längre behövs. Det gör du genom att välja resursgruppen för den virtuella datorn och klicka på **Ta bort**.


## <a name="next-steps"></a>Nästa steg
I den här snabbstarten har du skapat en grundläggande skalningsuppsättning i Azure Portal. Om du vill veta mer kan du gå till självstudiekursen om hur man skapar och hanterar VM-skalningsuppsättningar för Azure.

> [!div class="nextstepaction"]
> [Skapa och hantera VM-skalningsuppsättningar för Azure](tutorial-create-and-manage-powershell.md)
---
title: Tillgängliggöra Virtual Machine Scale Sets i Azure Stack | Microsoft Docs
description: Lär dig hur en molnoperator kan lägga till VM-Skalningsuppsättningar i Azure Stack Marketplace
services: azure-stack
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: sethm
ms.reviewer: kivenkat
ms.openlocfilehash: 37122f11990d292e250c0a0bc42c0527731f599a
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076430"
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Tillgängliggöra Virtual Machine Scale Sets i Azure Stack

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*
  
VM-skalningsuppsättningar är en beräkningsresurs för Azure Stack. Du kan använda dem för att distribuera och hantera en uppsättning identiska virtuella datorer. För alla virtuella datorer konfigureras på samma sätt, skalningsuppsättningar som inte kräver företablering av virtuella datorer. Det är enklare att skapa storskaliga tjänster som riktar sig mot big compute, stordata och arbetsbelastningar i behållare.

Den här artikeln vägleder dig genom processen att tillgängliggöra skalningsuppsättningar i Azure Stack Marketplace. När du har slutfört den här proceduren kan användarna lägga till VM-skalningsuppsättningar till sina prenumerationer.

VM-skalningsuppsättningar i Azure Stack är som VM-skalningsuppsättningar i Azure. Mer information finns i följande videoklipp:
* [Mark Russinovich berättar om Azure-skalningsuppsättningar](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Skaluppsättningar för virtuell dator med Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

På Azure Stack stöder skalningsuppsättningar för virtuella datorer inte automatisk skalning. Du kan lägga till fler instanser till en skalningsuppsättning med Resource Manager-mallar, CLI eller PowerShell.

## <a name="prerequisites"></a>Förutsättningar

- **På Marketplace**  
    Registrera Azure Stack med globala Azure för att möjliggöra tillgängligheten för objekt i Marketplace. Följ instruktionerna i [registrera Azure Stack med Azure](azure-stack-registration.md).
- **Avbildning av operativsystemet**  
  Innan du kan skapa en skalningsuppsättning för virtuella datorer (VMSS), måste du hämta VM-avbildningar för användning i VMSS från den [Azure Stack Marketplace](azure-stack-download-azure-marketplace-item.md). Bilderna måste redan finnas innan en användare kan skapa en ny VMSS. 


## <a name="use-the-azure-stack-portal"></a>Använd Azure Stack-portalen 

>[!NOTE]  
> Informationen i det här avsnittet gäller när du använder Azure Stack-version 1808 eller senare. Om du har version 1807 eller tidigare, se [lägga till den virtuella datorns Skalningsuppsättning (före 1808)](#add-the-virtual-machine-scale-set-(prior-to-version-1808)).

1. Logga in på Azure Stack-portalen. Gå sedan till **alla tjänster** > **VM-skalningsuppsättningar**, och sedan under *COMPUTE*väljer **förskalningsuppsättningarförvirtuelladatorer**. 
   ![Välj VM-skalningsuppsättningar](media/azure-stack-compute-add-scalesets/all-services.png)

2. Välj Skapa ***VM-skalningsuppsättningar***.
   ![Skapa en VM-skalningsuppsättning](media/azure-stack-compute-add-scalesets/create-scale-set.png)

3. Fyll i de tomma fälten, Välj från listrutorna för *avbildning av operativsystemdisken*, *prenumeration*, och *Instansstorlek*. Välj **Ja** för *Använd hanterade diskar*. Välj sedan **Create** (Skapa).
    ![Konfigurera och skapa](media/azure-stack-compute-add-scalesets/create.png)

4. Se din nya VM-skalningsuppsättningar ställer, gå till **alla resurser**, söka för namn på VM-skalningsuppsättningen och välj sedan dess namn i sökningen. 
   ![Visa skalningsuppsättningen](media/azure-stack-compute-add-scalesets/search.png)



## <a name="add-the-virtual-machine-scale-set-prior-to-version-1808"></a>Lägg till den virtuella datorns Skalningsuppsättning (före version 1808)
>[!NOTE]  
> Informationen i det här avsnittet gäller när du använder en version av Azure Stack före 1808. Om du använder version 1808 eller senare, se [använder Azure Stack portal](#use-the-azure-stack-portal).

1. Öppna Azure Stack Marketplace och Anslut till Azure. Välj **Marketplace management**> **+ Lägg till från Azure**.

    ![Marketplace-hantering](media/azure-stack-compute-add-scalesets/image01.png)

2. Lägg till och hämta Virtual Machine Scale Sets marketplace-objekt.

    ![Skalningsuppsättning för virtuell dator](media/azure-stack-compute-add-scalesets/image02.png)

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Uppdatera avbildningar i Virtual Machine Scale Sets

När du skapar en VM-skalningsuppsättning kan kan användare uppdatera avbildningar i skalningsuppsättningen utan skalningsuppsättningen behöva återskapas. Processen för att uppdatera en bild är beroende av följande scenarier:

1. Distributionsmall för VM-skalningsuppsättningen **anger senaste** för *version*:  

   När den *version* har angetts som **senaste** i den *imageReference* avsnitt i mallen för en skalbar, skala upp åtgärder på scale set användningen av den senaste tillgängliga versionen Anger instanser av avbildningen för skalan. När en uppskalning är klar kan du ta bort äldre VM scale sets-instanser.  (Värdena för *publisher*, *erbjuder*, och *sku* förblir oförändrade). 

   I följande JSON-exempel anges *senaste*:  

    ```Json  
    "imageReference": {
        "publisher": "[parameters('osImagePublisher')]",
        "offer": "[parameters('osImageOffer')]",
        "sku": "[parameters('osImageSku')]",
        "version": "latest"
        }
    ```

   Innan du skala upp kan använda en ny avbildning, måste du hämta den nya avbildningen:  

   - När avbildningen på Marketplace är en senare version än avbildningen i skalningsuppsättningen: ladda ned den nya avbildningen som ersätter äldre avbildningen. När avbildningen har ersatts kan kan en användare fortsätta att skala upp. 

   - När versionsnumret för avbildningen på Marketplace är samma som på bilden i skalningsuppsättningen: ta bort den avbildningen som används i skalningsuppsättningen och ladda ned den nya avbildningen. Under tiden mellan borttagning av den ursprungliga bilden och hämtning av den nya avbildningen kan du skala upp. 
      
     Den här processen måste anges till resyndicate avbildningar som gör användning av sparse-fil-format, som introducerades i version 1803. 
 

2. Distributionsmall för VM-skalningsuppsättningen **anger inte senaste** för *version* och anger ett versionsnummer i stället:  

    Om du har hämtat en avbildning med en nyare version (som ändrar den tillgängliga versionen), det går inte att skalningsuppsättningen skala upp. Detta är avsiktligt eftersom versionsnumret för avbildningen som angetts i mallen för skalningsuppsättningen måste vara tillgängliga.  

Mer information finns i [operativsystemsdiskar och avbildningar](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="scale-a-virtual-machine-scale-set"></a>Skala en VM-skalningsuppsättning
Du kan skala storleken på en *virtual machine scale Sets* så att de blir större eller mindre.  

1. Välj din skalningsuppsättning i portalen och välj sedan **skalning**.
2. Använd på skjutreglaget för att ange den nya nivån av skalning för den här skalningsuppsättningen för virtuell dator och välj sedan **spara**.
     ![Skala uppsättningen](media/azure-stack-compute-add-scalesets/scale.png)





## <a name="remove-a-virtual-machine-scale-set"></a>Ta bort en VM-Skalningsuppsättning

Skala set galleri-objekt för att ta bort en virtuell dator, kör du följande PowerShell-kommando:

```PowerShell  
    Remove-AzsGalleryItem
````

> [!NOTE]
> Galleri-objekt kan inte tas bort omedelbart. Natt måste du uppdatera portalen flera gånger innan objektet visas som tas bort från Marketplace.

## <a name="next-steps"></a>Nästa steg
[Vanliga frågor om Azure Stack](azure-stack-faq.md)
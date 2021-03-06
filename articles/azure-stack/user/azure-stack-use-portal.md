---
title: Med hjälp av Azure Stack-portalen | Microsoft Docs
description: Lär dig hur du använder användarportalen i Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: mabrigg
ms.reviewer: efemmano
ms.openlocfilehash: 7ccf746cfd9645b63c2810bff83cfb0c4fa55d65
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713755"
---
# <a name="use-the-azure-stack-portal"></a>Använd Azure Stack-portalen

*Gäller för: integrerade Azure Stack-system och Azure Stack Development Kit*

Du kan använda Azure Stack-portalen för att prenumerera offentliga och använda de tjänster som dessa erbjudanden tillhandahåller. Om du har använt den globala Azure-portalen kan är du redan bekant med hur webbplatsen fungerar.

## <a name="access-the-portal"></a>Få åtkomst till portalen

Azure Stack-operatör (en tjänstleverantör eller en administratör i din organisation), meddelar rätt URL för åtkomst till portalen.

- För ett integrerat system URL: en varierar beroende på region och externa domännamn för din operatör och ska vara i formatet https://portal.&lt; *region*&gt;.&lt; *FQDN*&gt;.
- Om du använder Azure Stack Development Kit är Portaladress https://portal.local.azurestack.external.

![Skärmdump av användarportalen för Azure Stack](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Anpassa instrumentpanelen

Instrumentpanelen innehåller en standarduppsättning med paneler. Du kan välja **redigera instrumentpanel** att ändra standardinstrumentpanelen eller välj **ny instrumentpanel** att skapa en anpassad instrumentpanel. Du kan enkelt anpassa en instrumentpanel genom att lägga till eller ta bort paneler. Om du vill lägga till en panel för beräkning, väljer du exempelvis **+ skapa en resurs**. Högerklicka på **Compute**, och välj sedan **fäst på instrumentpanelen**.

## <a name="create-subscription-and-browse-available-resources"></a>Skapa prenumeration och bläddra bland tillgängliga resurser

Om du inte redan har en prenumeration, är det första du behöver göra prenumerera på ett erbjudande. Sedan kan bläddra du de tillgängliga resurserna. Bläddra och skapa resurser genom att använda någon av följande metoder:

- Välj den **Marketplace** panelen på instrumentpanelen.
- På den **alla resurser** panelen, väljer **skapa resurser**.
- I det vänstra navigeringsfönstret väljer **+ skapa en resurs**.

## <a name="learn-how-to-use-available-services"></a>Lär dig hur du använder tillgängliga tjänster

Om du behöver vägledning för hur du använder tillgängliga tjänster kan finnas det olika alternativ tillgängliga för dig.

- Din organisation eller en tjänstleverantör kan ge sin egen dokumentation, som normalt är fallet om de erbjuder anpassade tjänster eller appar.
- Appar från tredje part har sin egen dokumentation.
- Vi rekommenderar att du först granska Azure Stack-dokumentation för Azure-konsekventa tjänster. Välj ikonen Hjälp för att komma åt Azure Stack-användardokumentation, och välj sedan **hjälp + support**.

    ![Hjälp och support alternativet i Användargränssnittet](media/azure-stack-use-portal/HelpAndSupport.png)

    I synnerhet föreslår vi att du läser följande artiklar för att komma igång:

    - [Viktiga överväganden: med hjälp av tjänster eller att skapa appar för Azure Stack](azure-stack-considerations.md)
    - I den **anlita** avsnittet i dokumentationen, det finns en artikel för överväganden för varje tjänst. Att tänka på sidan beskriver skillnaderna mellan tjänsten erbjuds i Azure och samma tjänst som erbjuds i Azure Stack. Ett exempel finns i [överväganden för virtuella datorer](azure-stack-vm-considerations.md). Det kan finnas andra informationen i den **anlita** avsnitt som är unik för Azure Stack.

      Du kan använda Azure-dokumentationen som allmän referens för en tjänst, men du måste känna till dessa skillnader. Förstå att dokumentationen länkar på den **snabbstartsguider** panelen pekar på Azure-dokumentationen.

## <a name="get-support"></a>Få support

Om du behöver support kontaktar du din organisation eller service provider för mer information.

Om du använder Azure Stack Development Kit på [Azure Stack-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) är den enda källan för support.

## <a name="next-steps"></a>Nästa steg

[Viktiga överväganden: med hjälp av tjänster eller att skapa appar för Azure Stack](azure-stack-considerations.md)

---
title: Lägga till eller ta bort en grupp från någon annan grupp i Azure Active Directory | Microsoft Docs
description: Lär dig mer om att lägga till eller ta bort en grupp från någon annan grupp med Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: c28fe5ef226fac993fde221b16bfa875ba4845ca
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579776"
---
# <a name="how-to-add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>Så här: lägga till eller ta bort en grupp från någon annan grupp med Azure Active Directory
Den här artikeln hjälper dig att lägga till och ta bort en grupp från någon annan grupp med Azure Active Directory.

>[!Note]
>Om du försöker ta bort den överordnade gruppen kan se [uppdatera eller ta bort en grupp och dess medlemmar](active-directory-groups-delete-group.md).

## <a name="add-a-group-as-a-member-to-another-group"></a>Lägga till en grupp som en medlem till en annan grupp
Du kan lägga till en befintlig grupp till en annan befintlig grupp, skapar en medlem i gruppen (Undergrupp) och en överordnad grupp. Medlem-grupp ärver de attribut och egenskaperna för den överordnade gruppen, sparar du konfigurationen.

### <a name="to-add-a-group-as-a-member-to-another-group"></a>Lägg till en grupp som en medlem till en annan grupp

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som Global administratör för katalogen.

2. Välj **Azure Active Directory**, och välj sedan **grupper**.

3. På den **grupper – alla grupper** , söka efter och välj den grupp som är för att bli medlem i en annan grupp. I den här övningen ska du använda den **MDM princip – västra** grupp.

    >[!Note]
    >Du kan lägga till din grupp som en medlem till endast en grupp i taget. Dessutom kan den **Välj grupp** box filtrerar visningen baserat på matchning av ditt bidrag till någon del av ett namn för användaren eller enheten. Dock stöds jokertecken inte.

    ![Grupper – alla gruppsidan med MDM-principen – västra grupp vald](media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. På den **gruppmedlemskap för MDM - Väst - princip** väljer **gruppmedlemskap**väljer **Lägg till**, leta upp den grupp du vill att din grupp att vara medlem i och välj sedan  **Välj**. I den här övningen ska du använda den **MDM princip – alla org** grupp.

    Den **MDM princip – västra** är nu medlem av den **MDM princip – alla org** grupp, ärver alla egenskaper och konfiguration av MDM-principen - gruppen med alla org.

    ![Skapa en gruppmedlemskap genom att lägga till gruppen till en annan grupp](media/active-directory-groups-membership-azure-portal/add-group-membership.png)

5. Granska den **gruppmedlemskap för MDM - Väst - princip** och se relationen mellan gruppen och medlemmen.

    ![MDM - Väst - gruppmedlemskap sidan som visar den överordnade gruppen](media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. För en mer detaljerad vy av relationen mellan gruppen och medlemmen, väljer du gruppnamnet (**MDM princip – alla org**) och ta en titt på de **MDM princip – västra** sidan information.

    ![Sidan med medlemskap som visar både medlemmen och gruppinformation](media/active-directory-groups-membership-azure-portal/group-membership-review.png)

## <a name="remove-a-member-group-from-another-group"></a>Ta bort en grupp från någon annan grupp
Du kan ta bort en befintlig grupp från någon annan grupp. Men ta bort medlemskapet tar också bort alla ärvda attribut och egenskaper för dina användare.

### <a name="to-remove-a-member-group-from-another-group"></a>Att ta bort en medlem från en annan grupp
1. På den **grupper – alla grupper** , söka efter och välj den grupp som ska tas bort som medlem i någon annan grupp. I den här övningen använder vi igen den **MDM princip – västra** grupp.

2. På den **MDM princip – västra översikt** väljer **gruppmedlemskap**.

    ![Princip för MDM – västra översiktssidan](media/active-directory-groups-membership-azure-portal/group-membership-overview.png)

3. Välj den **MDM princip – alla org** gruppen från den **gruppmedlemskap för MDM - Väst - princip** och välj sedan **ta bort** från den **MDM princip – västra** sidan information.

    ![Sidan med medlemskap som visar både medlemmen och gruppinformation](media/active-directory-groups-membership-azure-portal/group-membership-remove.png)


## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

- [Visa dina grupper och medlemmar](active-directory-groups-view-azure-portal.md)

- [Skapa en basgrupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)

- [Lägg till eller ta bort medlemmar från en grupp](active-directory-groups-members-azure-portal.md)

- [Redigera din gruppinställningar](active-directory-groups-settings-azure-portal.md)

- [Tilldela licenser till användare gruppvis](../users-groups-roles/licensing-groups-assign.md)

---
title: Återställa eller permanent ta bort en nyligen borttagna användare i Azure Active Directory | Microsoft Docs
description: Lär dig mer om att visa återställningsbara användare, återställa en borttagen användare eller permanent ta bort en användare med Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 88d3c672cd072cd4b252f7ce4ede3a4c7b13a7db
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736180"
---
# <a name="how-to-restore-or-permanently-remove-a-recently-deleted-user-with-azure-active-directory"></a>Så här: Återställ eller permanent ta bort en nyligen borttagna användare med Azure Active Directory
När du tar bort en användare kan fortfarande kontot i ett väntetillstånd i 30 dagar. Under den 30-dagars fönstret kan användarkontot återställas, tillsammans med alla dess egenskaper. När 30-dagars fönstret skickar bort användaren automatiskt och permanent.

Du kan visa dina återställningsbara användare, Återställ en borttagen användare eller permanent ta bort en användare med Azure Active Directory (AD Azure) i Azure-portalen.

>[!Important]
>Varken du eller Microsofts kundsupport kan återställa en användare som har tagits bort permanent.

## <a name="required-permissions"></a>Nödvändiga behörigheter
Du måste ha något av följande roller för att återställa och ta bort användarna permanent.

- Företagsadministratör

- Partnersupport, nivå 1

- Partnersupport, nivå 2

- Användarkonto-administratör

## <a name="view-your-restorable-users"></a>Visa dina återställningsbara användare
Du kan se alla användare som har tagits bort mindre än 30 dagar sedan. Dessa användare kan återställas.

### <a name="to-view-your-restorable-users"></a>Visa dina återställningsbara användare
1. Logga in på den [Azure-portalen](https://portal.azure.com/) med ett konto som Global administratör för katalogen.

2. Välj **Azure Active Directory**väljer **användare**, och välj sedan **borttagna användare**.

    Granska listan över användare som är tillgängliga för återställning.

    ![Användare – sidan borttagen användare med användare som fortfarande kan återställas](media/active-directory-users-restore/users-deleted-users-view-restorable.png)

## <a name="restore-a-recently-deleted-user"></a>Återställa en nyligen borttagna användare
Alla relaterade kataloginformation bevaras även om en användares konto har inaktiverats. När du återställer en användare, återställs även den här kataloginformation.

### <a name="to-restore-a-user"></a>Att återställa en användare
1. På den **användare – borttagna användare** , söka efter och välj en av de tillgängliga användarna. Till exempel _Mary Parker_.

2. Välj **återställning användaren**.

    ![Användare – sidan borttagna användare med återställning användare alternativet är markerat](media/active-directory-users-restore/users-deleted-users-restore-user.png)

## <a name="permanently-delete-a-user"></a>Ta bort en användare permanent
Du kan permanent ta bort en användare från din katalog utan att vänta i 30 dagar för automatisk borttagning. En permanent borttagen användare kan inte återställas av dig, en annan administratör eller av Microsoft kundsupport.

>[!Note]
>Om du permanent ta bort en användare av misstag, måste du skapa en ny användare och manuellt ange all information som tidigare. Mer information om hur du skapar en ny användare finns i [Lägg till eller ta bort användare](add-users-azure-active-directory.md).

### <a name="to-permanently-delete-a-user"></a>Permanent ta bort en användare

1. På den **användare – borttagna användare** , söka efter och välj en av de tillgängliga användarna. Till exempel _Rae Huff_.

2. Välj **ta bort permanent**.

    ![Användare – sidan borttagna användare med återställning användare alternativet är markerat](media/active-directory-users-restore/users-deleted-users-permanent-delete-user.png)

## <a name="next-steps"></a>Nästa steg
När du har återställt eller ta bort användarna, kan du utföra följande basic-processer:

- [Lägga till eller ta bort användare](add-users-azure-active-directory.md)

- [Tilldela roller till användare](active-directory-users-assign-role-azure-portal.md)

- [Lägga till eller ändra profilinformation](active-directory-users-profile-azure-portal.md)

- [Lägga till gästanvändare från annan katalog](../b2b/what-is-b2b.md) 

Mer information om andra tillgängliga hanteringsuppgifter, [Azure Active Directory management supportdokumentation](../users-groups-roles/index.yml).

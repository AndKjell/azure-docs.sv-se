---
title: Snabbstart för att få åtkomst till Azure Active Directory och för att skapa en ny klient | Microsoft Docs
description: Snabbstart med instruktioner om hur du hittar Azure Active Directory och hur du skapar en ny klient för din organisation.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: lizross
custom: it-pro
ms.openlocfilehash: 8ef68c8afcf61a1a11c341a679443071aece9812
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363012"
---
# <a name="quickstart-access-azure-active-directory-to-create-a-new-tenant"></a>Snabbstart: Komma åt Azure Active Directory för att skapa en ny klientorganisation
Du kan utföra alla administrativa aktiviteter med hjälp av Azure Active Directory (Azure AD)-portalen, inklusive att skapa en ny klient för din organisation. 

I den här snabbstarten får du lära dig hur du kommer till Azure-portalen och Azure Active Directory och du får lära dig hur du skapar en grundläggande klient för din organisation.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Nödvändiga komponenter
Innan du börjar måste du:

- Kontrollera att din organisation har en giltig Azure AD-licens.

- Kontrollera att du är en global administratör.

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure Portal
Logga in på din organisations [Azure-portal](https://portal.azure.com/) med ett konto som global administratör.

![Azure Portal-skärmen](media/active-directory-access-create-new-tenant/azure-ad-portal.png)

## <a name="create-a-new-tenant-for-your-organization"></a>Så här skapar du en ny klient för din organisation
När du har loggat in på Azure-portalen kan du skapa en ny klient för din organisation. Den nya klientorganisationen representerar din organisation och hjälper dig att hantera en specifik instans av Microsoft-molntjänster för dina interna och externa användare.

### <a name="to-create-a-new-tenant"></a>Så här skapar du en ny klient
1. Välj **Azure Active Directory**, **Skapa resurser**, **Identitet** och välj sedan **Azure Active Directory**.

    Sidan **Skapa katalog** visas.

    ![Sidan skapa Azure Active Directory](media/active-directory-access-create-new-tenant/azure-ad-create-new-tenant.png)

2.  På sidan **Skapa katalog** anger du följande information:
    
    - Skriv _Contoso_ i rutan **Organisationsnamn**.

    - Skriv _Contoso_ i rutan **Ursprungligt domännamn**.

    - Lämna alternativet _USA_ i rutan **Land eller region**.

3. Välj **Skapa**.

Den nya klientorganisationen skapas med domänen contoso.onmicrosoft.com.

## <a name="clean-up-resources"></a>Rensa resurser
Om du inte planerar att fortsätta använda det här programmet kan du ta bort klienten med följande steg:

- Välj **Azure Active Directory** och klicka sedan på sidan **Contoso – översikt** och **Ta bort katalogen**.

    Klienten och dess tillhörande information tas bort.

    ![Skapa katalogsida](media/active-directory-access-create-new-tenant/azure-ad-delete-new-tenant.png)

## <a name="next-steps"></a>Nästa steg
- Information om att ändra eller lägga till fler domännamn finns i [Lägga till ett anpassat domännamn i Azure Active Directory](add-custom-domain.md)

- Se [Lägga till eller ta bort en ny användare](add-users-azure-active-directory.md) för att lägga till användare

- Se [Skapa en basgrupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md) för att lägga till grupper och medlemmar

- Lär dig mer om [rollbaserad åtkomst med hjälp av privilegierad identitetshantering](../../role-based-access-control/pim-azure-resource.md) och [Villkorlig åtkomst](../../role-based-access-control/conditional-access-azure-management.md) för att hantera din organisations program och åtkomst.

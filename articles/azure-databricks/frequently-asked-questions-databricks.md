---
title: 'Azure Databricks: Vanliga frågor och hjälp | Microsoft Docs'
description: Få svar på vanliga frågor och felsökningsinformation om Azure Databricks.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: c3ba235c60480c38a21ee3264c54b4a4dcdea340
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434609"
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Vanliga frågor och svar om Azure Databricks

Den här artikeln visar de viktigaste frågorna som du kanske har relaterat till Azure Databricks. Här visas även några vanliga problem som du kan ha när du använder Databricks. Mer information finns i [vad är Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Kan jag använda min egen nycklar för kryptering av lokala? 
I den aktuella versionen stöds med hjälp av dina egna nycklar från Azure Key Vault inte. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Kan jag använda Azure-nätverk med Databricks?
Ett nytt virtuellt nätverk skapas som en del av Databricks etablering. Du kan inte använda din egen Azure-nätverk i den här versionen.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Hur kommer jag åt Azure Data Lake Store från en bärbar dator? 

Följ de här stegen:
1. Etablera ett huvudnamn för tjänsten i Azure Active Directory (AD Azure), och registrera nyckeln.
1. Tilldela behörighet till tjänstens huvudnamn i Data Lake Store.
1. Använd autentiseringsuppgifter för tjänstens huvudnamn i anteckningsboken för att komma åt en fil i Data Lake Store.

Mer information finns i [Använd Data Lake Store med Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html).

## <a name="fix-common-problems"></a>Åtgärda vanliga problem

Här är några problem som kan uppstå med Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problem: Den här prenumerationen har inte registrerats för användning av namnområdet 'Microsoft.Databricks'

#### <a name="error-message"></a>Felmeddelande

”Den här prenumerationen har inte registrerats för användning av namnområdet 'Microsoft.Databricks'. Se https://aka.ms/rps-not-found för hur du registrerar prenumerationer. (Code: MissingSubscriptionRegistration) ”

#### <a name="solution"></a>Lösning

1. Gå till [Azure-portalen](https://portal.azure.com).
1. Välj **prenumerationer**, den prenumeration som du använder, och sedan **resursprovidrar**. 
1. I listan över resursproviders, mot **Microsoft.Databricks**väljer **registrera**. Du måste ha rollen deltagare eller ägare på prenumerationen för att registrera resursprovidern.


### <a name="issue-your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problem: {Email} för ditt konto inte har rollen ägare eller deltagare på Databricks-arbetsytan i Azure portal

#### <a name="error-message"></a>Felmeddelande

”Ditt konto {email} inte har rollen som Prenumerationsägare eller deltagare på Databricks-arbetsytan i Azure-portalen. Det här felet kan också inträffa om du är en gästanvändare i klienten. Be administratören att ge dig åtkomst till eller lägga till dig som en användare direkt i Databricks-arbetsytan ”. 

#### <a name="solution"></a>Lösning

Här följer några lösningar på problemet:

* För att initiera klienten, måste du vara inloggad som en vanlig användare på klienten, inte som en gästanvändare. Du måste också ha en deltagarrollen på Databricks-arbetsytan. Du kan ge en användaråtkomst från den **åtkomstkontroll (IAM)** flik i Databricks-arbetsyta i Azure-portalen.

* Det här felet kan också inträffa om ditt domännamn för e-post har tilldelats flera kataloger i Azure AD. Undvik problemet genom att skapa en ny användare i katalogen som innehåller prenumerationen med din Databricks-arbetsyta.

    a. Gå till Azure AD i Azure-portalen. Välj **användare och grupper** > **lägga till en användare**.

    b. Lägga till en användare med en `@<tenant_name>.onmicrosoft.com` e-post i stället för `@<your_domain>` e-post. Du hittar det här alternativet i **anpassade domäner**, under Azure AD i Azure-portalen.
    
    c. Ge den nya användaren i **deltagare** rollen på Databricks-arbetsytan.
    
    d. Logga in på Azure-portalen med den nya användaren och hitta i Databricks-arbetsytan.
    
    e. Starta Databricks-arbetsyta som den här användaren.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problem: Ditt konto {email} har inte registrerats i Databricks 

#### <a name="solution"></a>Lösning

Om du inte har skapat arbetsytan, och du har lagts till som en användare kontaktar du den person som skapade arbetsytan. Har den personen lägga till dig med hjälp av Azure Databricks-administratörskonsolen. Anvisningar finns i [lägga till och hantera användare](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Om du har skapat arbetsytan och du felet fortfarande, försök med att markera **initiera arbetsyta** igen från Azure-portalen.

### <a name="issue-cloud-provider-launch-failure-while-setting-up-the-cluster-publicipcountlimitreached"></a>Problem: Molnet Providerfel start när du konfigurerar kluster (PublicIPCountLimitReached)

#### <a name="error-message"></a>Felmeddelande

”Starta Providerfel i molnet: ett moln provider-fel påträffades när du konfigurerar klustret. Mer information finns i guiden Databricks. Azure felkod: PublicIPCountLimitReached. Azure-felmeddelande: Det går inte att skapa mer än 60 offentliga IP-adresser för den här prenumerationen i den här regionen ”.

#### <a name="solution"></a>Lösning

Databricks-kluster använder en offentlig IP-adress per nod. Om din prenumeration redan har använt alla dess offentliga IP-adresser, bör du [begäran om att öka kvoten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Välj **kvot** som den **typ av problem**, och **nätverk: ARM** som den **Kvottypen**. I **information**, begära en kvot för offentlig IP-adress. Exempel: om din gräns för närvarande är 60 och du vill skapa ett kluster med 100 noder, få gränsen utökad till 160.

### <a name="issue-a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster-missingsubscriptionregistration"></a>Problem: En andra typen av molnet providern startfel när du konfigurerar kluster (MissingSubscriptionRegistration)

#### <a name="error-message"></a>Felmeddelande

”Starta Providerfel i molnet: ett moln provider-fel påträffades när du konfigurerar klustret. Mer information finns i guiden Databricks.
Azure felkod: MissingSubscriptionRegistration Azure-felmeddelande: prenumerationen har inte registrerats för användning av namnområdet 'Microsoft.Compute'. Se https://aka.ms/rps-not-found för hur du registrerar prenumerationer ”.

#### <a name="solution"></a>Lösning

1. Gå till [Azure-portalen](https://portal.azure.com).
1. Välj **prenumerationer**, den prenumeration som du använder, och sedan **resursprovidrar**. 
1. I listan över resursproviders, mot **Microsoft.Compute**väljer **registrera**. Du måste ha rollen deltagare eller ägare på prenumerationen för att registrera resursprovidern.

Mer detaljerade instruktioner finns i [resursproviders och resurstyper](../azure-resource-manager/resource-manager-supported-services.md).

### <a name="issue-azure-databricks-needs-permissions-to-access-resources-in-your-organization-that-only-an-admin-can-grant"></a>Problem: Azure Databricks måste ha behörighet att komma åt resurser i din organisation som bara en administratör kan bevilja.

#### <a name="background"></a>Bakgrund

Azure Databricks är integrerad med Azure AD. Detta kan du ange behörigheter i Azure Databricks (till exempel på bärbara datorer eller kluster) genom att ange användare från Azure AD. Det kräver läsbehörighet till den här informationen för Azure Databricks för att kunna visa namnen på användarna från din Azure AD. Detta kräver ett medgivande. Om medgivande inte redan är tillgänglig kan du se felet.

#### <a name="solution"></a>Lösning

Logga in som global administratör till Azure-portalen. För Azure Active Directory, går du till den **användarinställningar** fliken och kontrollera att **användare kan godkänna att appar får åtkomst till företagets data å deras vägnar** är inställd på **Ja**.

## <a name="next-steps"></a>Nästa steg

- [Snabbstart: Kom igång med Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Vad är Azure Databricks?](what-is-azure-databricks.md)


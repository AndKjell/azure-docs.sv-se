---
title: Förfallodatum för Office 365 grupper i Azure Active Directory | Microsoft Docs
description: Hur du ställer in förfallodatum för Office 365-grupper i Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 29a53101bff8c384d01f952c4498e09d9d970ee3
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841742"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Konfigurera en princip för Office 365-grupper

Genom att ange en princip för dem kan du nu hantera livscykeln för Office 365-grupper. Du kan ange princip för endast Office 365-grupper i Azure Active Directory (AD Azure). 

När du har en grupp att upphöra att gälla:
-   Ägare av gruppen får ett meddelande om att förnya gruppen som närmar sig förfallodatum
-   En grupp som inte förnyas tas bort
-   Alla Office 365-grupp som tas bort kan återställas inom 30 dagar av gruppägarna eller administratören

> [!NOTE]
> Konfigurera och använda en princip för Office 365-grupper måste du ha till hands Azure AD Premium-licenser för alla medlemmar i grupper som en princip tillämpas.

Information om hur du hämtar och installerar Azure AD PowerShell-cmdlets finns i [Azure Active Directory PowerShell för Graph 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Roller och behörigheter
Följande är roller som kan konfigurera och använda förfallodatum för Office 365-grupper i Azure AD.

Roll | Behörigheter
-------- | --------
Global administratör eller Användaradministratör för konto | Kan skapa, läsa, uppdatera eller ta bort principinställningar för Office 365 grupper upphör att gälla<br>Kan förnya någon Office 365-grupp
Användare | Förnya ett Office 365-grupp som de äger<br>Återställa en Office 365-grupp som de äger<br>Kan läsa förfallodatum principinställningar

Läs mer om behörigheter för att återställa en borttagen grupp [återställa en borttagen Office 365-grupp i Azure Active Directory](groups-restore-deleted.md).

## <a name="set-group-expiration"></a>Ange gruppförfallotid

1. Öppna den [Azure AD administratörscenter](https://aad.portal.azure.com) med ett konto som är en global administratör i Azure AD-klienten.

2. Välj **grupper**och välj sedan **upphör att gälla** att öppna inställningar för giltighetstid.
  
  ![Bladet upphör att gälla](./media/groups-lifecycle/expiration-settings.png)

4. På den **upphör att gälla** bladet kan du:

  * Ange gruppens livstid i dagar. Du kan välja någon av de förinställda värdena, eller ett anpassat värde (bör vara 31 dagar eller mer). 
  * Ange en e-postadress där förnyelse och förfallodatum meddelanden ska skickas när en grupp har ingen ägare. 
  * Välj vilka Office 365-grupper går ut. Du kan aktivera förfallotid för **alla** Office 365-grupper som du kan välja att aktivera endast **valda** Office 365-grupper, eller välj **ingen** att inaktivera upphör att gälla för alla grupper .
  * Spara dina inställningar när du är klar genom att välja **spara**.


E-postmeddelanden som den här skickas till Office 365-gruppägare 30 dagar, 15 dagar och 1 dag före förfallodatumet i gruppen.

![Meddelande om utgången e-post](./media/groups-lifecycle/expiration-notification.png)

Från den **förnya grupp** e-postmeddelande, gruppen ägare kan direkt komma åt gruppsidan i åtkomstpanelen. Där kan kan användarna få mer information om gruppen, till exempel en beskrivning, när den senast har förnyats, när den upphör och möjligheten att förnya gruppen. Gruppsidan nu innehåller också länkar till Office 365 gruppera resurser, så att gruppägare kan enkelt visa innehåll och aktivitet i gruppen.

När en grupp upphör att gälla, är gruppen bort en dag efter utgångsdatum. Ett e-postmeddelande som följande skickas till Office 365-gruppägare som informerar dem om förfallodatum och kommande borttagning av deras Office 365-grupp.

![Gruppen borttagning av e-postmeddelande](./media/groups-lifecycle/deletion-notification.png)

Gruppen kan återställas inom 30 dagar från dess borttagningen genom att välja **Återställ grupp** eller med hjälp av PowerShell-cmdletar, enligt beskrivningen i [återställa en borttagen Office 365-grupp i Azure Active Directory](groups-restore-deleted.md).
    
Om gruppen som du återställa innehåller dokument, SharePoint-webbplatser och andra beständiga objekt, kan det ta upp till 24 timmar att fullständigt återställa gruppen och dess innehåll.

> [!NOTE]
> * När du först konfigurerar upphör att gälla är grupper som är äldre än giltighetsintervallet inställda på 30 dagar kvar till förfallodatum. Första förnyelse-e-postmeddelandet skickas ut inom en dag. 
>   Till exempel grupp A skapades 400 dagar sedan och giltighetsintervallet är inställd på 180 dagar. När du tillämpar inställningar för giltighetstid har 30 dagar innan de tas bort i grupp A Om inte ägaren förnyar den.
> * För närvarande endast en princip kan konfigureras för Office 365-grupper på en klient.
> * När en dynamisk grupp tas bort och återställs, är det som en ny grupp och nytt fylls i enligt regeln. Den här processen kan ta upp till 24 timmar.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Hur Office 365 förfallodatum fungerar med en postlåda av juridiska skäl
När en grupp upphör att gälla och tas bort, sedan 30 dagar efter borttagningen gruppens data från appar som Planner, platser, eller team raderas, men grupp-postlådan som finns på bevarande av juridiska skäl finns kvar och tas inte bort permanent. Administratören kan använda Exchange-cmdlets för att återställa postlådan för att hämta data. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>Hur Office 365 förfallodatum fungerar med bevarandeprincip
Bevarandeprincipen konfigureras via Security and Compliance Center. Om du har ställt in en bevarandeprincip för Office 365-grupper, när en grupp upphör att gälla och tas bort, gruppkonversationer i grupp-postlåda och filer i grupp-platsen ska finnas kvar i behållaren kvarhållning för ett visst antal dagar som definierats i kvarhållning princip. Användarna ser inte gruppen eller dess innehåll efter förfallodatumet, men kan återställa platsen och postlåda data via e-discovery.

## <a name="powershell-examples"></a>PowerShell-exempel
Här följer exempel på hur du kan använda PowerShell-cmdletar för att konfigurera inställningar för giltighetstid för Office 365-grupper i din klient:

1. Installera PowerShell v2.0-förhandsversionsmodulen (2.0.0.137) och logga in i PowerShell-Kommandotolken:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Konfigurera inställningar för giltighetstid New-AzureADMSGroupLifecyclePolicy: den här cmdlet: en anger livslängden för alla Office 365-grupper i klienten och 365 dagar. Förnyelse av meddelanden för Office 365 grupper utan ägare kommer att skickas till 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Hämta den befintliga principen Get-AzureADMSGroupLifecyclePolicy: denna cmdlet hämtar de aktuella upphör att gälla inställningarna för Office 365-grupp som har konfigurerats. I det här exemplet ser du:
  * Princip-ID 
  * Livslängd för alla Office 365-grupper i klienten är inställd på 365 dagar
  * Förnyelse av meddelanden för Office 365 grupper utan ägare kommer att skickas till ”emailaddress@contoso.com”.
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Uppdatera den befintliga principen Set-AzureADMSGroupLifecyclePolicy: den här cmdleten används för att uppdatera en befintlig princip. I exemplet nedan ändras grupp livstid i den befintliga principen från 365 dagar till 180 dagar. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Lägga till specifika grupper i principen Add-AzureADMSLifecyclePolicyGroup: den här cmdleten lägger till en grupp i policyn för onlinelivscykeln. Som exempel: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Ta bort den befintliga principen Remove-AzureADMSGroupLifecyclePolicy: den här cmdleten tar bort förfallodatum för Office 365 gruppinställningar men kräver princip-ID. Detta inaktiverar förfallodatum för Office 365-grupper. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
Följande cmdletar kan användas för att konfigurera principen i detalj. Mer information finns i [PowerShell-dokumentationen](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure AD-grupper.

* [Visa befintliga grupper](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Hantera inställningar för en grupp](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Hantera medlemskap i en grupp](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](groups-dynamic-membership.md)
